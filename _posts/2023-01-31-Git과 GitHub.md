---
title: Git과 GitHub
date: 2023-01-31 17:10:00 +09:00
categories: [전산 기초, 개발 상식]
tags: [git, gitHub]
---
<br><br>

## Git이란?

Git은 많은 기능을 가진 버전 관리 도구로서, 오픈소스에 기여하거나 여러 사람과 공동 작업을 하기 위해서 꼭 필요로 한다. 

Linux 프로젝트 중 버전 관리 소프트웨어의 필요성을 느끼고 만들어 졌으며 초기 개발자는 Linux의 개발자인 리누스 토발즈이다. 

<br><br>

<br><br>

### Git 개발 목표

- 빠른 속도
- 단순한 구조
- 비선형적인 개발(수천 개의 동시 다발적인 브랜치)
- 완벽한 분산(DVCS)
- Linux 커널 같은 대형 프로젝트에도 유용할 것(속도나 데이터 크기 면에서)



이러한 목표를 가지고 Git 에 브랜치 시스템을 도입하였고, 원격 저장소와 로컬을 분리하여 여러 개발자가 분산 작업을 원활하게 할 수 있게 되었다.

<br><br>

<br><br>

### Git의 동작 원리

GIT은 기본적으로 파일 시스템의 스냅샷(커밋 당시의 Git 디렉토리 내의 모든 파일 정보를 저장)을 저장하고 스냅샷을 해시(Checksum)하여 빠르게 바뀐 버전인지 아닌 지를 체크한다.

이후 스냅샷들의 크기가 커지면 주기적으로 git gc(garbage collection)을 통해 delta를 만들어 관리한다.

<br><br>

<br><br>



### Git과 GitHub의 차이

|                             Git                              |                            GitHub                            |
| :----------------------------------------------------------: | :----------------------------------------------------------: |
| 내 로컬 저장소를 사용해서 버전 관리, 다른 사람과 실시간으로 작업 공유가 불가능 | 클라우드 서버를 이용해 로컬 버전관리, 소스코드를 업로드해 다른 사람과 공유가 가능 |
|                      버전관리 프로그램                       | 버전관리, 소스코드 공유, 분산 버전 제어 등이 가능한 원격 저장소 |

<br><br>

<br><br>

### Git의 대표적인 명령어

Git의 명령어는 워낙 다양하기 때문에 모든 명령어를 활용하려면 심도있게 공부해야할 것 같다.

<br><br>

**git init** => Git 디렉토리 시작

**git add *** => Git으로 관리할 파일 선택

**git commit** => 메시지와 함께 디렉토리의 상태 저장

**git push** => 원격 저장소에 커밋 내용 전송

**git checkout** => 브랜치의 상태로 디렉토리를 변경/사용할 브랜치 지정

**git branch {name}** => 새로운 브랜치 생성

**git reset --hard HEAD** => 마지막 커밋으로 되돌리기

**git merge {branch-name}** => 현재 체크아웃된 브랜치를 기준으로 name을 머지

**git status** => 파일 상태 확인

**git status -s** => 파일 상태 간단하게 보기

<br><br>

>  더 많은 명령어는 [Git을 조금 더 알아보자!](https://www.slideshare.net/ky200223/git-89251791) 참고

<br><br>

<br><br>

## Git vs GitHub vs GitLab Flow

> git-flow의 종류는 크게 세 가지로 분리되는데 어떤 차이점이 있을까?

<br><br>

### 1. Git Flow

가장 최초로 제안된 Workflow 방식으로, 대규모 프로젝트에 적합한 방식이라고 평가된다.

<br><br>

#### 기본 브랜치 

- feature :arrow_right: develop :arrow_right: release :arrow_right: hotfix :arrow_right: master

  

<img src="/assets/img/cs/git.png">

##### Feature

> Develop 브랜치에서 기능 구현을 할 때 만드는 브랜치

한 기능 단위마다 Feature 브랜치를 생성하는게 원칙이다.

<br><br>

##### Develop

> 다음 릴리즈 버전 개발을 진행하는 브랜치

추가 기능 구현이 필요해지면, 해당 브랜치에서 다시 브랜치(Feature)를 내어 개발을 진행하고, 완료된 기능은 다시 Develop 브랜치로 Merge한다.

<br><br>

##### Release

> Develop에서 파생된 브랜치

Master 브랜치로 현재 코드가 Merge 될 수 있는지 테스트하고, 이 과정에서 발생한 버그를 고치는 공간이다. 확인 결과 이상이 없다면, 해당 브랜치는 Master와 Merge한다.

<br><br>

##### Hotfix

> Mater브랜치의 버그를 수정하는 브랜치

검수를 해도 릴리즈된 Master 브랜치에서 버그가 발견되는 경우가 존재한다. 이때 Hotfix 브랜치를 내어 버그 수정을 진행한다. 디버그가 완료되면 Master, Develop 브랜치에 Merge해주고 브랜치를 닫는다.

<br><br>

##### Master

> 릴리즈 시 사용하는 최종 단계 메인 브랜치

Tag를 통해 버전 관리를 한다.

<br><br>

Git-Flow에서 가장 중심이 되는 브랜치는 `master`와 `develop`이다. (꼭 있어야 함)

> 브랜치 명을 바꿀 수는 있지만 통상적으로 사용하는 이름을 그대로 따르는 것을 추천한다.
{: .prompt-tip}

<br><br><br><br>

### 2. GitHub Flow

git-flow를 개선하기 위해 나온 하나의 방식으로 흐름이 단순한 만큼, 역할도 단순하다. git flow의 `hotfix`나 `feature` 브랜치를 구분하지 않고, pull request를 권장한다.

<img src="/assets/img/cs/github.png">

`master`브랜치가 릴리즈에 있어 절대적인 역할을 한다. `master `브랜치는 항상 최신으로 유지하여야 하며, Stable한 상태로 Product에 배포되는 브랜치다.

따라서 Merge를 하기 전에 충분한 테스트 과정을 거쳐야만 한다. 



새로운 브랜치는 항상 `master ` 브랜치에서 만들어야 하고 새로운 기능 추가나 버그 해결을 위한 브랜치라면 이름을 명확하게 지어주고 커밋 메세지 또한 명시적으로 작성해야 한다.



그리고 Merge하기 전에 항상 pull request를 통해 공유하여 코드 리뷰를 진행한다. 이를 통해 피드백을 받고 준비가 완료되면 `master ` 브랜치로 요청을 하게 된다.

> 여기서 merge는 바로 product에 반영되므로 충분한 논의가 필요하며 **CI**도 필수적이다.

merge가 완료되면 push를 진행하고 자동으로 배포가 완료된다.

<br><br>

> #### CI(Continuous Integration)
>
> - 형상 관리 항목에 대한 선정과 형상 관리 구성 방식 결정
> - 빌드/배포 자동화 방식
> - 단위 테스트/ 통합 테스트 방식
>
> 이 세 가지를 모두 고려한 자동화된 프로세스를 구성하는 것
{: .prompt-info}

<br><br>

### 3. GitLab Flow

github flow의 간단한 배포 이슈를 보완하기 위해 관련 내용을 추가로 덧붙인 flow 방식이다.

<img src="/assets/img/cs/gitlab.png">

Production 브랜치가 존재하며 커밋 내용을 일방적으로 Deploy(배치)하는 형태를 갖추고 있다.

`master` 브랜치와 `Production` 브랜치 사이에 `pre-production` 브랜치를 두어 개발 내용을 바로 반영하지 않고, 시간을 두고 반영한다. 이를 통한 이점은, Production 브랜치에서 릴리즈된 코드가 항상 프로젝트의 최신 버전 상태를 유지할 필요가 없는 것이다.

즉, github-flow의 단점인 안정성과 배포 시기 조절에 대한 부분을 production이라는 추가 브랜치를 두어 보강하는 전력이라고 볼 수 있다.



<br><br>



<br><br>

### 정리

위의 세가지 방법 모두 사용이 가능하며, 무엇이 더 나은 방법이라고 말할 수는 없다. 한 방식을 사용하다 변경하는 경우도 있고 각자 상황에 맞추어 사용하면 될 것 같다.



<br><br>

<br><br>



> #### 참고
>
> [Git vs GitHub vs GitLab Flow](https://github.com/gyoogle/tech-interview-for-developer/blob/master/ETC/Git%20vs%20GitHub%20vs%20GitLab%20Flow.md)
>
> [Git을 조금 더 알아보자!](https://www.slideshare.net/ky200223/git-89251791)

<br><br>

