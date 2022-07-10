---
layout: posts
title:  "Getx에서 dialog만 지우는 방법"
date:   2022-07-10 16:53:51 +0900
categories: getx
---
<br>
Getx를 이용하여 앱을 만들면서 dialog만 지우는 기능이 필요해졌어요.

처음에는 이런 식으로 처리해봤는데 이렇게 하면 현재 페이지까지 나가지더라고요.
```dart
void dialogBack() {
  Get.back(closeOverlays: false);
    }
```

___

그래서 방법을 찾아봤는데 Getx에 `Get.isOverlaysClosed` 라는 변수가 있었어요. 이 변수는 dialog나 snackba나 bottomsheet가 닫혀있으면 true 열려있으면 false 값을 리턴합니다.

그리고 Getx에는 `Get.until` 함수가 있었는데 이 함수는 return 값이 false 이면 현재 페이지를 나가고 반복하여 return 값이 true일 때까지 페이지를 나갑니다.

이 변수와 함수를 이용하여 이 문제를 해결했습니다.

```dart
void dialogBack() {
  Get.until((route) {
    return Get.isOverlaysClosed;
  });
}
```
dialogBack 함수를 작동시키면 dialog나 snackbar나 bottomsheet는 전부 없어지고 페이지만 남게 됩니다.