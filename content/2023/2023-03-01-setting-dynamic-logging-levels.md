---
layout: post
title: Setting Dynamic Logging Levels
author: FrozenFOXX
tags:
 - ghactions
 - github
 - devops
---

There's often a need to crank up logging levels when running applications for debugging, but in CICD workflows this can be difficult since you don't normally want to slam your logs with lots of output all the time. Being able to flexibly adjust your logs for a run is a great tool and GitHub Actions is up to the task.

Here's a quick way to do this for Packer:
```shell
---
name: Packer Logging Example

on:
  workflow_dispatch:
    inputs:
      packerLogLevel:
        description: 'Packer logging level'
        required: false
        default: 0
        type: integer

jobs:
  image-build:
    name: Build an Image
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout Repository
        uses: actions/checkout@v3

      - name: Setup Packer
        uses: hashicorp/setup-packer@main
        id: setup
        with:
          version: "1.8.3" # or `latest`

      - name: Init Packer
        id: init
        run: "packer init ."
        shell: bash
        env:
          PACKER_GITHUB_API_TOKEN: ${{ secrets.PACKER_GITHUB_API_TOKEN }}

      - name: Run Packer
        run: "packer build -only=\"image.pkr.hcl\" ."
        shell: bash
        working-directory: .
        env:
          PACKER_GITHUB_API_TOKEN: ${{ secrets.PACKER_GITHUB_API_TOKEN }}
          PACKER_LOG: ${{ inputs.packerLogLevel }}
```

This will set the default value of the input to `0` when not modified which means Packer runs at the normal level of logging. If you need more, when clicking the `workflow_dispatch` you can specify `1` which will give significantly more logging info but only for that run.
