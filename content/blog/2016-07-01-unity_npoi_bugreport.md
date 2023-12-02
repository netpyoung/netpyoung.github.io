+++

title = 'Unity NPOI with IL2CPP'
tags = ['unity3d', 'NPOI']

+++


## NPOI란?

- <https://npoi.codeplex.com/>
- Excel을 읽어들일 수 있는 POI Java project를 dotnet으로 구현한 것으로, net20, net40 dll을 제공하고 있다.
- 게임 개발을 하면서 데이터를 excel로 관리하는 경우가 많은데, 툴을 구현할 시 유용하다.


## 문제는?

- iOS 디바이스에서 빌드해서 사용시, 문제가 있다는 것을 1년전 쯤에 빌드해보고 알고있었다.(그때는 IL2CPP나 개발 중이여서 추후에 보자고 덮어뒀었다.)
- 최근 혹시나 하는 마음에 빌드를 하였고, XCodeProject생성 빌드시 IL2CPP에서 문제가 발생하였다.


## 해결책은?

- 이번에는 1년 전과는 달리 포럼에 [post](http://forum.unity3d.com/threads/il2cpp-build-error-unity5-3-5f1-personal-version.413861/)를 남기어 두었고, 관계자의 답글대로, Unity Editor의 Bug Report를 이용하여 버그리포트를 보냈다.
- 지금(5.3.5f1)에서는 .NET 2.0 Subset로 빌드하면 된다.
- [issue](https://issuetracker.unity3d.com/issues/ios-build-fails-with-net-2-dot-0-in-mscorlib-dot-dll)가 등록되었고, UnityQA에서 온 메일로 확인한 결과, 5.3.5p8에서는 이용할 수 있다고 한다. (ios device에서 excel reading이 가능해 질 것으로 본다)
