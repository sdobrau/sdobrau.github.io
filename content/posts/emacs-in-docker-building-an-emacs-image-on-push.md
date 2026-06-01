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

# Conclusion

Docker is the ideal platform for bundling applications with their
dependencies in an easy-to-share format. I hope with this post that
I encourage more people to use Docker. I have also learned how
to write a Dockerfile and to take minimum dependencies into
consideration when bundling for Docker.
