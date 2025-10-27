---
title: "Submodules"
categories: [ "Git & GitHub" ]
tags: [ "[인프런] 제대로 파는 Git & GitHub - by 얄코", "git submodule" ]
date: "2025-10-17 03:16:00"
---

## Submodule

- 프로젝트 폴더 안에 또 다른 프로젝트가 포함될 때 사용
- 여러 프로젝트에 사용되는 공통 모듈일 때 유용

## Submodule로 등록하기

```bash
git submodule (서브모듈 원격 저장소 주소) (서브 모듈을 받아올 위치, 없을 시 생략)
```

- `.gitmodules` 라는 파일이 생기고 이 파일을 통해 서브 모듈이 연결됨

이제 서브 모듈의 변경되어도 메인 모듈은 간섭 하지 않습니다.

파일의 변경은 간섭하지 않지만, `commit` 한 `commit` 기록은 간섭을 하기 때문에
서브 모듈에서 `commit`을 했다면 메인 모듈에서도 `commit`을 해서 서브 모듈 `commit`을 최신으로 유지하여야 함

## 메인 모듈에 기록된 커밋으로 서브모듈 맞추기

```bash
git submodule update
```

## GitHub 최신 커밋으로 맞추기

```bash
git submodule update --remote
```

## 클론 받기

- 메인 모듈을 클론 받으면 `submodule`은 껍데기만 있음
- 이때, 아래 명령어를 통해 받아와야 함

```bash
git submodule init (서브모듈명, 생략 시 전체)
```

```bash
git submodule update
```

## 참고

- [https://www.inflearn.com/course/제대로-파는-깃](https://www.inflearn.com/course/%EC%A0%9C%EB%8C%80%EB%A1%9C-%ED%8C%8C%EB%8A%94-%EA%B9%83)
