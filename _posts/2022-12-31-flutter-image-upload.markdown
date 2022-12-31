---
layout: posts
title:  "[Flutter] 플러터에서 서버에 파일 및 이미지를 업로드 하는 법"
date:   2022-12-31 15:59:51 +0900
categories: flutter
---
<br>
<br>
진행하고 있는 프로젝트에서 서버에 파일 및 이미지를 업로드할 필요가 생겼습니다.

저는 HTTP 프로토콜을 사용하고 있었기 때문에 서버에 데이터를 보낼 때 데이터를 **json**형식으로 변환하여 데이터를 보내고 있습니다.

그래서 파일도 json으로 변환하여 보내면 서버에 보내질 줄 알았지만 보내보니 서버에서 받아지지 않았습니다.

그래서 검색한 결과 **multipart/form-data**형식을 알게 되었습니다.

___
## multipart/form-data
<br>
multipart/form-data에 대해 알려면 HTTP에 대한 지식이 필요하기 때문에 여기서 설명하지는 않고 설명이 잘 되어있는 블로그를 참조하겠습니다!

**HTTP와 multipart/form-data 설명**<br> 
<https://velog.io/@shin6403/HTTP-multipartform-data-%EB%9E%80>
{: .notice--info}

___

## Flutter에서의 multipart/form-data 사용
<br>
우선 플러터에서 HTTP 프로토콜을 편하게 사용하려면 [http](https://pub.dev/packages/http)라이브러리를 추가하는 것이 좋습니다.
<br>

**pubspec.yaml** 파일의 dependencies에 http 라이브러리를 추가해줍니다.

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_localizations:
    sdk: flutter
  http: ^원하는 버전 <--
```
<br>

우선 http 라이브러리를 사용하기 위해서 import 해줍니다. 

그런데 http 라이브러리의 메소드들이 다른 라이브러리들의 메소드들과 이름이 겹칠 수 있어서
http로 네임스페이스를 지정해주었습니다.

```dart
import 'package:http/http.dart' as http;
```

<br>

파일을 보낼 api 주소를 파라미터로 Uri를 선언해주고 **MultipartRequest** 라는 클래스 안에 **해당 통신의 HTTP메서드 종류**와 **Uri**를 넣고 객체를 만듭니다.

그리고 header 부분에 **Content-Type**을 **multipart/form-data**으로 바꿔주기 위한 변수를 만듭니다. 그 후에 request에 headers를 적용해줍니다. 

<u>이 부분을 설정 안해주면 서버에서 파일을 받지 못합니다.</u>

```dart
final uri = Uri.parse("파일을 보낼 Api 주소");

var request = http.MultipartRequest('POST', uri);

final headers = {
      'Content-Type': 'multipart/form-data',
    };

request.headers.addAll(headers);
```

<br>

## Multipart 변환 및 Request에 파일 추가
<br>
이제 파일을 Multipart 형식으로 변환해야 합니다.

###### http의 MultipartFile이라는 클래스를 이용하여 변환할 수 있습니다. 

이때 파일의 Byte, Path 등을 통해 변환할 수 있는데 저는 핸드폰에 있는 파일 및 이미지를 서버에 보내는 것이기 때문에 Path를 사용했습니다.

fromPath 메서드에서는 첫번째 파라미터로 **field(String)**를 받고 두번째 파라미터로 파일의 **Path(String)**를 받습니다. 저는 이미지를 보내는 통신이었기 때문에 field명을 image로 설정하였습니다.

그 후 headers 처럼 request.files에 해당 MultipartFile 객체를 추가합니다.

파일이 아닌 텍스트 같은 경우는 request.fields에 key값을 설정하여 서버로 보낼 수 있습니다.
{: .notice--info}

```dart
//파일이 한 개인 경우
var multipartFile =
        await http.MultipartFile.fromPath('image', image.path);

request.files.add(multipartFile);

request.fields["text"] = "text"
```

<br>
###### 파일이 여러개 인 경우 for문을 사용하여 여러개를 보내는 것이 가능합니다.

```dart
//파일이 여러 개인 경우
List<File> images = [File(), File(), File(), File(), File()];

for(File image in images) {
    var multipartFile =
        await http.MultipartFile.fromPath('image', image.path);

    request.files.add(multipartFile);
}
```

<br>

파일과 텍스트를 다 담았으면 `request.send()`를 통해 서버로 Request를 보내게 됩니다. 이 때 return 값은 http의 **StreamedResposne**입니다.

**response.statusCode가 200인 경우 통신에 성공한 것입니다.**

```dart
http.StreamedResponse response = await request.send();
print(response.statusCode)
```

<br>

## Response body 사용

<br>

통신 후 서버에서 데이터를 보낼 수가 있는데 Multipart 형식의 통신을 했을 경우 플러터에서는 **StreamedResponse**로 받기 때문에 Response 안의 데이터를 사용하기 위해서는 아래 코드와 같이 Bytes를 String 으로 변환하여 사용해야 합니다.

```dart
String responsebody = await response.stream.bytesToString();
```
____

## 전체코드

```dart
import 'package:http/http.dart' as http;

void imageUpload() async {
    final uri = Uri.parse("파일을 보낼 Api 주소");

    var request = http.MultipartRequest('POST', uri);

    final headers = {
      'Content-Type': 'multipart/form-data',
    };

    request.headers.addAll(headers);

    var multipartFile =
            await http.MultipartFile.fromPath('image', image.path);

    request.files.add(multipartFile);

    request.fields["text"] = "text"

    http.StreamedResponse response = await request.send()
          .timeout(Duration(milliseconds: 10000));

    String responsebody = await response.stream.bytesToString();
}
```
