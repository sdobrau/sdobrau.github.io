+++
date = '2026-05-12T19:59:17+03:00'
draft = false
title = 'Goc: Git Cloner in Go'
+++

![Go concurrency](../../images/go-concurrency.webp)

# Introduction

In order to learn Go and practice its concurrency primitives I've
set out to write a small Git cloner with concurrency support.

Its features are:

* [x] Clone users or groups, organisations' repositories
* [x] Parallelism: as many as N concurrent pulls/clones
* [x] If repository already present, pull
* [x] Forges supported:
  * [x] GitHub
  * [x] GitLab (including instances)
  * [x] Gitea (including instances)
* [x] Tokens (taken from envvars `GITHUB|GITLAB|GITEA_TOKEN` or
      provided per-user/organisation)
* [x] YAML file for declaring what and how to clone (use with -F):
  * [x] Ignore forks
  * [x] Ignore with stars lower than a specific amount
  
Example YAML file:

```yaml
- forge: github
  users:
    - name: abo-abo
	- name: sdobrau
- forge: gitea
  users:
    - name: mayx
```

Details and code can be found at my repository [here](https://github.com/sdobrau/goc#).

# Concurrency

The concurrency is implemented using `sync.WaitGroup`s, `Goroutine`s,
`Channel`s and a `Worker Pool`:

A configurable amount of workers (-t flag) is spun up first, as
workers need to poll the `0` size channel to avoid blocking:

```go
var repositoryWithDirChan = make(chan RepositoryWithDir)
	for i := 0; uint(i) < goroutines; i++ {
		wg.Go(func() {cloneOrPullWorker(&wg, repositoryWithDirChan)})
	}
```

Each `cloneOrPull` worker listens for repositories on the
`repositoryWithDirChan` channel:

```go
func cloneOrPullWorker(wg *sync.WaitGroup, repositoryWithDirChan <-chan RepositoryWithDir) {
	for repo := range repositoryWithDirChan {
		// processing...
```
	
When processing a fetched repository list, send repositories
(`RepositoryWithDir` struct) to the channel

```go
repositoryWithDirChan <- RepositoryWithDir{Repository: repository, Directory: repoDir}
```

Thus repositories are sent to workers as they are available. When all
workers are occupied cloning or pulling the main loop is blocked until
a worker is free, then a repository is sent as normal.

# Conclusion

Go is known for its friendly concurrency primitives. I hope that
with this post I will convince more readers to give Go a try.

See these posts for more information:

[Synchronizing Go Routines with Channels and WaitGroups](https://dev.to/sophiedebenedetto/synchronizing-go-routines-with-channels-and-waitgroups-3ke2)

[Buffered Channels and Worker Pools](https://golangbot.com/buffered-channels-worker-pools/)
