---

layout: post
title: 'skype봇 sevabot의 한글명령어 문제 해결'
tags: skyepot sevabot NFD

---


# sevabot이란?
* http://sevabot-skype-bot.readthedocs.org/
* Skype4Py 프레임워크를 기반으로한, 스카이프 봇 프로그램.


# 문제는?
* 한글 명령어가 안먹히는 문제가 있음.
* 파이썬2에선 문자열 다룰때 x신같은게 꼭 인코딩 문제가 있음.
* 윈도우 경우, `utf-8`, `cp949` 관련 인코딩 문제가 있으며.
* 맥인 경우, `utf-8`뿐만 아니라, [NFD](http://en.wikipedia.org/wiki/Unicode_equivalence)문제까지 있다.


# 해결책은?

* `settings.py`의 `MOUDLE_PATHS`를 유니코드 문자열로 바꿔주고.

```python
    MODULE_PATHS = [u"modules"]
```


* `NFD`문제 해결을 위한, [Hye-Shik Chang 님의 hangul.py](https://raw.github.com/sublee/hangulize/master/hangulize/hangul.py) 을 `sevabot/hangul.py`로 저장.


* `sevabot/bot/module.py` 수정.

```python
# 기본 인코딩을 utf-8로 바꿔주고
import sys
reload(sys)
sys.setdefaultencoding('utf-8')

from sevabot.plugins import hangul


# windows 인코딩을 위한 threaded_run 함수 수정.

    if sys.platform == 'win32':
        env = os.environ.copy()
        env["SKYPE_USERNAME".encode("utf-8")] = username.encode("utf-8")
        env["SKYPE_FULLNAME".encode("utf-8")] = full_name.encode("utf-8")

        p_args = [arg.encode('cp949') for arg in args]
        process = subprocess.Popen(p_args, stdout=subprocess.PIPE, stderr=subprocess.STDOUT, shell=True, env=env)
        out = process.communicate()[0]
        return out.decode("utf-8")


# osx에 NFD문제 해결을 위한 load_modules 함수 수정

        if sys.platform == 'darwin':
            body = hangul.conjoin(body.decode('utf-8'))

```

# 남아있는 문제점.
* 윈도우즈 같은 경우 bat파일을 실행시킬 경우, output이 cp494인데, 기존 utf-8인코딩과 cp494인코딩을 동시에 지원하기에는 에로사항이 있다.


# 작업된 프로젝트.
* https://github.com/netpyoung/sevabot


# 참고.

* [OSX에 Skype bot - sevabot 설치하기](http://blog.naver.com/amurorei82/10188770513)
* [sevabot issue / With windows many problem... #58](https://github.com/opensourcehacker/sevabot/issues/58)
* [Mac OS X Filename Encoding](https://github.com/drypot/writings/blob/master/categories/mac/mac-os-x-filename-encoding.md)
