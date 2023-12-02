+++
title = 'jekyll에서 zola로'
tags = ['talk', 'homepage', 'jekill', 'zola']
+++

5년전에 블로그 글을 올리고 다시 새로운 글을 올린다. 블로그에 글을 쓰는것보다 재미있는 일들/해야 할 들이 많았다.

홈페이지를 새로 정리하려고 하는데, 기존에 쓰던 방식인 jekyll을 실행시키자니, 디펜던시를 받던가 도커를 실행해야하는데 이 작업자체가 귀찮았다.
대안을 생각해보았고, 생성기를 jekyll에서 zola로 교체하였다.

- 이전글
  - <https://netpyoung.github.io/blog/netpyoung.github.io_on_jekyll/>
  - <https://netpyoung.github.io/blog/githubpages_jekyll_v3/>

- jekyll을 썻던 이유
  - github 기본
  - 추가 커밋 불필요
- zola를 쓰는 이유
  - github기본 jekyll버전의 업데이트가 멈추었다
    - 2023.12.03 현재
      - github's jekyll current version `## 3.9.3 / 2023-01-27`
      - github's jekyll latest version `4.3.2`
    - jekyll의 의존성 설치문제 짜증나서 안키게 된다
    - 깔끔한 폴더구조
      - jekyll에선 _posts로 블로그를 넣었는데, 이젠 그냥 content에 넣으면 된다
    - 그냥
      - rust를 쓰니 속도가 빨라보였다
- 후보들
  - Cobalt: rust로 작성.
    - 문서, 사용자층 빈약해서 패스
  - hugo: go로 작성.
    - 문서, 사용자층 괜찮음
    - 어색한 템플릿 언어 Go’s html/template and text/template


- 이름도 zola라 졸라 빨라 보인다

## 바꾸면서

- 만족
  - 빠르다
  - 셋업에 골머리 썩힐 필요가 없었다.
    - jekyll은 ruby베이스라서 버전, 라이브러리가 os문제를 가지면 꽤나 골치 아파진다.
      - 그걸 피하려고 docker버전을 썼지만 docker도 셋팅하는게 귀찮다.
  - zola의 tera
    - liquid랑 비슷해서 딱히 문제없음. hugo로 갔으면 go템플릿하느라 빡쳤을듯
  - 깔끔해졌다
    - jekyll은 _posts폴더를 강제로 쓰도록하는데 zola는 괜찮았다
    - webpack쓰던걸 그냥 cdn쓰도록
      - jekyll에서 webpack썼는데 관리 귀찮
- 다행
  - 글이 별로 없어서 마이그레이션 비용이 그리 크지 않았다
- 어색
  - index.md가 아닌 contents/에 _index.md로 처리
  - slugify's path
    - 예를들어 파일이름이 `elisp_lexcical_dynamic`이런데 url은 `elisp-lexcical-dynamic`로 자동으로 바뀌는걸 방지하려면
    - config에서 `[slugify]`의 `paths = "safe"`로 해줘야 했다
- 별로
  - zola 마크다운에서 rendering코드를 넣지 못함
    - zola에는 shortcodes
      - templates/shortcodes/에 넣는다
      - 근데 shortcode는 Tera 변수를 참조할 수 없음
- 앞으로
  - 계속 쓰자
    - 추후 github action의 유료화나 jekyll만 쓰도록 한다면 다시 바꾸는걸 고려해야겠지만 지금은 zola로 만족
    - 다른 좋은 생성기가 있으면 고려해보자
    - 아싸리 개인 생성기도 만드는 것도 재미있을거 같은데, 시간이 부족하다.
  - search기능도 있는것 같은데 어떻게 쓰는지 알아봐야 할 것 같다
  - lispkorea.github.io도 zola로 바꿔봐야겠다


``` bash
git tag -a 1.9.0 2136b1c0e601ec1a68b94aa0daefd871769dff97 -m "jekyll version"
git tag -a 2.0.0 73a536e07328bc298586e75c0a5b752073aaf6f3 -m "zola verison"
```

## Ref


- [설정](https://www.getzola.org/documentation/getting-started/configuration/)
- [github action](https://www.getzola.org/documentation/deployment/github-pages/)
- [tera](https://keats.github.io/tera/)

| templates/              |                                                                                    |
| ----------------------- | ---------------------------------------------------------------------------------- |
| index.html              | applied to the site homepage;                                                      |
| section.html            | applied to all sections                                                            |
| page.html               | applied to all pages                                                               |
| 404.html                |                                                                                    |
| anchor-link.html        |                                                                                    |
| sitemap.xml             | <https://www.getzola.org/documentation/templates/sitemap/>                         |
| split_sitemap_index.xml |                                                                                    |
| atom.xml                | config.toml 바꿀 수 있다. <https://www.getzola.org/documentation/templates/feeds/> |
| rss.xml                 |                                                                                    |
| sitemap.xml             |                                                                                    |
| split_sitemap_index.xml |                                                                                    |
| robots.txt              |                                                                                    |


``` bash
brew install zola
zola init myblog
> What is the URL of your site? (https://example.com): https://netpyoung.github.io
> Do you want to enable Sass compilation? [Y/n]:
> Do you want to enable syntax highlighting? [y/N]: y
> Do you want to build a search index of the content? [y/N]: y
cd myblog
zola serve
Web server is available at http://127.0.0.1:1111
Options:
  -i, --interface <INTERFACE>    Interface to bind on [default: 127.0.0.1]
  -p, --port <PORT>              Which port to use [default: 1111]
  -o, --output-dir <OUTPUT_DIR>  by default 'public' dir in project root
  -u, --base-url <BASE_URL>      Changes the base_url [default: 127.0.0.1]
  -O, --open                     Open site in the default browser

# .gitignore
# public/
# .DS_Store
```
