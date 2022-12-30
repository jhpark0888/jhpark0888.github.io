---
layout: posts
title:  "[Flutter] 스크롤 뷰에서 ScrollGlow 없애는 방법"
date:   2022-07-12 20:58:51 +0900
categories: flutter
---
<br>
<br>
플러터로 앱을 만들다 보면 디자인이 맘에 안드는 경우가 있습니다.

ListView나 SingleChildView를 사용하는 경우 스크롤이 끝에 다다랐을 때 생기는 **ScrollGlow**도 그 중 하나입니다.

```dart
physics: const BouncingScrollPhysics()
```

스크롤 뷰의 physics 속성을 `BouncingScrollPhysics()`로 바꿔 ScrollGlow를 없앨 수 있지만 가로 스크롤의 경우 이러한 스크롤이 어색할 수 있습니다.

___
따라서 저는 근본적으로 ScrollGlow를 없애는 방법을 찾아야 했습니다. 그리고 찾은 방법은 아래와 같습니다

```dart
import 'package:flutter/material.dart';

class ScrollNoneffectWidget extends StatelessWidget {
  ScrollNoneffectWidget({Key? key, required this.child}) : super(key: key);
  Widget child;

  @override
  Widget build(BuildContext context) {
    return NotificationListener<OverscrollIndicatorNotification>(
        onNotification: ((notification) {
          notification.disallowIndicator();
          return false;
        }),
        child: child);
  }
}

```

`ScrollNoneffectWidget`에 child로 ScrollGlow를 없애고 싶은 스크롤 뷰를 넣게 되면 스크롤이 끝에 다다랐을 때 아무런 효과도 나타나지 않게 됩니다. 

---

**추가** 

안드로이드 폰에서는 스크롤이 끝에 다다르게 되면 위의 컨텐츠들이 조금 늘어났다가 돌아가는 효과를 가집니다.
이러한 효과를 플러터에서 구현하려면 `MaterialApp`위젯에 `scrollBehavior`를 설정하면 됩니다.

```dart
class MyApp extends StatelessWidget {
  MyApp({Key? key,}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      //
      scrollBehavior: const ScrollBehavior(
          androidOverscrollIndicator: AndroidOverscrollIndicator.stretch),
      //
    );
  }
}
```
