---
layout: post
title: Escaping Characters in Google Credentials
author: FrozenFOXX
tags:
 - shell
 - devops
---

Sometimes you'll need to translate Google credential JSON files into environment variables. This can be a bit of a problem because there's multiple values to escape but there's a few ways to get it done. I've got a few here I've used that can be taken on their own or combined in several circumstances.

## Base64

The simplest of course is _base64_ encoding. On Linux and MacOS it's done like this:

``` shell
$ base64 ~/.gcloud/user@google-project.json
AAAAAAAAAAAAAAAAAAAAAAAAAAA
BBBBBBBBBBBBBBBBBBBBBBBBBBB
CCCCCCCCCCCCCCCCCCCCCCCCCCC
```

Problem is that'll give you carraige returns. What you REALLY want is to disable wrapping:

``` shell
$ base64 -w 0 ~/.gcloud/user@google-project.json
AAAAAAAAAAAAAAAAAAAAAAAAAAABBBBBBBBBBBBBBBBBBBBBBBBBBBCCCCCCCCCCCCCCCCCCCCCCCCCCC
```

Much better.

## Carriage Returns

Suppose you've got your file:

``` shell
$ cat ~/.gcloud/user@google-project.json
{
  "type": "service_account",
  "project_id": "google-project",
  "private_key_id": "AAAAAAAAAAAAAAAAAAAAAAA",
  "private_key": "BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB",
  "client_email": "user@google-project.iam.gserviceaccount.com",
  "client_id": "CCCCCCCCCCCCCC",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://oauth2.googleapis.com/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/user%40google-project.iam.gserviceaccount.com"
}
```

Great, but small problem. It's got a whole lot of carriage returns. How can you get rid of them quickly and effectively?

``` shell
$ cat ~/.gcloud/user@google-project.json | tr -d '\r\n'
{  "type": "service_account",  "project_id": "google-project",  "private_key_id": "AAAAAAAAAAAAAAAAAAAAAAA",  "private_key": "BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB",  "client_email": "user@google-project.iam.gserviceaccount.com",  "client_id": "CCCCCCCCCCCCCC",  "auth_uri": "https://accounts.google.com/o/oauth2/auth",  "token_uri": "https://oauth2.googleapis.com/token",  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/user%40google-project.iam.gserviceaccount.com"}
```

## Double Quotes

Better, but some shells will choke on the double quotes. So we can go one step further.

``` shell
$ cat ~/.gcloud/user@google-project.json | tr -d '\r\n' | sed 's/\"/\\"/g'
{  \"type\": \"service_account\",  \"project_id\": \"google-project\",  \"private_key_id\": \"AAAAAAAAAAAAAAAAAAAAAAA\",  \"private_key\": \"BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB\",  \"client_email\": \"user@google-project.iam.gserviceaccount.com\",  \"client_id\": \"CCCCCCCCCCCCCC\",  \"auth_uri\": \"https://accounts.google.com/o/oauth2/auth\",  \"token_uri\": \"https://oauth2.googleapis.com/token\",  \"auth_provider_x509_cert_url\": \"https://www.googleapis.com/oauth2/v1/certs\",  \"client_x509_cert_url\": \"https://www.googleapis.com/robot/v1/metadata/x509/user%40google-project.iam.gserviceaccount.com\"}
```
