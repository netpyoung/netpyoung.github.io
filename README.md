---
layout: default
title: "netpyoung.github.io"
---

netpyoung.github.io
===================
>
넷평남짓한공간

# using docker
```bash
docker-compose up
```


```bash
alias homepage='cd ~/github/netpyoung.github.io/ && docker run -v "$PWD":/usr/src/app -p "4000:4000" starefossen/github-pages'
```


``` bash
jekyll serve --port 8000 --watch --force_polling
```

# TODO
* search : <http://jekyll.tips/tutorials/search/>
* comment : facebook
* essay, tech blog division


# ref
* <http://netpyoung.github.io/blog/netpyoung.github.io_on_jekyll>
* <https://pages.github.com/versions/>
* <https://help.github.com/articles/configuring-jekyll/>
* <http://kramdown.gettalong.org/quickref.html>
* <https://github.com/Starefossen/docker-github-pages>
