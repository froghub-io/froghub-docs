---
title: "Command Reference"
date: 2022-8-8T11:02:05+06:00
lastmod: 2022-08-09T10:42:26+06:00
weight: 2
draft: false
keywords: ["cli"]
---

FrogHub CLI Command List
Welcome to the FrogHub CLI! This site provides online access to all help strings in the FrogHub CLI. For a more in-depth guide, please see our Getting Started guide on our main docs site.

If you have questions, ideas, or would like to contribute, check out the repository on GitHub.

Before you begin Make sure you have Node.js version 12.20.0, 14.14.0, 16.0.0, or later.

Install the CLI

To install the CLI, pop open your terminal and install with npm.

```shell
npm install froghub-cli -g
```

Important: When using the CLI in a CI environment we recommend installing it locally. See more here.

Listing commands

To get a list of commands, run:

```shell
froghub help
```

To get a list of available sub-commands, arguments, and flags, run:

```shell
froghub [command] help
```