---
description: 튜토리얼
---

# 리액트 입문

### 최신 흐름을 추구하는 리액트 강좌

* 프론트 영역의 변화는 극심하지만 최신 흐름을 파악하는 것을 목표로 함.
* 현재 상황에서는 클래스 컴포넌트의 도퇴되는 과정과, Hooks 와 Functional Component 의 부흥이 꼽혀진다 \(2019년 기준\)

### 리액트는 어쩌다가 만들어졌을까? 

```markup
<h2 id="number">0</h2>
<div>
  <button id="increase">+1</button>
  <button id="decrease">-1</button>
</div>
```

```javascript
const number = document.getElementById('number');
const increase = document.getElementById('increase');
const decrease = document.getElementById('decrease');

increase.onclick = () => {
  const current = parseInt(number.innerText, 10);
  number.innerText = current + 1;
};

decrease.onclick = () => {
  const current = parseInt(number.innerText, 10);
  number.innerText = current - 1;
};
```

* 간단한 카운트를 행하는 소스다.
*  `DOM` 을 직접 다루거나 이벤트가 더 많아 지는 경우에는 관리하기 매우 까다로워 진다.
* `react`는 대상이 되는 `component`를  특정한 규칙에 의해서 업데이트를 하는 것이 아닌, 새로 만드는 것에 대해서 사상을 가지고  있다.
* 매번 복잡한 구조의 컴포넌트를 새로 만든다면 시간 낭비일 수 있으나, `virtual DOM` 이라는 것을 통해서 성능적인 이슈도 해결했다고 한다. 

![&#xB9AC;&#xC561;&#xD2B8;&#xC758; DOM Rendering &#xACFC;&#xC815;](../../.gitbook/assets/image%20%284%29.png)

* `react`가 DOM을 Rendering 하는 과정
  * RealDOM &lt;--&gt; VirtualDOM 비교 과정
  * 일치하지 않는 것을 patch 과정을 통해서 부분적인 업데이트를 진
  *  이로서 전체적인 것을 다시 그리는 행위를 취하지 않고 성능도 지켜나갈 수 있다.
  * 고로 어떻게 업데이트 하냐의 관점이 아닌 어떻게 보여줄 지에 집중이 필요하다고 한다.

### 작업환경 준비 

### 나의 첫번째 리액트 컴포넌트 

### JSX 

### props 를 통해 컴포넌트에게 값 전달하기 

### 조건부 렌더링 

### useState 를 통해 컴포넌트에서 바뀌는 값 관리하기 

### input 상태 관리하기 

### 여러개의 input 상태 관리하기 

### useRef 로 특정 DOM 선택하기 

### 배열 렌더링하기 

### useRef 로 useRef 로 컴포넌트 안의 변수 만들기 

### 배열에 항목 추가하기 

### 배열에 항목 제거하기 

### 배열에 항목 수정하기 

### useEffect를 사용하여 마운트/언마운트/업데이트시 할 작업 설정하기 

### useMemo 를 사용하여 연산한 값 재사용하기 

### useCallback 를 사용하여 함수 재사용하기 

### React.memo 를 사용한 컴포넌트 리렌더링 방지 

### useReducer 를 사용하여 상태 업데이트 로직 분리하기 

### 커스텀 Hooks 만들기 

### Context API 를 사용한 전역 값 관리 

### Immer 를 사용한 더 쉬운 불변성 관리 

### 클래스형 컴포넌트 

### LifeCycle Method 

### componentDidCatch 로 에러 잡아내기 / Sentry 연동 

### 리액트 개발 할 때 사용하면 편리한 도구들 - Prettier, ESLint, Snippet 

### 리액트 입문 마무리

