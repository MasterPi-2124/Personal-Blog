---
# Name of the article
title: "Every Problem With Running Node"

# Quick description
description: 

# Author of the article
author: Master Pi

# Appears as the tail of the output URL.
slug: "every-problem-with-running-node"

# Date created
date: 2021-10-10T20:25:54+07:00

# Date published. Before that day, the post can not be available
publishDate: 

# Daye expired. After that day, the post can not be available
expiryDate:

# Last modified time of the file
lastmod: 
    - :fileModTime
    - :git
    
# Article's tags
tags: 
    - node
    - validator

# Article's categories: Blog, Project or Guideline
categories:
    - Project


# Allow share?
socialShare: true

# Useful to link articles together for "See also" part
series: Notional

# is Math included? Default: false
math: false

# Cover image of the article
image: 

# License. Default: CC BY-NC-SA 4.0
license:

---

Here are every error I got when trying to run a full node. For difference between nodes and validators, see other posts in my blog.

# Common step in every node
Below are the common setup for all node:
- Clone from a remote repository
- Checkout a version. This step is really important and is the most difficult step because you don't know which version can run or not. Most problems occur can be fixed by change the version, which is only by try-and-fail.
- Install with `go install` or `make install`. If you want to reinstall after a failed set up, try with `make clean install`.
- Initialize the node name: `chaind init moniker --chain-id network`.
- Replace the **genesis.json** created in step above with the genesis file from the repo.
- Set up the ports:
    - RPC:
    - gRPC:
    - P2P:
    - API: 
    - NODE:
    - PEERS: 
- Start the node with `chaind start`.

# Problem #1: 