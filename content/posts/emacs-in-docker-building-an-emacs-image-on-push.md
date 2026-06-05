+++
date = '2026-06-01T22:53:19+01:00'
draft = false
title = 'Emacs in Docker: Building an Emacs image on push'
+++

![Emacs Docker](../../images/emacs-docker.webp)

# Introduction

I want to have my text editing environment easily portable across environments.

In this situation Docker and GitHub Actions come to the rescue for
easily turning an application into a shareable, portable image.

The Emacs init code as well as its Dockerfile can be
found [here](https://github.com/sdobrau/.emacs.d).

With that in mind I set out for the following:

## Dockerfile

The `Dockerfile`:
* [x] Fetches the necessary build dependencies
* [x] Clones and builds Emacs with configuration flags
* [x] Cleans up build dependencies once building is done
* [x] Runs Emacs once with the pushed config and additional forms for setup.

```
FROM debian:sid as builder

# ... build ...

RUN emacs --batch -l early-init.el -l init.el --eval "(progn (treesit-auto-install-all) (ghostel-download-module))"
```

## GitHub Action

The GitHub workflow:
* [x] Fetches the init repository with the Dockerfile
* [x] Uses it to build and push the resulting Emacs image to the
Docker Hub registry.

```
- name: Build and push
  uses: docker/build-push-action@v7
  with:
    context: .
    push: true
    tags: ${{ secrets.DOCKER_USERNAME }}/emacs:latest
```

# End result

With this, whenever I update my Emacs configuration and do a push,
I have a ready-baked, portable Emacs image to use anywhere Docker is
available.

# DevSecOps: Attestation, Provenance, and image signing

Furthermore, in compliance with DevSecOps best practices, I follow
the following extra processes:

## Image signing using 'cosign'

I generate a key-pair specifically for signing my images, and sign
my latest image with my Cosign private key:

```
cosign generate-key-pair
cosign sign --yes serbandobrau/emacs:latest
```

Then other end-users can verify the signature using the
Cosign public key `cosign.pub`:

```
cosign verify --key cosign.pub serbandobrau/emacs:latest
```

This way we ensure the integrity of the image.

## Attestation: Extra rich metadata

Image attestation provides the image with detailed
metadata that helps ensure quality control.

It can be enabled by simply supplying `provenance: mode=max` to the
`build-push-action`:

```
- name: Build and push
  uses: docker/build-push-action@v7
  with:
    ...
    provenance: mode=max
    ...
```

The attestation metadata can then be checked via either
`docker scout` or `regctl`. More details can be found
[here](https://docs.docker.com/dhi/how-to/verify/#verify-image-attestations).

## SBOM: Scannable manifest of the image's contents

SBOM metadata can be obtained in two ways:
- Using `syft` to inspect the filesystem directly for packages
- Using SBOM metadata attached to the image during Docker build.

Getting SBOM with `syft` is as easy as passing it an image:

```
$ syft nginx:latest
 ✔ Loaded image                                                                nginx:latest
 ✔ Parsed image                    sha256:7aaca76c508f7d121ff29cbe9dd071012486d00c21e17655eb
 ✔ Cataloged contents              694fa3085e224b310dd07240e1d0969d3b94cb5d64d056f49fb3fcd04
   ├── ✔ Packages                        [152 packages]
   ├── ✔ Executables                     [831 executables]
   ├── ✔ File metadata                   [3,226 locations]
NAME                       VERSION                               TYPE
apt                        3.0.3                                 deb
base-files                 13.8+deb13u5                          deb
base-passwd                3.6.7                                 deb
bash                       5.2.37-2+b9                           deb
bsdutils                   1:2.41-5                              deb
...
```

Adding SBOM to the metadata directly can be done by just adding
`sbom: true` to the GitHub Action:

```
- name: Build and push
  uses: docker/build-push-action@v7
  with:
    ...
    sbom: true
    ...
```

This can then be pulled from the iamge fed to `trivy` for CVE scanning.

Here is an example used against alpine:3.15:

```
$ trivy image --format spdx-json --output result.json IMAGE
$ trivy sbom result.json
Report Summary

┌──────────────────────────────┬────────┬─────────────────┐
│            Target            │  Type  │ Vulnerabilities │
├──────────────────────────────┼────────┼─────────────────┤
│ result.json (alpine 3.15.11) │ alpine │        0        │
└──────────────────────────────┴────────┴─────────────────┘
```

More details about SBOM can be found [here](https://docs.docker.com/build/ci/github-actions/attestations/).

# Conclusion

Docker is the ideal platform for bundling applications with their
dependencies in an easy-to-share format. I hope with this post that
I encourage more people to use Docker. I have also learned how
to write a Dockerfile and Docker security best practices.
