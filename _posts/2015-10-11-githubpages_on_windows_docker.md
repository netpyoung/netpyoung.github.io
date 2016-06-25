---

layout: post
title: '윈도우즈 도커를 이용하여 깃허브 페이지 테스트하기'
tags: docker jekyll

---

# Github Pages on Windows Docker


& MacBook이 있기에, Windows에서 삽질을 하지 않아도 되지만... Docker에 익숙해지고, Windows에서의 Docker활용하면서 삽질을 미리 경험해보고자... 삽을 푸기로 했다. 사실 이 문서를 보게될 사람 역시 (아마도) 삽질매니아 인가보다.


![](http://www.newsm.com/news/photo/200812/1106_2264_4956.jpg)

## Github Pages란?
* github 저장소를 이용하여, github.io 도메인으로 정적웹사이트 서비스를 제공한다.
* 스테틱 파일인, html 자체를 이용할 수 있을 뿐만 아니라, github에서 제공해주는 Jekyll 정적 블로그엔진을 이용할 수 있다.
* ref:
 - [Jekyll, Git 을 몰라도 무료 Github Pages 즐기기](http://ilmol.com/2015/01/Jekyll,Git%20%EC%9D%84%20%EB%AA%B0%EB%9D%BC%EB%8F%84%20%EB%AC%B4%EB%A3%8C%20Github%20Pages%20%EC%A6%90%EA%B8%B0%EA%B8%B0.html)
 - [GitHub의 페이지 기능 이용하기](https://dogfeet.github.io/articles/2012/github-pages.html)
 - https://pages.github.com/versions/
 - https://help.github.com/articles/using-jekyll-with-pages/#installing-jekyll

## Jekyll이란?
![](https://jekyllrb.com/img/logo-2x.png)

* <http://jekyllrb.com/>
* 정적 웹 블로그 프레임워크로, Ruby로 짜여졌다.
* Markdown, Liquid, HTML & CSS을 template제공하여, 렌더링할 수 있게 도와주며 프레임워크인 만큼 여러 편의 기능들이 내장되어 있다.



## 문제는?
일단 Jekyll을 Windows에서 그냥 실행시키려고 하면, (아마도)gem관련 오류 및 한글 encoding관련 문제에 봉착할 것이다.
 비단, Ruby든 Python이든 Windows에서 라이브러리 호환성 및 한글 인코딩 문제는 빠질 수 없다(삽질 필요).
 [Windows 에서 Jekyll](http://jekyllrb-ko.github.io/docs/windows/)에서 소개한 http://jekyll-windows.juthilo.com/ 사이트 역시 (아마도) 어느정도만 해결하고 걸렸던게 있던걸로 기억한다.



## 해결책은?
(Docker를 활용하여) Windows에 Linux환경을 만들면 된다.



## Docker란?
![](https://www.docker.com/sites/all/themes/docker/assets/images/logo.png)

* An open platform for distributed applications for developers and sysadmins
* ref:
 - [가장 빨리 만나는 도커(Docker) 출간 및 원고 공개](http://pyrasis.com/private/2014/11/30/publish-docker-for-the-really-impatient-book)
 - [도커(Docker) 튜토리얼 : 깐 김에 배포까지](http://blog.nacyot.com/articles/2014-01-27-easy-deploy-with-docker/)

### Windows에 Docker 설치
* <https://docs.docker.com/installation/windows/>

1. 가상화 확인
2. Docker Toolbox 설치
3. Docker Toolbox 실행.

```
## open port
docker@default:~$ echo 'export DOCKER_HOST=tcp://$(boot2docker ip 2>/dev/null):8000' >> ~/.bash_profile

## install image
docker@default:~$ docker search ubuntu
docker@default:~$ docker pull ubuntu:latest

## run image
docker@default:~$ docker run -it --name jekyll ubuntu



##########################################################################
# install needed package
root@33b5f808449d:/# apt-get update
root@33b5f808449d:/# apt-get install -y git
root@33b5f808449d:/# apt-get install -y curl
root@33b5f808449d:/# apt-get install -y node
root@33b5f808449d:/# apt-get install -y python-pygments
root@33b5f808449d:/# apt-get clean
root@33b5f808449d:/# rm -rf /var/lib/apt/lists/

## install rvm
root@33b5f808449d:/# gpg --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3
root@33b5f808449d:/# \curl -sSL https://get.rvm.io | bash -s stable
root@33b5f808449d:/# source /etc/profile.d/rvm.sh
root@33b5f808449d:/# rvm list known
root@33b5f808449d:/# rvm install 2.2
root@33b5f808449d:/# rvm use 2.2

## install github-pages
root@33b5f808449d:/# gem install github-pages

root@33b5f808449d:/# exit
##########################################################################


docker@default:~$ docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS                    NAMES
33b5f808449d        ubuntu              "/bin/bash"         About an hour ago   Up About an hour    0.0.0.0:8000->8000/tcp   jekyll

## backup image
docker@default:~$ docker commit 33b5f808449d github-pages
docker@default:~$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             VIRTUAL SIZE
github-pages        latest              3ccaea30ec80        About an hour ago   641.5 MB
ubuntu              latest              cdd474520b8c        45 hours ago        188 MB

## set volum from VirtualBox
docker@default:~$ docker run -it --name jekyll -p 8000:8000 -v /c/Users/pyoung/hello/netpyoung.github.io/:/site github-pages


##########################################################################
root# cd /site
root# jekyll serve --port 8000 --watch --force_polling
##########################################################################
```



## 주저리.
사실 이 글을 작성하기 시작한건 꽤 되었는데, 그동안 Boot2Docker가 공식 메뉴얼에서 deprecate되었다.. 대충 기능은 비슷하고 docker-container로 변경된점?
게임회사에서 기능별로 브랜치까고, 서버 테스트를 할때 공유서버가 아닌, 기능 담당자별 서버가 필요한데. 배포나 설치하는걸 docker로 하면 그냥 간단하게 될것같다.
쩝 지금있는데는 뭐 한쪽은 svn쓰고, 한쪽은 git을 svn처럼 활용하고 있는데 뭐 더 할말이 있겠냐만은..
음.. 이제 좀 포스트하는데 편해졌다.

## 추가.
* docker github-pages로 쉽게 테스트 해볼 수 있게 되었다. <https://github.com/Starefossen/docker-github-pages>
