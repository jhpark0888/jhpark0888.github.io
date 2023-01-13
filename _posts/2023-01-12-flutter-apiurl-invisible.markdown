---
layout: posts
title:  "[Flutter] API KEY 숨기는 법"
date:   2023-01-12 01:28:00 +0900
categories: flutter
---
<br>
<br>
프로젝트를 진행하면서 API KEY이나 URL등은 팀원이 아닌 사람들은 알 수 없도록 해야한다.

따라서 Github에 해당 정보들을 올리지 않으면서 로컬에서는 그 정보들을 사용할 수 있어야 한다.

이럴 때 env 파일을 사용한다.


## .env 파일
<br>
.env파일은 환경변수를 정의하는 파일이다. API KEY, URL, 포트 등과 같은 정보들을 정의해서 사용한다.

주로 **dotenv** 패키지를 사용하여 env 파일을 사용한다.

___
## 라이브러리 추가
<br>
Flutter에서 .env 파일을 사용하기 위해서 라이브러리를 주로 사용하는데 나는 [flutter_dotenv](https://pub.dev/packages/flutter_dotenv) 라이브러리를 사용하였다.

아래와 같이 **flutter_dotenv** 라이브러리를 추가해준다.

![flutter_dotenv](https://user-images.githubusercontent.com/77378301/211866074-420f8c1f-7359-429c-b036-cb0a9853f0ed.png)

___
## .env 파일 생성 및 작성
<br>
프로젝트의 최상위 루트에 .env파일을 생성해준다

![envfile](https://user-images.githubusercontent.com/77378301/211868188-7f2c45a0-2ea3-42f6-b8af-306b33ededfb.png)

그리고 정의할 환경변수를 작성해준다. 나는 API_URL을 작성하였다.

![envfile_write](https://user-images.githubusercontent.com/77378301/211869804-e17ba977-d554-4cd0-ab52-c225b15476e0.png)

___
## flutter_dotenv 사용
<br>
**flutter_dotenv**을 사용하기 위해서는 메인 메서드에서 dotenv의 load 메서드를 호출 해야한다.

```dart
void main() async {
  WidgetsFlutterBinding.ensureInitialized();

  await dotenv.load();

   runApp(
    MyApp(), 
  );
}
```

그 후 아래와 같이 ```dotenv.env```를 통해 해당 값을 env파일에서 불러올 수 있다.

```dart
dotenv.env["API_URL"]
```

**Environment** 클래스를 만들어서 사용하면 정의한 값들을 쉽게 가져올 수 있다.

```dart
class Environment {
  static String get apiUrl {
    return dotenv.env["API_URL"] ?? "API_URL not found!";
  }
}

Environment.apiUrl
```

___
## .gitignore에 env파일 추가
<br>
마지막으로 .gitignore에 .env 파일을 추가해야 Github에 .env 파일이 올라가지 않는다.

![gitignore_env](https://user-images.githubusercontent.com/77378301/211872799-7c055179-8dcc-4aa2-906d-d7e7b0802423.png)

___
<br>

**참고자료**<br> 
<https://www.youtube.com/watch?v=xTxwjbcd8kA><br>
{: .notice--info}