---
layout: post
title: Using Dynamic Values for Matrix Variables
author: FrozenFOXX
tags:
 - ghactions
 - github
 - devops
---

There are many times when you wish to using a dynamic value such as a variable or a secret as an input for a later job, typically as part of a [Matrix Variable](https://docs.github.com/en/actions/using-jobs/using-a-matrix-for-your-jobs). However, one constraint of a matrix variable is that its values must be *static* and therefore GitHub Actions will complain if you try to have it use a dynamic lookup or the direct result of a lookup.

However, there's a way around this using [job output](https://docs.github.com/en/actions/using-jobs/defining-outputs-for-jobs). The catch is, it can't be just any job output but must be JSON explicitly. Fortunately, [fromJSON()](https://docs.github.com/en/actions/learn-github-actions/expressions#fromjson) is now supported and can let work with this limitation.
```Shell
---
name: Dynamic Value Conversion for Matrix

on:
  workflow_dispatch:

jobs:
  retrieve-versions:
    runs-on: ubuntu-latest
    name: Retrieve and output version groups
    outputs:
      versions-2020: ${{ steps.versions.outputs.VERSIONS_2020 }}
      versions-2021: ${{ steps.versions.outputs.VERSIONS_2021 }}

    steps:
      - name: Install jq
        id; 'jq'
        run: sudo apt-get install -y jq

      - name: Output JSON of Versions
        id: 'versions'
        run: |
          echo "VERSIONS_2020=$(echo '"${{ vars.VERSIONS_2020 }}"' | jq -c 'split(" ")')" >> $GITHUB_OUTPUT
          echo "VERSIONS_2021=$(echo '"${{ vars.VERSIONS_2021 }}"' | jq -c 'split(" ")')" >> $GITHUB_OUTPUT

  parse-2020:
    if: !contains(needs.*.result, 'failure')
    needs: [ retrieve-versions ]
    name: Parse Versions (2020)
    runs-on: ubuntu-latest
    strategy:
      matrix:
        versions: ${{ fromJSON(needs.retrieve-versions.outputs.versions-2020) }}

      - name: Parse a version
        run: |
          echo "Current version: ${VERSION}"
        shell: bash
        env:
          VERSION: ${{ matrix.versions }}

  parse-2021:
    if: !contains(needs.*.result, 'failure')
    needs: [ retrieve-versions ]
    name: Parse Versions (2021)
    runs-on: ubuntu-latest
    strategy:
      matrix:
        versions: ${{ fromJSON(needs.retrieve-versions.outputs.versions-2021) }}

      - name: Parse a version
        run: |
          echo "Current version: ${VERSION}"
        shell: bash
        env:
          VERSION: ${{ matrix.versions }}
```

In `retrieve-versions` it sets two outputs, `versions-2020` and `versions-2021` that it got from looking them up in GitHub Variables for the repo. However, it converts them using `jq` into JSON lists which is what Matrix variables require to process as a `Sequence`.

Once this is done, we can set the `strategy` to `matrix` and supply a list of versions, creating two branches of job from different version lists. Each will then dynamically create a set of jobs to correspond to the list of versions and output them.
