---
layout: posts
title:  "[Git] 깃 커밋 수정하는 법"
date:   2023-01-21 16:00:00 +0900
categories: git
---
<br>
<br>
코드를 작성하고 깃허브에 코드를 올리다보면 실수로 커밋 메세지를 잘못 입력하거나 잘못된 내용을 커밋하는 경우가 있을 수 있다.

이런 경우에 ```git rebase -i <수정할 커밋 시점 전 커밋>``` 명령어를 통해 실수를 바로 잡을 수 있다.

바로 직전 커밋을 수정하는 경우에는 ```git commit --amend``` 명령어를 통해 더 쉽게 수정할 수 있다.

## 바로 직전 커밋 수정
<br>
우선 커밋 메세지를 수정하기 위해 위와 같이 3개의 커밋을 만들었다. 

![git_log_pretty_oneline](https://user-images.githubusercontent.com/77378301/213856875-d2c20b04-e236-46e3-aaa7-4a4c7da548e0.png)

```git log --pretty=oneline``` 명령어를 이용하면 git log를 한 줄로 볼 수 있다.
{: .notice--info}
<br>

바로 직전 커밋은 third.txt 파일을 만들어 줬고 이미지와 같이 텍스트를 적고 Third Commit이라고 커밋메세지를 적었다.

나는 이 커밋에서 **Hello**라는 텍스트를 **안녕하세요**로 바꾸고 **Third Commit**이라는 커밋메세지를 **세번째 커밋**이라고 바꿀 것이다.

![바로 직전 커밋](https://user-images.githubusercontent.com/77378301/213858735-4d088f68-be66-46dc-a646-c5daa382ca76.png)

우선 third.txt 파일의 Hello를 안녕하세요로 바꿔준다.
그 후 아래와 같이 명령어를 입력해준다.

```bash
git add .
git commit --amend
```

명령어를 입력하면 편집기가 열리게 된다. 
![커밋 메세지 수정 전](https://user-images.githubusercontent.com/77378301/213858910-e43b1013-5709-45ac-adc4-d39cfabc5920.png)

**s**키를 누르면 커밋 메세지를 변경할 수 있게 되고 나는 아래와 같이 바꿔 주었다.
![커밋 메세지 수정 후](https://user-images.githubusercontent.com/77378301/213859087-f184b258-423f-435d-b4a1-0277cfdae73b.png)

수정을 다 했으면 **esc키**를 눌러 변경을 종료하고 **:wq**를 눌러 편집기를 나온다.

```git log```를 이용해서 커밋 기록을 보면 커밋 메세지가 바뀌어 있는 것을 볼 수 있다.
![커밋 메세지 수정 후 git log](https://user-images.githubusercontent.com/77378301/213859308-586dc742-9393-4393-9daa-537a59316df9.png)

해당 명령어를 통해 푸쉬하면 리포지토리에 커밋도 바뀐 것을 볼 수 있다.
```bash
git push origin master --force
```

**해당 명령어는 강제로 리포지토리에 푸쉬하는 것이기 때문에 신중하게 사용해야 한다.**
{: .notice--danger}

<br>

![직전 커밋 수정 후 파일 확인](https://user-images.githubusercontent.com/77378301/213859644-c5030e28-57c0-4432-b6b0-ef9ce2e0e8a3.png)

___

## 특정 커밋 수정
<br>
나는 이 커밋에서 **반갑습니다**라는 텍스트를 **안녕하세요**로 바꾸고 **Second Commit**이라는 커밋메세지를 **두번째 커밋**이라고 바꿀 것이다.

![두번째 커밋](https://user-images.githubusercontent.com/77378301/213859865-18518e87-874c-4adf-ba1e-02cc7adb7b7a.png)

<br>

우선 두번째 커밋을 고치기 위해 첫번째 커밋의 아이디를 알아야 하기 때문에 ```git log --pretty=oneline``` 명령어를 통해 첫번째 커밋의 아이디를 알아낸다.

![git log](https://user-images.githubusercontent.com/77378301/213860040-3df7041f-4e33-43df-b278-13a79affe64c.png)

<br>

첫번째 커밋의 아이디를 알아냈으면 아래 명령어를 통해 편집기를 열어준다.
```bash
git rebase -i 8b238c185871fef9f8ba3400586398c65edbc7df
```

![git rebase -i 편집기](https://user-images.githubusercontent.com/77378301/213860243-412a3c1a-3d02-455e-a60d-b079318e0b36.png)

편집기를 보면 많은 명령어가 있는데 이번에는 커밋을 수정할 것이기 때문에 **e or edit**을 사용할 것이다.

**e or edit** 명령어는 커밋 메세지와 작업 내용을 수정할 수 있게 해준다.

**다른 명령어들의 사용법은 맨 아래의 첫번째 참조 블로그를 보면 알 수 있다.**
{: .notice--info}

<br>

![두번째 커밋 git rebase edit으로 변경](https://user-images.githubusercontent.com/77378301/213860350-c3154cc9-5c6b-4548-93e3-8f074e80acef.png)

**s**키를 누르고 변경이 가능해지면 pick을 edit으로 바꾸고 **esc**키를 눌러 변경을 종료하고 **:wq**를 눌러 편집기를 나온다.

그러면 해당 커밋으로 HEAD가 옮겨지게 되고 이제 파일을 수정하면 된다.
수정을 완료했으면 아래 명령어를 입력하여 커밋을 진행한다.

```bash
git add . (파일 수정을 했을 때만 입력)
git commit --amend
```

명령어를 입력하면 아까와 같이 편집기가 나오게 되고 커밋 메세지를 수정하고 **:wq**를 눌러 편집기를 나온다.

![두번째 커밋 메세지 변경](https://user-images.githubusercontent.com/77378301/213860825-f64d7938-6940-430c-9783-85e612ca181a.png)

마지막으로 ```git rebase --continue``` 명령어를 입력하여 리베이스 과정을 종료한다.
```bash
git rebase --continue
```

그 후 리포지토리에 푸쉬하면 아래와 같이 바뀐 것을 확인 할 수 있다.
![특정 커밋 수정 후 파일 확인](https://user-images.githubusercontent.com/77378301/213861041-5cdc44ba-9385-4caf-aea1-47e174d05dbf.png)

<br>

**참고자료**<br> 
<https://mizzo-dev.tistory.com/entry/git-commit-edit><br>
<https://wormwlrm.github.io/2020/09/03/Git-rebase-with-interactive-option.html><br>
<https://github.com/HomoEfficio/dev-tips/blob/master/Git%20%EA%B3%BC%EA%B1%B0%EC%9D%98%20%ED%8A%B9%EC%A0%95%20%EC%BB%A4%EB%B0%8B%20%EC%88%98%EC%A0%95%ED%95%98%EA%B8%B0.md>
{: .notice--info}