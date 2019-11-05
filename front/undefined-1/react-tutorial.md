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

* Node.js 설치
* yarn\(더 빠른 설치\) or npm 중 취향적인 선택
* 본인인 intellij 에서 프로젝트를 함. 

### 나의 첫번째 리액트 컴포넌트 

* 이미 클래스, 함수형 스타일의 컴포넌트를 실습했다.

### JSX 

* JSX 에서 선언하는 태그는 반드시 닫혀야한다.
* N개 이상의 태그는 하나로 감싸야하고 감싸는 용도로 보통 `<div>` 를 사용하는 편이나 이마저도 싫다면 편법으로는 fragment를 사용한다. 
* javascript 변수를 JSX 내에서 사용하고 자하는경우 `{}`를 사용하면 된다.
* JSX 내에서 style 은 객체 형태로 지정해야한다. 그리고 camelCase 형식으로 지정해야한다.
* JSX 내에서 css의 class 를 지하는 것은 className으로 사용해야 한다.
* 주석을 작성하는 법은 제시해줬으나 사용하고 싶지 않다. `{}` 사용

### props 를 통해 컴포넌트에게 값 전달하기 

#### props의 개요

```javascript
import React from 'react';

function Hello(props) {
  return <div>안녕하세요 {props.name}</div>
}

export default Hello;
```

* 부모에서 자식 컴포넌트로 값을 전달하는 용도로 사용한다.

#### 번거로운 props 과정을 생략하는 비 구조화 할 

```javascript
import React from 'react';

function Hello(props) {
  return <div style={{ color: props.color }}>안녕하세요 {props.name}</div>
}

export default Hello;
```

* 매번 위와 같은 구조로 `props.` 과 같은 과정으로 호출하게되면 상당히 귀찮기 마련이다. 비 구조화 할당을 통해서 바로 사용하고자 하는 값을 곧바로 쓸 수도 있다. 
* 아래와 같은 구조로 사용하는 것이 비 구조화 할당이라 명명한다.

```javascript
import React from 'react';

function Hello({ color, name }) {
  return <div style={{ color }}>안녕하세요 {name}</div>
}

export default Hello;
```

#### defaultProps 로 기본값을 할당해보자

```javascript
import React from 'react';

function Hello({ color, name }) {
  return <div style={{ color }}>안녕하세요 {name}</div>
}

Hello.defaultProps = {
  name: '이름없음'
}

export default Hello;
```

* 다음과 같이  Hello 컴포넌트에 defaultProps 를 활용하는 법을 배웠다.

#### `props.children` 로 컴포넌트 태그사이의 값 조회하

```javascript
import React from 'react';

function Wrapper() {
  const style = {
    border: '2px solid black',
    padding: '16px',
  };
  return (
    <div style={style}>

    </div>
  )
}

export default Wrapper;
```

* 컴포넌트를 래핑하기 위한 용도의 별도의 컴포넌트이다.

```javascript
import React from 'react';
import Hello from './Hello';
import Wrapper from './Wrapper';

function App() {
  return (
    <Wrapper>
      <Hello name="react" color="red"/>
      <Hello color="pink"/>
    </Wrapper>
  );
}

export default App;
```

* `Wrapper` 를 특정 컴포넌트에서 사용하려면 위와 같은 형태에서는 사용이 불가하기 때문에 `Wrapper`에서 `props.children` 을 활용해야한다.

```javascript
import React from 'react';

function Wrapper({ children }) {
  const style = {
    border: '2px solid black',
    padding: '16px',
  };
  return (
    <div style={style}>
      {children}
    </div>
  )
}

export default Wrapper;
```

* 이전 항목에서 배운 비구조화 할당을 활용하면 위와 같이 간략하게 작성이 가능해졌다.

### 조건부 렌더링 

#### isSpecial 사용자 정의 props 설정 및 활용해보기

```javascript
import React from 'react';
import Hello from './Hello';
import Wrapper from './Wrapper';


export default function App() {
  return (
    <Wrapper>
      <Hello name="react" color="red" isSpecial={true}/>
      <Hello name="react" color="red" isSpecial/>
      <Hello color="pink" />
    </Wrapper>
  )
}
```

* 간단한 삼항 연산자로 조건부 렌더링을 실습해보았다.
* 다양하게 props 값을 설정할 수 있으나, 자바스크립트 값을 쓰기위해선 `{}`를 사용한다. 
* 또한 props 값을 생략하면 default value 는 `true` 이다.

#### 조금더 축약된 표현을 활용해 본다면..

```javascript
import React from 'react';

export default function Hello({color, name, isSpecial}){
    return (
      <div style={{color}}>
          {isSpecial ? <p>*</p> : null}
          {isSpecial && <p>*</p>}
          안녕하세요 {name}
      </div>
    );
}
Hello.defaultProps = {
    name : '이름 없음'
};
```

* 우리가 단순히 보이고 안 보이고 수준의 표기용도이라면 `&&` 연산자를 고려해보는 것도 나쁘지 않다.

### useState 를 통해 컴포넌트에서 바뀌는 값 관리하기 

#### 사용자 인터렉션에 변화에 따른 구현 방법

* 리액트 16.8 버전 이전에는 함수형 컴포넌트로 state를 관리할 수 없었다.
* 이후 버전부터 Hooks의 등장으로 정식적인 기능은 아니였으나 많은 리액트 유저들이 이용한다.
* 맛보기로 Hooks의 useState 를 활용하여  함수형 컴포넌트의 동적인 부분에 대응해본다.

{% tabs %}
{% tab title="배열 비구조화 할당하여 useState 활용" %}
```javascript
import React, {useState} from 'react'

export default function Counter() {
    const [number,setNumber] = useState(0);
    const onIncrease = () => {
        setNumber(number+1);
    };
    const onDecrease = () => {
        setNumber(number-1);
    };
    return (
      <div>
          <h1>{number}</h1>
          <button onClick={onIncrease}>+1</button>
          <button onClick={onDecrease}>-1</button>
      </div>
    );
}
```
{% endtab %}

{% tab title="직접 선언하여 사용" %}
```javascript
import React, {useState} from 'react'

export default function Counter() {
    const numberState = useState(0);
    const number = numberState[0];
    const setNumber = numberState[1];
    const onIncrease = () => {
        setNumber(number+1);
    };
    const onDecrease = () => {
        setNumber(number-1);
    };
    return (
      <div>
          <h1>{number}</h1>
          <button onClick={onIncrease}>+1</button>
          <button onClick={onDecrease}>-1</button>
      </div>
    );
}
```
{% endtab %}
{% endtabs %}

* 엘리먼트에 이벤트를 설정 시 `xxxMethod()`와 같은 형태 호출하여로 넣게되면 `DOM`이 렌더링 되기 전에 실행되버리므로 오류가 난다. 

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

