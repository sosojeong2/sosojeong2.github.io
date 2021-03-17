---
layout: post
title: "[Board]게시판만들기-curl구현"
date: 2020-12-14 17:47:23 +0900
category: board_project
---

# 게시판만들기 - curl구현

## Service 
```java

    // 게시글 등록
    public String boardRegister(BoardDto dto) {
        dao.register(dto);
        return dao.latelyId();

    }

    // 게시글 전체 조회
    public List<BoardDto> boardQuery(Criteria cri) {
        List<BoardDto> response = dao.query(cri);
        return response;
    }

    // 게시글 상세보기 조회
    public BoardDto boardDetail(String id) {
        BoardDto response = dao.detail(id);
        return response;
    }

    // 조회수 증가
    public void plusViews(String id) {
        dao.plusViews(id);
    }

    // 게시글 수정
    public void boardUpdate(BoardDto dto) {
        dao.update(dto);
    }

    // 게시글 삭제
    public void boardDelete(String id) {
        dao.delete(id);
    }

```
