---
layout: post
title:  "How to setup different SSH keys for multiple hosts"
date:   2021-04-29 11:39:00 +0100
categories: programming 
---

I often use different laptops to write code for projects hosted on either GitHub, GitLab, or
CodeCommit.
In other words, there's a many-to-many relation between my laptops and the code hosts.

I also want to use a different SSH key for each code host, and perhaps a specific key pair to
log into some EC2 instances, without having to remember or specify which key to use each time.

The solution is to create the appropriate SSH keys, in example:

```
ssh-keygen -f id_ed25519_github -t ed25519 -C "<comment-or-email-address>"
```

and then edit `.ssh/config` in a way like this:

```
$ cat ~/.ssh/config

Host git-codecommit.*.amazonaws.com
  User <your-user-id-here>
  IdentityFile ~/.ssh/codecommit_rsa

Host *.amazonaws.com
  User ec2-user
  Identityfile ~/.aws/EC2LinuxKeyPair.pem

Host github.com
    User git
    Hostname github.com
    PreferredAuthentications publickey
    IdentityFile ~/.ssh/id_ed25519_github
```

