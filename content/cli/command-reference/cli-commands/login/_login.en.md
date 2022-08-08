---
title: "login"
date: 2022-8-8T11:02:05+06:00
lastmod: 2022-08-09T10:42:26+06:00
weight: 2
draft: false
keywords: ["cli"]
---

Build on your local machine

### Usage

```shell
froghub build
```

### Flags

* `context` (string) - Build context
* dry (boolean) - Dry run: show instructions without running them
* offline (boolean) - disables any features that require network access
* debug (boolean) - Print debugging information
* httpProxy (string) - Proxy server address to route requests through.
* httpProxyCertificateFilename (string) - Certificate file to use when connecting using a proxy server

#### Examples

```shell
froghub build
```