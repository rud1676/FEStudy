# 브라우저 렌더링

> author: @OOWGNOD

- 검색창에 Google.com을 입력하면 생기는 일?
  - MDN에서 찾아보았다.
- 브라우저의 렌더링 과정
  - 브라우저마다 다른 렌더링 엔진
    - 렌더링 엔진의 목표
    - 렌더링 엔진의 동작 과정(Critical Rendering Path)
  - DOM Tree와 CSSOM Tree의 생성
  - Render Tree
    - Render Tree 생성
    - Render Tree 배치
- UI가 업데이트되는 3가지 상황
  - 다시 Layout이 발생하는 경우
  - Paint부터 다시 발생되는 경우
  - 레이어의 합성만 다시 발생하는 경우
- 브라우저 최적화
  - 렌더링 성능 최적화
    - 애니메이션 최적화 (Reflow, Repaint)
  - 로딩 성능 최적화
    - 컴포넌트 Lazy Loading (Code Splitting)
    - 컴포넌트 Preloading
    - 이미지 Preloading
- React.js와 Vue.js의 렌더링

  -- WIP --

---
