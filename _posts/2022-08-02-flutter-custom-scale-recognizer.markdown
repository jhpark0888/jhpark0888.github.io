---
layout: posts
title:  "Flutter GestureArena에서 우선적으로 ScaleGestureRecognizer를 선택하는 법"
date:   2022-08-02 01:18:00 +0900
categories: flutter
---
<br>
프로젝트를 진행하다가 곤란한 문제를 맞이하게 됐다.

나는 `ScrollView`안에 좌우 상하로 제스쳐가 가능한 위젯을 넣고 싶었다.
예를 들면 `ListView`안에 이미지를 좌우상하로 움직일 수 있게 하는 `GestureDetector`를 넣는 것이다. 

```dart
ListView(
  children: [
    GestureDetector()
    ///
  ]
)
```
위와 같이 코드를 짜고 실행을 하면 `GestureDetector` 영역 안에서 좌우로 스크롤 할 경우는 이미지가 움직이지만 상하로 스크롤 할 경우 `ListView`의 스크롤이 움직이게 된다.

그래서 나는 ListView 보다 `GestureDetector`가 우선적으로 Gesture를 인식하는 방법을 찾아야 했다.

___

많은 검색 끝에 `GestureArena`를 알게 되었다. 
`GestureArena는` 자세히는 모르겠지만 한 영역에서 Gesture가 일어났을 때 해당 영역의 여러개의 Gesture들 중 한 개의 Gesture를 선택해주는 것 같았다.

그리고 `EagerGestureRecognizer는` `GestureArena에서` 무조건 우선적으로 선택이 되는 `GestureRecognizer`였다.
그 이유는 확실한 것은 아니지만 `addAllowedPointer`의 `resolve`라는 함수 때문인 것 같았다.

```dart
class EagerGestureRecognizer extends OneSequenceGestureRecognizer {
  ///
  @override
  void addAllowedPointer(PointerDownEvent event) {
    super.addAllowedPointer(event);
    resolve(GestureDisposition.accepted);
    stopTrackingPointer(event.pointer);
  }
  ///
}

```

`resolve`라는 함수를 `ScaleGestureRecognizer`의 `addAllowedPointer`에 넣으면 `GestureArena`에서 우선적으로 `ScaleGestureRecognizer`를 선택하게 될 것이다.

```dart
class CustomScaleGestureRecognizer extends OneSequenceGestureRecognizer {
  ///
    @override
  void addAllowedPointer(PointerDownEvent event) {
    super.addAllowedPointer(event);
    _velocityTrackers[event.pointer] = VelocityTracker.withKind(event.kind);
    if (_state == _ScaleState.ready) {
      _state = _ScaleState.possible;
      _initialSpan = 0.0;
      _currentSpan = 0.0;
      _initialHorizontalSpan = 0.0;
      _currentHorizontalSpan = 0.0;
      _initialVerticalSpan = 0.0;
      _currentVerticalSpan = 0.0;
      _pointerLocations = <int, Offset>{};
      _pointerQueue = <int>[];
    }
    resolve(GestureDisposition.accepted);
    // stopTrackingPointer(event.pointer);
  }
  ///
}
```
`GestureRecognizer`를 사용하기 위해서 `GestureDetector` 대신 `RawGestureDetector`를 사용하였다.

```dart
ListView(
  children: [
    RawGestureDetector(
          gestures: {
            CustomScaleGestureRecognizer: GestureRecognizerFactoryWithHandlers<
                CustomScaleGestureRecognizer>(
              () => CustomScaleGestureRecognizer(),
              (CustomScaleGestureRecognizer instance) {
                instance
                  ..onStart = _handleScaleStart
                  ..onUpdate = _handleScaleUpdate
                  ..onEnd = _handleScaleEnd;
              },
            ),
          },
          child: Image.network(image),
    )
  ]
)
```

다행히 위의 코드로 실행시킬 경우 `RawGestureDetector`의 영역 안에서는 좌우상하 모두 `GestureArena`에서 `CustomScaleGestureRecognizer`를 선택하였고 이미지를 움직이게 할 수 있었다.

