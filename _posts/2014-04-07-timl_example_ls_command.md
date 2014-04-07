---

layout: post
title: 'Timl example (:Ls Command)'
tags: vim Viml clojure Timl

---

## Timl이란?
* https://github.com/tpope/timl
* lisp 방언(dialect)중 하나로, VimL로 컴파일된다. (Clojure가 VimL을 만났다고 생각하면 된다.)
* 4월 1일날 공개해서, 만우절 낚시인줄 알았는데 잘돌아간다..
* [2014.04.01 sbcl 클로져 지원 낚시](http://www.reddit.com/r/Clojure/comments/21x6hr/sbcl_with_support_for_clojure_xpost_from_rlisp/)


## Feature.
* 매크로지원:`syntax quoting`, `&form`, `&env`.
* 레퍼런스타입 지원: vars, atoms, futures.
* 내장 함수 사용: `(#*toupper "TimL is pretty neat")`
* Vim command 호출: `(execute "wq")`
* 기타 등등.. (공식페이지 참조.)


## Example.
* `:!ls`로 Vim `밖`에서 파일목록을 볼 수 있지만, Vim 버퍼 `안`에서 ls 명령결과를 확인할 수 있는 플러그 인을 작성해보자.


플러그인 구조.

    [~/.vim]$ tree xxx
    xxx
    ├── autoload
    │   └── xxx
    │       └── core.tim
    └── plugin
        └── xxx.vim


vimrc

```vim
if has('vim_starting')
  set runtimepath+=~/.vim/xxx/
endif
```


xxx/autoload/xxx/core.tim

```clojure
(ns xxx.core)


;; ref: http://clojuredocs.org/clojure_core/clojure.core/->>
;; timl에는 `->`만 들어가 있고, 아직 `->>`는 들어가 있지 않다.
;; 기존 clojure 소스카피


(defmacro ->>
  ([x form] (if (seq? form)
    (with-meta `(~(first form) ~@(next form)  ~x) (meta form))
    (list form x)))
  ([x form & more]
   `(->> (->> ~x ~form) ~@more)))


(defn ls [& args]
  (->> args
       (concat ["ls" "-al"])
       (join " ")
       (#*system)
       (println)))
```


xxx/plugin/xxx.vim

```vim
if exists("g:loaded_xxx") || v:version < 700 || &cp
    finish
endif

let g:loaded_xxx = 1

command! -bang -complete=file -nargs=* Ls call xxx#core#ls(<f-args>)
```


## Testing.

Command. (in Vim)

    :Ls ~/.vim/conf.d/ ~/.vim/xxx/


Result.

    /home/pyoung/.vim/conf.d/:
    total 20
    drwxrwxr-x 2 pyoung pyoung 4096  4월  7 00:54 .
    drwxr-xr-x 7 pyoung pyoung 4096  4월  7 03:59 ..
    -rw-rw-r-- 1 pyoung pyoung 2092  4월  7 00:27 00_neobundle.vim
    -rw-rw-r-- 1 pyoung pyoung 3518 11월 16 05:46 01_init.vim
    -rw-rw-r-- 1 pyoung pyoung   60  4월  7 00:54 02_tim.vim

    /home/pyoung/.vim/xxx/:
    total 16
    drwxrwxr-x 4 pyoung pyoung 4096  4월  7 03:19 .
    drwxr-xr-x 7 pyoung pyoung 4096  4월  7 03:59 ..
    drwxrwxr-x 3 pyoung pyoung 4096  4월  7 00:54 autoload
    drwxrwxr-x 2 pyoung pyoung 4096  4월  7 03:28 plugin


    Press ENTER or type command to continue


## Problem.
* 캐쉬 문제.
 - 2014.04.07 현재 캐쉬문제가 있다. 한번 컴파일이되면, 다음 변경시 재 컴파일이 안 된채로 에러를 뱉는 문제가 있다.
 - https://github.com/tpope/timl/issues/12

* 캐쉬 문제 해결.
 - `rm -rf ~/.cache/vim/timl/`로 캐쉬를 날려주자.


## Tip.
* `:Wepl`: 현재 파일을 로드하면서, repl을 띄움.
* `:TLsource %`: 현재 파일을 다시 읽음.(이 역시 캐쉬문제가 발생하면 캐쉬파일 지워주는 수밖에..)


# Reference.
* 공식사이트:
 - https://github.com/tpope/timl

* tslime2.vim:
 - vim에서 tmux에게 명령어를 보내주는 플러그인.
 - https://github.com/sjl/tslime2.vim
