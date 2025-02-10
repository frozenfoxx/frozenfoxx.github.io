---
title: Generating Kibana Keys
tags:
  - tools
  - devops
  - elk
---

[Kibana](https://www.elastic.co/kibana) is a visualization and exploration tool for ElasticSearch, commonly used with an ElasticSearch, Logstash, Kibana (ELK) stack. It has a variety of security features and that means sometimes having to generate security keys. Fortunately it has some tools to help with this. Run the following command:

```
bin/kibana-encryption-keys generate
```

This will by default generate encryption keys, then you just need to put them in your `kibana.yml` or environment variables.
