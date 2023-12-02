+++

title = 'AutoGenerated.Message.dll on Unity'
tags = ['unity3d', 'code-generator', 'protobuf']

+++


## 문제인식.

 message의 수정 추가시, 클라이언트 프로그래머가 매번 명칭과 인자에 따라 클래스를 수정한다.
단순 api통신은 dll하나로 만들어져서, 클라이언트 프로그래머가 별로 신경 쓰지않고, 다른 작업에 좀 더 집중하고 싶다.

## 바라는점.

message 수정, 추가시 하나로 된 dll파일을 얻었으면 한다.

## 시도방법.

- message에서 하나의 dll을 얻는다.
- dll을 로드하여 sender를 생성한다.
- sender와 앞선 dll을 하나로 묶는다.

## 해결

- ![flow.png](https://github.com/netpyoung/nf.protocol-flow/raw/master/flow.png)
- [https://github.com/netpyoung/nf.protocol-flow](https://github.com/netpyoung/nf.protocol-flow)
- 플로우는 Rakefile 참조


## 후기.

mesage의 추가 수정이 가해져도, 신경을 덜 쓰게 되었다.