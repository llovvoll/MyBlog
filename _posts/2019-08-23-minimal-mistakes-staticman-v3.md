---
layout: single
title: "How to Enable Staticman v3 on Minimal Mistakes"
date: 2019-08-23
modified:
description:
categories:
    - "Tech Article"
tags:
    - Staticman
    - Blog
header:
---

Hey I came back to write the blog but I found out that my staticman has failed. I checked the document but I have been unable to enable it for a long time. So I chose to use [Vincent Tam](https://github.com/VincentTam)'s staticman to get the job done you can refer to my method to make it work.

## Invite staticmanlab as a collaborator to your repository

[Staticman comments](https://github.com/daattali/beautiful-jekyll/#staticman-comments)

To use Staticman, you first need to invite staticmanlab as a collaborator to your repository (by going to your repository Settings page, navigate to the Collaborators tab, and add the username staticmanlab), and then accept the invitation by going to https://staticman3.herokuapp.com/v3/connect/github/<username>/<repo-name>. Lastly, fill in the staticman parameters in the Staticman section of _config.yml. You may also choose a different Staticman instance other than staticmanlab.

![]({{ site.url }}/assets/images/2019/08/23/2019-08-23-001.png)

## Add endpoint in _config.yml

```yaml
# _config.yml
repository  : # Git username/repo-name e.g. "mmistakes/minimal-mistakes"
comments:
  provider  : "staticman_v2"
  staticman:
    branch    : "master"
    endpoint  : https://staticman3.herokuapp.com/v3/entry/github/
```
When you are done, I think staticman is already working. If you need turn on reCaptcha you should going to http://staticman3.herokuapp.com/v3/encrypt/<your SECRET KEY> encrypt your secret key and add to _config.yml and staticman.yml

```yaml
# _config.yml
reCaptcha:
  siteKey                : ""
  secret                 : ""
```

```yaml
# staticman.yml
  reCaptcha:
    enabled: true
    siteKey: ""
    secret: ""
```

When you're done, your staticman goes back to your blog and starts working.