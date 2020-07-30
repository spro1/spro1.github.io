---
layout: post
title:  "git 기본 명령어 모음"
date:   2020-07-30
categories: git github
---
last update : 20200730

Git 기본 명령어 모음 - 추가로 업로드 예정

### 사용 명령어

|명령어|설명|
|:-----|:------------|
|git init|저장소 생성|
|git add <파일명>|파일을 git에 추가|
|git commit|변경 사항 git에 업로드|
|git status|저장소 상태 출력|
|git branch <브랜치명>|브랜치 생성|
|git checkout <브랜치명>|작업 브랜치 변경|
|git merge <브랜치명>|현재 브랜치에 <브랜치명> 병합|
|git clone|원격 저장소 -> 로컬 저장소 복사|
|git remote|특정 원격 저장소 연결|
|git push|로컬 저장소의 변경사항을 원격 저장소에 저장|
|git fetch|로컬 저장소와 원격 저장소의 변경사항 대조|
|git pull|원격 저장소의 최신 내용을 로컬 저장소로 병합|
|git tag|tag 붙이기|
|git commit --amend|최종 커밋 취소 후 새로운 내용 추가|
|git revert|이전 커밋 제거, 기록 남음|
|git reset|이전 커밋 버전으로 돌아감, 기록 안남음|
|git checkout HEAD --filename|아직 커밋하지 않은 변경 내용 취소|
|git rebase|병합할때 사용|
|git rebase -i|서로 다른 두 커밋의 내역을 합침|


### 일반적인 github 사용법

```
// 저장소 생성
git init

// readme.md 생성
echo "# test" >> README.md

// readme 추가
git add README.md

// commit
git commit -m "first commit"

// 원격저장소 추가
git remote add <리모트명> <github 주소>

// 변경사항 원격저장소 업로드
git push -u <리모트명> <브랜치명>

```