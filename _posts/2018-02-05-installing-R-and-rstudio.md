---
layout: post
title: Installing R and Rstudio in Ubuntu 16.0.4
gh-repo: rizwan-abro
gh-badge: [star, fork, follow]
---

**R Mirror**

First we need to add a mirror of R server, open your terminal and write the following command:

```
$ sudo nano /etc/apt/sources.list
you can edit it in any editor, i used nano.
```

paste this server link:

```
deb http://mirrors.psu.ac.th/pub/cran//bin/linux/ubuntu xenial/
```

now run update command:

```
$ sudo apt-get update
```

now download the R:

```
sudo apt-get install r-base
```

when download complete, you can start R by simply typing R in the terminal



**Installing R Studio**

You need to download the R studio from this [link](https://download1.rstudio.org/rstudio-xenial-1.1.419-amd64.deb) 

{: .box-note}
**Note:** This is 64 bit version.

Install the downloaded Rstudio, after installing you need run following commands for R server:

~~~
$ sudo apt-get install gdebi-core
$ wget https://download2.rstudio.org/rstudio-server-1.1.419-amd64.deb
$ sudo gdebi rstudio-server-1.1.419-amd64.deb
~~~

You can open Rstudio from terminal by typing rstudio