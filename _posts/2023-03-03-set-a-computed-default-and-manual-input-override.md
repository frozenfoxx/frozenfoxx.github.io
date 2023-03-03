---
layout: post
title: Set a Computed Default and Manual Input Override
author: FrozenFOXX
tags:
 - ghactions
 - github
 - devops
---

Sometimes we want a default value for a Workflow to be computed, such as a date, but also the ability to adjust it as needed. Let's explore an example of how to do this:

```shell
---
name: Computed Default and Manual Override

on:
  workflow_dispatch:
    inputs:
      date:
        description: 'Date Override MMDDYYYY'
        required: false
        default: ''
        type: string

jobs:
  get-date:
    if: ${{ !inputs.date }}
    runs-on: ubuntu-latest
    name: Get date
    outputs:
      date: ${{ steps.get-date.outputs.DATE }}

    steps:
      - name: Get the date
        id: 'get-date'
        run: echo "DATE=$(date +'%m%d%Y')" >> $GITHUB_OUTPUT

  build-image:
    if: always() && !contains(needs.*.result, 'failure')
    needs: get-date
    name: Build an image
    runs-on: ubuntu-latest

    steps:
      - name: Init Packer
        id: init
        run: "packer init ."
        shell: bash
        env:
          PACKER_GITHUB_API_TOKEN: ${{ secrets.PACKER_GITHUB_API_TOKEN }}

      - name: Run Packer
        id: run
        run: "packer build -only\"image-profile.provider.image\" ."
        env:
          IMAGE_NAME: "profile-${{ inputs.date || needs.get-date.outputs.date }}.img"
          PACKER_GITHUB_API_TOKEN: ${{ secrets.PACKER_GITHUB_API_TOKEN }}
```

If a date is supplied to `inputs` then the `get-date` job will not need to exist. Since it's listed as a `need`, that will make that fail for this job so we use `always()` to get around that. However, we ALSO want it to fail if that job DOES exist because no override was supplied and something goes wrong (or if any other needs are included and fail!) so we add the `!contains(needs.*.result, 'failure')` which translates to "none of the `needs` have failed."

This way you can easily have additional `needs` yet still use ones that may not exist depending on their execution which also allows you to override them.
