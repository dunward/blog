---
layout: post
title:  Unity Build Pipeline
date:   2022-05-03 21:12:03 +0900
categories: unity il2cpp
---
유니티에서 Burst Compiler가 도입되기전에는 빌드시에 아래와 같은 프로세스를 거칩니다.

![1](https://github.com/novemberi/novemberi.github.io/tree/master/_posts/il2cpp/1.png)

최근에 업무중에 오래된 유니티 프로젝트를 최신버전으로 업데이트해야할 일이 있었는데, 사실 유니티빌드 그대로 사용하는 프로젝트라면 크게 문제되지 않습니다.

하지만 만약 유니티 프로젝트를 Native Export 이후에, Native build를 거치는 과정이 있다면 약간 까다로워질 수 있습니다. Native Project에 커스텀으로 이것저것 수정했다면 말이죠.

유니티 Burst Compiler는 위 이미지에서 IL2CPP라고 불리는 과정을 약간 수정한 기능입니다.

![2](https://github.com/novemberi/novemberi.github.io/tree/master/_posts/il2cpp/2.png)

위 이미지에 보시면 IL 과정이 약간 바뀌었습니다. 여기서는 Burst Compiler에 관해 다루는 포스트가 아니기 때문에 그렇게 자세히 다루지는 않을 예정이지만, 유니티 프로젝트는 이제 Burst Compiler의 영향을 받는 코드와, 일반 코드 이렇게 두개로 나뉘어져있습니다.

우리가 사용하는 일반적인 코드는 IL2CPP 과정으로 그대로 나아가지만, Burst는 당연히 Burst Compiler 과정으로 들어가게 됩니다. 이 프로세스때문에 구버전 Native Project에서 빌드 파이프라인이 좀 바뀌게 되었으니 그 부분을 유의하시는게 좋습니다.