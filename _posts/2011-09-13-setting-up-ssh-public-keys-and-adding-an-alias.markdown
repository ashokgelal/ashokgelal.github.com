---
author: admin
comments: false
date: 2011-09-13 05:48:45+00:00
layout: post
slug: setting-up-ssh-public-keys-and-adding-an-alias
title: Setting up ssh public keys and adding an alias
wordpress_id: 79
categories:
- scripting
tags:
- remote server
- scripting
- ssh
---

This post is sort of a 'note-to-myself' post. For my Computer Science class, I have to do a lots of ssh-ing into remote host - my university server . I got tired of typing username and password every time I try to connect from home (using my MacBook Pro). I've seen people using an alias without even typing a username, password, and a hostname. I've once set up the ssh keys on my HP laptop but after I sold the laptop (I wiped out everything before selling :),  I've been lazy setting up a keys; no more.





Here are the steps I did.





Let suppose our remote server (in my case, university computer) is called **_server.universityname.edu_**, your usual username is **_foouser_**, and you want to give an alias **_sshserv _**to connect to the remote server using ssh.







  1. Create a local ssh key on your local computer





`$ ssh-keygen -t -rsa
`
There are other algorithms you can use to create your ssh key. I'm using _rsa_ for this tutorial. ssh key algorithms and secure algorithms are too broad to be covered in this tutorial.





Anyway, you should have a _id_rsa.pub_ file inside _~/.ssh_ folder. Do





`$ ls ~/.ssh/
`
and make sure you have a _id_rs.pub_ file







  1. Copy the public file to your remote server:





`$ scp ~/.ssh/id_rsa.pub foouser@server.universityname.edu:.ssh/authorized_key_local
`
You will be prompted for password. Use your usual _ssh_ password.







  1. Log in to your remote server to manage the key:





`$ ssh foouser@server.universityname.edu
`
Again, you will be prompted for a password.







  1. Concatenate the contents of _~/.ssh/authorized_key_local_ to _~/.ssh/authorized_keys_:





`$ cat ~/.ssh/authorized_key_local >> ~/.ssh/authorized_keys
`
5. Remove the _~/.ssh/authorized_key_local file_ which is not required anymore:





`$ rm ~/.ssh/authorized_key_local
`
6. Logout from the remote server:





`$ exit
`
7. You are now set so that you don't need the password every time you ssh into your remote server. Verify this by:





`$ ssh foouser@server.universityname.edu
`
At this point, you shouldn't be asked for any password. If you are prompted for a password, please make sure your properly followed this tutorial up to this point.







  1. If you are in your remote server, exit so that you are on local computer:





`$ exit
`
Now, let's setup an alias.







  1. Edit your _~/.ssh/config_ file to add the following. If the file doesn't exist, create it first.





_#The Host is the alias that you want to use_





_Host=myremoteserv_





_#The Hostname is the real hostname of the remote server_





_Hostname=server.universityname.edu_





_#The user is your username_





_User=foouser_











  1. Save the file and try:





`$ ssh myremoteserv
`
You should be able to login to your remote server.







  1. If you want to save 3 keystrokes every time you ssh using above command, you can add an alias for the whole command in your _~/.bash_profile_ file:





`alias sshserv="ssh myremoteserv"
`
4. To use the alias 'sshserv', either restart the terminal or update your bash profile:





`$ source ~/.bash_profile
`
5. Now, you can login to your remote server by just using:





`$ sshserv
`
Happy ssh-ing!



