---

layout: post
title: 'Github Pages - Jekyll v3'
tags: talk jekyll docker

---

# Github Pages - Jekyll v3

오랜만에 블로그에서 사소한 내용을 수정후 커밋하니 다음과 같은 내용의 이메일이 도착했다.

```
The page build completed successfully, but returned the following warning:

You are currently using the 'redcarpet' Markdown engine, which will not be supported on GitHub Pages after May 1st. At that time, your site will use 'kramdown' for markdown rendering instead. To suppress this warning, remove the 'markdown' setting in your site's '_config.yml' file and confirm your site renders as expected. For more information, see https://help.github.com/articles/updating-your-markdown-processor-to-kramdown.

GitHub Pages was recently upgraded to Jekyll 3.0. It may help to confirm you're using the correct dependencies:

  https://github.com/blog/2100-github-pages-now-faster-and-simpler-with-jekyll-3-0

For information on troubleshooting Jekyll see:

  https://help.github.com/articles/using-jekyll-with-pages#troubleshooting

If you have any questions you can contact us by replying to this email.
```

내용인즉, 깃허브가 github-pages에서 쓰고있는 jekyll의 버전을 3으로 올린다는 내용이며, 이에 준하여 업데이트를 시켜주라는 말이였다.



github-pages페이지를 업데이트 시켜주고(오래간만에 gem update를...),

```bash
sudo gem update github-pages
```


나와같은 경우 다음의 라인이 문제가 되었다. 이제 github-pages를 이용시 highlighter부분과 markdown섹션을 신경쓸 필요가 없게되었다.

``` yaml
highlighter: pygments
markdown: redcarpet
redcarpet:
    extensions: ["no_intra_emphasis", "fenced_code_blocks", "autolink", "tables", "with_toc_data"]

```

수정시, highlighter만 pygments로 나두면 다음과 같은 메시지로 이메일이 날라온다.


```
The page build completed successfully, but returned the following warning:

You are attempting to use the 'pygments' highlighter, which is currently unsupported on GitHub Pages. Your site will use 'rouge' for highlighting instead. To suppress this warning, change the 'highlighter' value to 'rouge' in your '_config.yml'. For more information, see https://help.github.com/articles/page-build-failed-config-file-error/#fixing-highlighting-errors.

```


처음 github-pages를 설정할때 highlighter, markdown 설정 부분에서 좀 애를 먹은 기억이 있어, 나는 이와같이 github-pages가 변화한것을 환영한다.
