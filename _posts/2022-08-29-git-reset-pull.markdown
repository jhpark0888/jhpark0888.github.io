---
layout: posts
title:  "[Git] Git pull 되돌리는 방법"
date:   2022-08-29 22:09:01 +0900
categories: git
---
<br>
<br>
github를 이용하여 프로젝트를 진행하다 보면 pull을 하는 경우가 많은데 실수로 다른 brunch를 pull하는 경우가 생길 수 있습니다.
이런 경우에는 밑의 방법을 이용해서 특정 커밋으로 돌아가서 pull 한 것을 되돌릴 수 있습니다.

```
git log --oneline
```
`git log --oneline` 을 터미널에 입력하면 아래와 같이 커밋했던 목록들이 나오게 됩니다. 

![git_log](https://user-images.githubusercontent.com/77378301/187216054-833b4383-c207-40c0-bcd8-64e27de3a923.png)

<!-- ![git_log](/assets/images/git_log.png) -->

<!-- <img src="/assets/images/git_log.png"  title="git_log"/> -->

그 중에서 가고 싶은 시기의 커밋을 고른 뒤 터미널에 아래와 같이 입력하면 됩니다.

```
git reset --hard [해당 커밋 ID]
```

이 방법은 강제로 현재 HEAD에서 추가된 변경사항들을 모두 되돌리기 때문에 신중히 사용해야 합니다.

