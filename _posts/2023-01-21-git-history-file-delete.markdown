---
layout: posts
title:  "[Git] 깃허브 커밋 히스토리에 파일 또는 폴더 삭제하는 법"
date:   2023-01-05 21:00:00 +0900
categories: git
---
<br>
<br>
전 포스팅에서 이미 커밋 되어버린 API URL을 지우기 위해 커밋 히스토리를 모두 지우는 방법을 사용하였다.

하지만 이 방법을 사용하면 그동안 쌓아왔던 커밋 기록들이 다 없어지기 때문에 좋은 방법은 아니다.

git rebase -i를 이용해서 과거의 커밋을 수정하는 방법을 알게 되었지만 내 경우에는 너무 오랫동안 API URL이 노출 되었었기 때문에 수정해야 할 커밋이 너무 많았다.

그래서 새로운 방법을 찾아봤는데 모든 커밋에서 파일을 지울 수 있는 방법을 알게 되었다.

## 커밋 히스토리에 특정 파일 삭제하는 법
<br>

```bash
git filter-branch --force --index-filter "git rm --cached --ignore-unmatch 해당 파일 또는 폴더 경로" --prune-empty --tag-name-filter cat -- --all
```

이 명령어를 입력하면 커밋 히스토리에 있는 파일 또는 폴더를 모든 커밋에서 지울 수 있다. 

**filter-branch**는 필터를 정의하고 정의한 필터를 통해 커밋들을 하나씩 비교하여 새로운 커밋으로 변경한다.

**--index-filter**는 **filter-branch**의 한 옵션으로 모든 브랜치들을 순회하면서 필터 명령을 수행하고 인덱스들을 재작성한다.

**"git rm --cached --ignore-unmatch 해당 파일 또는 폴더 경로"** 중 **git rm** 명령어는 로컬과 원격 저장소 브랜치에 있는 파일을 삭제한다.
**--cached**는 로컬에는 남기고 원격 저장소 브랜치에 있는 파일만 삭제하게 해준다.
**--ignore-unmatch 해당 파일 또는 폴더 경로**는 해당 파일이 아닌 파일들은 삭제하지 않고 패스한다.

**--prune-empty**는 빈 커밋을 제거하는 옵션이다. 한 커밋에 해당 파일만 존재할 경우 파일이 삭제된 후 빈 커밋이 되므로 이 커밋을 제거한다.

## 예시
<br>
그럼 예시를 들어보자.

![첫번째 커밋](https://user-images.githubusercontent.com/77378301/213873655-7e064f9a-b2d1-425f-adb3-698d2ab177e5.png)

![두번째 커밋](https://user-images.githubusercontent.com/77378301/213873715-12856481-65ac-47e0-bad6-5aa5f57d9ebc.png)

![세번째 커밋](https://user-images.githubusercontent.com/77378301/213873616-d7263077-3357-45af-aea7-84e0964354f9.png)

첫번째 커밋 때 first.txt파일을 커밋했기 때문에 두번째 커밋과 세번째 커밋에도 first.txt 파일이 포함되어 있다.

이 때 first.txt 파일이 삭제 해야될 파일이라고 가정하자.

터미널에서 아래 명령어를 입력한다.
```bash
git filter-branch --force --index-filter "git rm --cached --ignore-unmatch ./first.txt" --prune-empty --tag-name-filter cat -- --all
```

명령어가 잘 작동되었다.
![git filter-branch 결과](https://user-images.githubusercontent.com/77378301/213873981-19188973-b7f6-4510-bfb8-311e3ce2cde3.png)


___

## 깃허브에 강제 푸쉬
<br>
해당 명령어를 통해 푸쉬한다. 
```bash
git push origin master --force
```

**해당 명령어는 강제로 리포지토리에 푸쉬하는 것이기 때문에 신중하게 사용해야 한다.**
{: .notice--danger}
<br>

커밋 목록에서 첫번째 커밋은 사라지고 두번째 커밋과 세번째 커밋에 first.txt 파일이 사라진 것을 볼 수 있다.

![파일 삭제 후 커밋 목록](https://user-images.githubusercontent.com/77378301/213874353-76b4b641-aa19-4af7-b274-1d171aa6eff5.png)

![파일 삭제 후 두번째 커밋](https://user-images.githubusercontent.com/77378301/213874389-fdecca75-907c-4bf1-a40c-7b25a335d0d9.png)

![파일 삭제 후 세번째 커밋](https://user-images.githubusercontent.com/77378301/213874374-1906ebcd-0149-44c3-b862-850cee7f86cd.png)

<br>

**참고자료**<br> 
<https://velog.io/@guno98/Git-history%EC%97%90%EC%84%9C-%ED%8C%8C%EC%9D%BC-%EC%82%AD%EC%A0%9C%ED%95%98%EA%B8%B0><br>
{: .notice--info}