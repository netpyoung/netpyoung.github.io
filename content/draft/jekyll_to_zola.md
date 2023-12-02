+++
draft = true
+++


jekyll을 썻던 이유
github기본
추가 커밋 불필요

zola를 쓰는 이유
github기본 jekyll버전
jekyll의 의존성 설치문제 짜증나서 안키게 된다
jekyll 컴파일 과정이 뒤에서 일어난다 github action사용 고려
리프레쉬
깔끔한 폴더구조 jekyll에선 _posts로 블로그를 넣었는데 content에 넣으면 된다
tera - https://github.com/Keats/tera
직관적 만족

추후 github action의 유료화 jekyll만 쓰도록 한다면

후보 Cobalt rust 문서빈약

webpack쓰던걸 그냥 cdn쓰도록
글이 별로 없어서 마이그레이션 비용이 그리 크지 않았다


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

.gitignore
public/
.DS_Store

config.toml
https://www.getzola.org/documentation/getting-started/configuration/

## action
https://www.getzola.org/documentation/deployment/github-pages/

tera
https://keats.github.io/tera/

``` yaml
on: push
name: Build and deploy GH Pages
jobs:
  build:
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    steps:
      - name: checkout
        uses: actions/checkout@v4

      # https://github.com/shalzz/zola-deploy-action
      - name: build_and_deploy
        uses: shalzz/zola-deploy-action@v0.17.2
        env:
          # Target branch
          PAGES_BRANCH: gh-pages
          # Provide personal access token
          TOKEN: ${{ secrets.GITHUB_TOKEN }} # same repo
          # TOKEN: ${{ secrets.TOKEN }} # other repo
          # BUILD_DIR: Default is . (current directory)
          # OUT_DIR:  Default is public
          GITHUB_HOSTNAME: github.io # default is github.com
```

| templates/              |                               |
| ----------------------- | ----------------------------- |
| index.html              | applied to the site homepage; |
| section.html            | applied to all sections       |
| page.html               | applied to all pages          |
| 404.html                |                               |
| atom.xml                | config.toml 바꿀 수 있다      |
| sitemap.xml             |                               |
| split_sitemap_index.xml |                               |
| anchor-link.html        |                               |
| robots.txt              |                               |
| rss.xml                 |                               |
| sitemap.xml             |                               |
| split_sitemap_index.xml |                               |

sass
bulma
https://bulma.io/documentation/overview/start/ 0.9.4
https://fontawesome.com/ 6.5.2
flag-icons@7.0.2
Droid+Sans+Mono
Noto+Sans+KR

zola에는 shortcodes
templates/shortcodes/에 넣는다
마크다운에서 rendering코드를 넣지못함
단축 코드는 일반적인 Tera 표현식처럼 보이지만 전혀 Tera가 아닙니다. 단지 인수를 템플릿으로 전달할 수 있을 뿐입니다. 참고할 수 있는 몇 가지 제한 사항은 다음과 같습니다.
단축 코드는 Tera 변수를 참조할 수 없습니다.

_index.md

elisp_lexcical_dynamic
elisp-lexcical-dynamic
[slugify]
paths = "safe"


===
emacs => vscode
느리다

https://www.getzola.org/documentation/templates/sitemap/
https://www.getzola.org/documentation/templates/feeds/