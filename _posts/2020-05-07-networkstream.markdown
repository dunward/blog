---
layout: post
title:  "NetworkStream과 Casting에 대한 내용"
date:   2020-05-07 19:41:36 +0900
categories: C# Knowledge
---
최근 개발하는 내용중에 중계서버를 경유하여 클라이언트끼리 통신해야하는 필요성이 있었는데 내가 원하는 구조는 다음과 같았다.
```csharp
[Serializable]
class Foo : Bar { }

[Serializable]
class Bar { }
```
위와 같이 직렬화를 시킨 클래스를 중계서버로 경유하여 다른 클라이언트로 보내주는데 궁금증이 들었다.<br>
예를 들어 A->중계서버->B와 같은 방식으로 통신이 진행될때 A->중계서버에서는 Foo 클래스를 NetworkStream으로 중계서버로 보낸뒤<br>
중계서버->B에서는 Bar클래스를 NetworkStream으로 보냈을때 B에서는 받았던 Bar클래스를 Foo로 형변환 시켜서 사용이 가능할까 의문점이 들었다.<br>
우선 주변에 물어본 결과 가능할것 같다고 들었다.<br>
업캐스팅이 진행될때 클래스의 힙 메모리또한 같이 변형될것이라고 생각했는데 손실없이 그대로 유지된다.

```csharp
Foo foo = new Foo();
Bar bar = new Bar();
Console.WriteLine($"{Marshal.SizeOf(foo)}");
Console.WriteLine($"{Marshal.SizeOf(bar)}");
Console.WriteLine($"{Marshal.SizeOf(foo as Bar)}"); // UpCasting
```

위와 같은 코드로 짤막하게 테스트해보니 업캐스팅이 진행되도 메모리크기가 변하지는 않았다.<br>
언제부터 업캐스팅했을때 메모리크기가 변했다고 생각했는지는 몰라도 지금이라도 깨달아서 다행이다.