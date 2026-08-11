# 2026-git-start
GitHub 와   로컬   저장소의 Merge  충돌   해결   실습

- 로컬 컴퓨터에서 추가한 내용입니다.
- GitHub 웹에서 추가한 내용입니다.

## 오늘의 학습 목표: Git 협업 이해
- 작업자 A·B의 Git 협업 및 Merge 충돌 해결

## Git 협업 시퀀스 다이어그램

```mermaid
sequenceDiagram
    autonumber

    participant A as 작업자 A 로컬 저장소
    participant G as GitHub 원격 저장소
    participant B as 작업자 B 로컬 저장소

    Note over A,B: A와 B는 같은 GitHub 저장소를 사용하지만<br/>각 로컬 저장소는 서로 독립적이다.

    A->>A: worker-a.md 생성 및 수정
    A->>A: git add worker-a.md
    A->>A: git commit -m "A: 작업자 A 문서 추가"
    A->>G: git push
    G-->>A: origin/main 업데이트 완료

    B->>G: git fetch origin
    G-->>B: 최신 원격 커밋 정보 전달
    B->>B: git merge origin/main

    B->>B: worker-b.md 생성 및 수정
    B->>B: git add worker-b.md
    B->>B: git commit -m "B: 작업자 B 문서 추가"
    B->>G: git push
    G-->>B: origin/main 업데이트 완료

    A->>G: git fetch origin
    G-->>A: B의 최신 커밋 정보 전달
    A->>A: git merge origin/main

    Note over A,B: 작업자 A, 작업자 B, GitHub가<br/>같은 최신 커밋 상태가 됨
```

## 같은 파일을 수정하여 충돌 발생

```mermaid
sequenceDiagram
    autonumber

    participant A as 작업자 A 로컬 저장소
    participant G as GitHub 원격 저장소
    participant B as 작업자 B 로컬 저장소

    Note over A,B: 충돌 실습 시작 전<br/>A와 B는 동일한 커밋 상태

    A->>A: README.md 수정
    A->>A: git add README.md
    A->>A: git commit -m "A: README 학습 목표 수정"
    A->>G: git push
    G-->>A: A의 커밋 반영

    B->>B: README.md의 같은 문장 수정
    B->>B: git add README.md
    B->>B: git commit -m "B: README 학습 목표 수정"

    B->>G: git push
    G--xB: Push 거절<br/>fetch first 또는 non-fast-forward

    B->>G: git fetch origin
    G-->>B: A의 최신 커밋 정보 전달

    B->>B: git merge origin/main
    B--xB: README.md Merge 충돌 발생

    B->>B: README.md 충돌 해결
    B->>B: git add README.md
    B->>B: git commit -m "B: A의 변경과 README 충돌 해결"

    B->>G: git push
    G-->>B: 충돌 해결 결과 반영

    A->>G: git fetch origin
    G-->>A: 최종 Merge Commit 전달
    A->>A: git merge origin/main

    Note over A,B: A, B, GitHub가 최종 상태로 동기화됨
```
