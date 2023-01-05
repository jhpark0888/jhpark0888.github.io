---
layout: posts
title:  "[Git] 깃허브 커밋 히스토리 모두 삭제하는 법"
date:   2023-01-05 21:00:00 +0900
categories: git
---
<br>
<br>
팀프로젝트를 개발하면서 코드를 프라이빗한 리포지토리에 관리 했었는데 내 리포지토리로 코드를 옮기기 위해서 API URL과 같은 정보들을 숨겼지만 커밋 이력에서는 그 정보들이 다 보이더라고요..ㅠ

그래서 방법을 찾아봤지만 그 정보들이 있는 커밋 들만 찾아서 지우기에는 어려울 거 같아서 모든 커밋 이력을 지우기로 했습니다ㅠㅠ

## 커밋 히스토리 삭제
<br>
커밋 히스토리를 삭제하려면 현재 작업 디렉토리에서 다음 명령어를 입력해야 합니다.
```bash
rm -rf .git
```

저는 VSCode를 사용하고 있는데 터미널에서 명령어를 입력했더니 아래와 같은 에러가 났습니다.
![커밋 히스토리 삭제 에러](https://user-images.githubusercontent.com/77378301/210778722-bccafbff-b556-4c3f-9a67-c984a541669d.png)

그래서 구글에 검색해봤더니 터미널을 Git Bash로 바꾸고 명령어를 입력하면 된다고 해서 그대로 해봤더니 에러가 해결되었습니다.

![깃배쉬](https://user-images.githubusercontent.com/77378301/210779376-5fe858c1-787f-4206-b5d6-e2549f3966b1.png)

___

## 깃허브에 푸쉬
<br>
커밋 히스토리를 삭제했으면 이제 리포지토리에 푸쉬를 해줍니다.
```git init```으로 초기화 해주고 ```git add .```으로 모든 파일을 추가합니다.

```
git init
git add .
```
<br>
그 후 현재 내용을 커밋해줍니다.
```
git commit -m "Fix: Commit history delete"
```

<br>
아래 명령어로 푸쉬를 할 리모트 서버 경로를 추가해줍니다.

```rm -rf .git``` 명령어로 인해 기존에 있던 remote가 사라졌으므로 서버 경로를 추가해야 합니다.
{: .notice--info}

```
git remote add origin [저장소 URL]
```

<br>
마지막으로 아래 명령어로 푸쉬해줍니다.
```
git push origin master
```
<br>

**만약 기존에 사용했던 리포지토리에 푸쉬하는 것이라면 아래 명령어를 사용합니다.**
```
git push -u --force origin master
```
**해당 명령어는 강제로 푸쉬하는 것으로 리포지토리에 있는 파일들이 없어질 수 있어 주의해서 사용해야 합니다.**
{: .notice--danger}

<br>
푸쉬하면 아래와 같이 커밋 히스토리가 초기화 돼있습니다.

![커밋 초기화](https://user-images.githubusercontent.com/77378301/210797022-19c3b92b-ceb0-4e1f-aa06-4503e627da16.png)

<br>

**참고자료**<br> 
<https://jootc.com/p/201909143109><br>
<https://velog.io/@kkyu0718/Remove-Item-%EB%A7%A4%EA%B0%9C-%EB%B3%80%EC%88%98-%EC%9D%B4%EB%A6%84-rf%EA%B3%BC%EC%99%80-%EC%9D%BC%EC%B9%98%ED%95%98%EB%8A%94-%EB%A7%A4%EA%B0%9C-%EB%B3%80%EC%88%98%EB%A5%BC-%EC%B0%BE%EC%9D%84-%EC%88%98-%EC%97%86%EC%8A%B5%EB%8B%88%EB%8B%A4>
{: .notice--info}