---
title: Generating Docker Registry Strings
tags:
  - tools
  - devops
  - docker
  - security
---

Docker Registry uses `htpasswd` strings for authentication. To generate a string to place in the `registry.password` file, run the following:

```
docker run -it --rm --entrypoint htpasswd registry:2.7.0 -Bbn [username] [password plaintext]
```

Simply take the string and put it in the `registry.password` file.
