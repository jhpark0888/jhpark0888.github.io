---
layout: posts
title:  "플러터 스크롤 뷰에서 ScrollGlow 없애는 방법"
date:   2022-07-12 20:58:51 +0900
categories: flutter
---
<br>
플러터로 앱을 만들다 보면 디자인이 맘에 안드는 경우가 있다.

ListView나 SingleChildView를 사용하는 경우 스크롤이 끝에 다다랐을 때 생기는 **ScrollGlow**도 그 중 하나이다.

```dart
physics: const BouncingScrollPhysics()
```

스크롤 뷰의 physics 속성을 `BouncingScrollPhysics()`로 바꿔 ScrollGlow를 없앨 수 있지만 가로 스크롤의 경우 이러한 스크롤이 어색할 수 있다.

___
따라서 근본적으로 ScrollGlow를 없애는 방법을 찾아야했다. 그리고 찾은 방법은 아래와 같다

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

`ScrollNoneffectWidget`에 child로 ScrollGlow를 없애고 싶은 스크롤 뷰를 넣게 되면 스크롤이 끝에 다다랐을 때 아무런 효과도 나타나지 않게 된다. 
