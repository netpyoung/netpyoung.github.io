---

layout: post
title: 'netpyoung.github.io 탄생'
tags: talk jekyll docker

---


netpyoung.github.io on jekyll
=============================

  개인적으로 생각하기에, 개발자가 자신의 블로그를 갖고 자신의 경험(삽질)을 공유해주는걸 멋있다고 생각하기에 블로그를 시작하게 되었다.
 그전까지는, http://netpyoung.springnote.com 를 이용해 단순 정리노트 , http://netpyoung.tistory.com 를 이용 내용 정리를 하였는데,
 springnote의 운영중단 및 tistory의 한계를 느끼는 터에 여기에 자리잡게되었다.
 개발일을 하다보면, 구글이 친구이며, stackoverflow와 reddit이나 facebook그룹이나 각종 포럼에서 글을 섭렵하기 마련이다.
 뭔가 끌리는 아이디어, 정리할 내용이 있으면 notepad++을 꺼내들고 살며시 저장 백업을 한다. evernote가 이러한 역활에는 맞아떨어지지만 웬지 모르게 거부감이 들어 아직까지도 evernote는 잘 안쓰고 있다.
 markdown으로 블로그를 작성할 수 있는 서비스를 찾았고, https://www.tumblr.com/ 가 대안으로 떠올랐지만,
 http://dogfeet.github.io/ 와 http://spoqa.github.io/ 를 보면서, github pages를 알게되었고, 그것이 제공해주는 기능에 더 끌렸다.


Github Pages는 github.io에 정적 호스팅을 할 수 있는 서비스이며, 기반 엔진으로 jekyll을 이용한다.
 완전히 옮겨가기 전에 한번 jekyll bootstrap으로 시도해봤다가 이도 저도 않되었으나,
 jekyll 처음부터 구축 다행이도 이때쯤에는 싸이트 문서화가 어느정도 되어있어서 비교적 쉽게 옮길 수 있었다.

다만, ruby에는 익숙했지만 jekyll에는 익숙치 않아서 문서를 뒤져가며 삽질.
 디자인에 영 꽝이여서 bootstrap을 이용하기로 결정하였고, 아이콘은 font awsome이용하기로 하였다.
 web front-end 개발자가 아니여서 살작 애를 먹긴 했으나 뭐 이정도 쯤이야.
 마크다운 엔진은 github가 쓰는 redcarpet을 사용하기로 결정.

옮긴 이후에, clojure로 작성된 https://github.com/cryogen-project/cryogen 역시 심각히 고려해봤지만,
 github내의 편집 용의성, jekyll이 보다 빠른 실행시간을 보여줘서 jekyll로 정착하기로 했다.

 음 되돌아보니, 2013.07.03부터 옮겼구나. jekyll bootstrap없이 생짜로 올리기 시작하면서 삽질한 내용, 참고 링크를 첨부하면서 이 포스팅을 계속 업데이트해 나가겠다.

--------------------------------------------

## jekyll
* 홈페이지:  http://jekyllrb.com/
* 일반 텍스트 문서를 정적 웹사이트나 블로그로 변환시켜줌.
* UTF8 no BOM으로 문서를 작성해야 한다.
* 참고:
 - jekyll wiki: https://github.com/mojombo/jekyll/wiki
 - jekyll tutorial : http://www.andrewmunsell.com/blog/ultimate-jekyll-tutorial/


## jekyll bootstrap
* 홈페이지: http://jekyllbootstrap.com/
* JB api: http://jekyllbootstrap.com/api/bootstrap-api.html
* jekyll을 보다 쉽게 사용할 수 있도록, 트위터 부트스트랩, JB 플러그인등 설정이 되어있음.
* 처음엔 간편하고 좋아보이나, 처음부터 이걸로 접근하는건 비추천..


## Docker
* Windows에서 기어이 돌릴려고 라이브러리 어거지로 돌아가도록 수정하다, 다 내려놓고 docker이용.
* [윈도우즈 도커를 이용하여 깃허브 페이지 테스트하기](/blog/githubpages_on_windows_docker)

```bash
docker@default:~$ docker start jekyll
jekyll
docker@default:~$ docker attach jekyll


root@33b5f808449d:/#
root@33b5f808449d:/# cd /site/
root@33b5f808449d:/site# source /etc/profile.d/rvm.sh
root@33b5f808449d:/site# rvm use 2.2
Using /usr/local/rvm/gems/ruby-2.2.1
root@33b5f808449d:/site# jekyll serve --port 8000 --watch --force_polling
```


# code highlight
* https://github.com/richleland/pygments-css
* http://richleland.github.io/pygments-css/
* https://github.com/chriskempson/tomorrow-theme
* ref:
 - tomorrow-pygments: https://github.com/MozMorris/tomorrow-pygments


## Tags
* http://www.minddust.com/post/tags-and-categories-on-github-pages/
* http://jekyllrb.com/docs/permalinks/


## Liquid
* jekyll 지원을 안해줘서 그렇지 개인적으로는 Mustache템플릿 엔진을 더 선호한다.
* http://jekyllrb.com/docs/variables/
* https://github.com/Shopify/liquid/wiki/Liquid-for-Designers


## markdown
* https://github.com/vmg/redcarpet


## Components
### bootstrap
* http://twitter.github.io/bootstrap/
* 동영상강의: http://www.youtube.com/user/easydevtuts/videos?shelf_index=1&tag_id=&sort=p&view=0
* ref:
 - bootswatch: http://bootswatch.com/
   - slate

### Font Awesome
* http://fortawesome.github.io/Font-Awesome/


### Music
* SCM Music Player: http://scmplayer.net/


### ColorZilla
* 색상 추출하는 firefox 플러그인.
* https://addons.mozilla.org/en-us/firefox/addon/colorzilla/
