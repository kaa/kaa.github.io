---
layout: post
title: "Batch updating Azure Storage blob properties in bash"
description: "Update blob properties with nothing but bash and the azure cli tool"
category: 
tags: []
---

Today I realized that I'd left out setting the content type on a bunch of blobs that I uploaded over a number of days. My first thought was just to multi-select blobs in Azure Storage Explorer and be done with it, but alas it doesn't allow batch updates. It looked more and more like I'd be forced to dvelve into Powershell to fix it - the horror!

Fortunately it turned out that the Azure CLI tool combined with the usual Unix toolbelt is flexible enough to get me out of this bind all by itself.

First set the connection string in an environment variable so you don't have to keep wielding it around.

```bash
% az login
% export AZURE_STORAGE_CONNECTION_STRING=$(az storage account show-connection-string -g <resource-group> -n <storage-account> --output tsv)
```

Then the monster

```bash
% az storage blob list -c <container-name> --prefix <prefix> -o tsv \
 | awk '{print $3}' \
 | grep "\.jpg"\
 | xargs -n1 az storage blob update --content-type "image/jpeg" -c cdn -n
```

Begin by listing all blobs matching _prefix_ in container _container-name_, selecting to output as tab separated values using `az storage blob list -c <container-name> --prefix <prefix>`. We pipe this to `awk` in order to select only the third (name) column and use `grep` to select only lines ending in `.jpg`. Finally we use `xargs -n1` to pass each of the matching lines to `az` in order to set the `content-type` property to `image/jpg` for each of the blobs. 