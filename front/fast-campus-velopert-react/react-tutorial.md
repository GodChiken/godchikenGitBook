---
description: 튜토리얼
---

# 리액트 입문

### 최신 흐름을 추구하는 리액트 강좌

* 프론트 영역의 변화는 극심하지만 최신 흐름을 파악하는 것을 목표로 함.
* 현재 상황에서는 클래스 컴포넌트의 도퇴되는 과정과, Hooks 와 Functional Component 의 부흥이 꼽혀진다 \(2019년 기준\)

### 리액트는 어쩌다가 만들어졌을까? 

```javascript
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

![&#xB9AC;&#xC561;&#xD2B8;&#xC758; DOM Rendering &#xACFC;&#xC815;](../../.gitbook/assets/image%20%2812%29.png)

* `react`가 `DOM`을 `Rendering` 하는 과정
  * RealDOM &lt;--&gt; VirtualDOM 비교 과정
  * 일치하지 않는 것을 patch 과정을 통해서 부분적인 업데이트를 진
  *  이로서 전체적인 것을 다시 그리는 행위를 취하지 않고 성능도 지켜나갈 수 있다.
  * 고로 어떻게 업데이트 하냐의 관점이 아닌 어떻게 보여줄 지에 집중이 필요하다고 한다.

### 작업환경 준비 

* `Node.js` 설치
* `yarn`\(더 빠른 설치\) or `npm` 중 취향적인 선택
* 본인인 intellij 에서 프로젝트를 함. 

### 나의 첫번째 리액트 컴포넌트 

* 이미 클래스, 함수형 스타일의 컴포넌트를 실습했다.

### JSX 

* `JSX` 에서 선언하는 태그는 반드시 닫혀야한다.
* N개 이상의 태그는 하나로 감싸야하고 감싸는 용도로 보통 `<div>` 를 사용하는 편이나 이마저도 싫다면 편법으로는 fragment를 사용한다. 
* `javascript` 변수를 `JSX` 내에서 사용하고 자하는경우 `{}`를 사용하면 된다.
* `JSX` 내에서 style 은 객체 형태로 지정해야한다. 그리고 camelCase 형식으로 지정해야한다.
* `JSX` 내에서 `css`의 class 를 지하는 것은 className으로 사용해야 한다.
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

#### 번거로운 props 과정을 생략하는 비 구조화 할당

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

#### `defaultProps`로 기본값을 할당해보자

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

* 다음과 같이  `Hello` 컴포넌트에 `defaultProps` 를 활용하는 법을 배웠다.

#### `props.children` 로 컴포넌트 태그사이의 값 조회하기

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

#### `isSpecial` 사용자 정의 `props` 설정 및 활용해보기

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
* 다양하게 `props` 값을 설정할 수 있으나, 자바스크립트 값을 쓰기위해선 `{}`를 사용한다. 
* 또한 `props` 값을 생략하면 default value 는 `true` 이다.

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

### `useState` 를 통해 컴포넌트에서 바뀌는 값 관리하기 

#### 사용자 인터렉션에 변화에 따른 구현 방법

* 리액트 16.8 버전 이전에는 함수형 컴포넌트로 `state`를 관리할 수 없었다.
* 이후 버전부터 Hooks의 등장으로 정식적인 기능은 아니였으나 많은 리액트 유저들이 이용한다.
* 맛보기로 `Hooks`의 `useState` 를 활용하여  함수형 컴포넌트의 동적인 부분에 대응해본다.

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

### `input` 상태 관리하기 

```javascript
import React,{useState} from 'react'

export default function InputSample() {
    const [text, setText] = useState('');

    const onChange = (e) => {
        setText(e.target.value);
    };

    const onReset = () => {
        setText('');
    };

    return (
        <div>
            <input onChange={onChange} value={text}/>
            <button onClick={onReset}>초기화</button>
            <div>
                <b>값: {text}</b>
            </div>
        </div>
    );
}
```

### 여러개의 `input` 상태 관리하기 

```javascript
import React,{useState} from 'react';

export default function InputSample() {
    const [inputs, setInputs] = useState({
       name: '',
       nickname: ''
    });

    const {name, nickname} = inputs; // 비 구조화 할당으로 값 추출 : {name: "", nickname: ""}
    console.log(inputs);
    const onChange = (e) => {
        const {value, name} = e.target;
        setInputs({
           ...inputs, // 기존 객체를 복사후
           [name]: value //name 키를 가진 값을 value 로 변경
        });
    };
    const onReset = () => {
        setInputs({
            name: '',
            nickname: ''
        })
    };

    return (
        <div>
            <input name="name" onChange={onChange} value={name} placeholder="이름" />
            <input name="nickname" onChange={onChange} value={nickname} placeholder="닉네임" />
            <button onClick={onReset}>초기화</button>
            <div>
                <b>값: </b>
                {name} ({nickname})
            </div>
        </div>
    );
}
```

* 여러 개의 `DOM`을 다룰 경우 단순히 `useState`, `onChange` 를 여러개 만드는 방법이 쉬울 수 있으나 좋은 방법은 아니다. 해서 `name`을 설정하여 이벤트 발생시 이 값을 참조하여 관리하는 방법이다.
* 리액트에서는 다음과 같은 연관배열이나 프로퍼티를 통해직접적인 수정을 하면 안된다.
  * `inputs[name] = value`
* 대신에 새로운 객체를 생성하여 새 객체에 변화를 주고 이것을 상태로 사용해야 한다.
  * ```
    setInputs({
        ...inputs, 
        [name]: value
    });
    ```
* 위와 같이 기존 객체를 통해서 새로운 객체를 생성해 나가며 불변성을 지켜나가야만 리액트 컴포넌트에서 상태가 업데이트 됨을 감지하고 필요해 의하여 리렌더링이 진행된다.
* 만약 직접 수정하는 경우 값이 바뀌어도 리렌더링이 되지 않는다.

#### 코드에서 \[name\] : value, name:value 차이가 무엇인가요?

![\[name\] , name &#xC758; &#xCC28;&#xC774;&#xC810;](../../.gitbook/assets/image%20%2839%29.png)

* 궁금할때 직접 찍어보는 것도 하나의 답이 된다.

### `useRef` 로 특정 `DOM` 선택하기 

#### `DOM`을 선택해야하는 상황과 리액트에서 처리하는 방향

* 리액트에서 직접적으로 `DOM`을 선택해야 하는 상황이 있다.
  * 엘리먼트의 크기, 위치, 포커스 설정
  * 외부 라이브러리
* 위와 같은 상황일 경우 리액트에서는 `ref` 라는 것을 사용한다.
* 함수형 컴포넌트의 경우 `Hook`의 `useRef` 함수를 사용.
* 클래스형 컴포넌트의 경우 `React.createRef` 함수를 사용.

#### 특정 `DOM`의 포커싱에 대해 처리해보자

```javascript
import React, {useState, useRef} from 'react'

export default function InputSample() {
    const [inputs, setInputs] = useState({
        name: '',
        nickname: ''
    });
    const nameInputs = useRef();
    const {name, nickname} = inputs;

    const onChange = e => {
        const {value, name} = e.target;
        setInputs({
            ...inputs,
            [name]: value
        })
    };

    const onReset = () => {
        setInputs({
            name: '',
            nickname: ''
        });
        nameInputs.current.focus();
    };

    return (
        <div>
            <input
                name='name'
                placeholder='이름'
                onChange={onChange}
                value={name}
                ref={nameInputs}
            />
            <input
                name='nickname'
                placeholder='별명'
                onChange={onChange}
                value={nickname}
            />
            <button onClick={onReset}>초기화</button>
            <div>
                <b>값 : </b>
                {name}({nickname})
            </div>
        </div>
    );
}
```

* `useRef()` 를 사용하여 `Ref` 객체 생성하여 선택하고자하는 `DOM` 요소에 `ref` 값으로 설정한 코

### 배열 렌더링하기 

#### 배열 데이터 처리 변천

{% tabs %}
{% tab title="고정적으로 코드 구성" %}
```javascript
import React from 'react';

export default function UserList() {
    const users = [
        {
            id: 1,
            username: 'godchiken',
            email: 'godchiken@naver.com'
        },
        {
            id: 2,
            username: 'tester',
            email: 'tester@example.com'
        },
        {
            id: 3,
            username: 'liz',
            email: 'liz@example.com'
        }
    ];
    return (
        <div>
            <div>
                <b>{users[0].username}</b> <span>({users[0].email})</span>
            </div>
            <div>
                <b>{users[1].username}</b> <span>({users[1].email})</span>
            </div>
            <div>
                <b>{users[2].username}</b> <span>({users[1].email})</span>
            </div>
        </div>
    );
}
```
{% endtab %}

{% tab title="자식 컴포넌트 생성으로 구성" %}
```javascript
import React from 'react';

function User({user}) {
    return (
        <div>
            {user.username}({user.email})
        </div>
    );
}

export default function UserList() {
    const users = [
        {
            id: 1,
            username: 'godchiken',
            email: 'godchiken@naver.com'
        },
        {
            id: 2,
            username: 'tester',
            email: 'tester@example.com'
        },
        {
            id: 3,
            username: 'liz',
            email: 'liz@example.com'
        }
    ];
    return (
        <div>
            <User user={users[0]}/>
            <User user={users[1]}/>
            <User user={users[2]}/>
        </div>
    );
}
```
{% endtab %}

{% tab title="동적 데이터에 대응하는 구성" %}
```javascript
import React from 'react';

function User({user}) {
    return (
        <div>
            {user.username}({user.email})
        </div>
    );
}

export default function UserList() {
    const users = [
        {
            id: 1,
            username: 'godchiken',
            email: 'godchiken@naver.com'
        },
        {
            id: 2,
            username: 'tester',
            email: 'tester@example.com'
        },
        {
            id: 3,
            username: 'liz',
            email: 'liz@example.com'
        }
    ];
    return (
        <div>
            {
                users.map(user =>(
                    <User user={user}/>
                ))
            }
        </div>
    );
}
```
{% endtab %}
{% endtabs %}

* 각 배열 요소를 사용하여 자식 컴포넌트를 구성하고 활용해보고, 앞으로 배열의 요소가 늘어날 경우를 대비한 동적인 구성에 대해서 배웠다.

#### key Props

![key props &#xC124;&#xC815;&#xC744; &#xC548;&#xD55C; &#xACBD;&#xC6B0;](../../.gitbook/assets/image%20%288%29.png)

* 리액트에서 배열을 랜더링시 `key`라는 `props`를 설정해야 한다. 

```javascript
return (
    <div>
        {
            users.map(user =>(
                <User user={user} key={user.id}/>
            ))
        }
    </div>
);
```

* 고유한 값을 다음과 같이 `key props`에 설정해본다. 왜 귀찮게 해야 하는가 생각이 들 수 있다.

![key props &#xAC00; &#xC5C6;&#xB294;&#xACBD;&#xC6B0;](../../.gitbook/assets/3rkaiy1.gif)

* 리렌더링 때, 해당 변화를 주고자하는 컴포넌트의 인덱스와 상관없이 모든 컴포넌트를 순차적으로 변경하는 과정을 취한다.

![key props &#xAC00; &#xC788;&#xB294; &#xACBD;&#xC6B0;](../../.gitbook/assets/yeus6bx.gif)

* 특정 인덱스의 컴포넌트에 변화를 주려면 고유 값을 알아야 원하는 곳만 변화를 줄 수 있다.  

### useRef 로 컴포넌트 안의 변수 만들기 

#### useRef의 또다른 용도

* 컴포넌트 내부에서 조회 및 수정이 가능한 변수를 관리하는 용도
* 값이 변경이 되어도 컴포넌트 리렌더링에 관여하지 않는다.
* 일반적인 컴포넌트는 상태를 바꾸는 함수를 호출 -&gt; 랜더링 이후에 업데이트 된 상태를 조회하는 반면 useRef는 관리하고 있는 변수를 설정 이후 바로 조회가 가능하다.

#### 위 용도를 통한 다양한 관리 사례

* 비동기 이벤트를 통해 생성된 고유값 \(ex: id값\)
* 외부 라이브러리를 통해 생성된 인스턴스
* scroll 위치

#### 직접 실습하여 보자 

```javascript
const nextId = useRef(4);
const onCreate = () => {
    nextId.current+=1;
}
```

### 배열에 항목 추가하기

{% tabs %}
{% tab title="CreateUser Component" %}
```javascript
import React from 'react';

export default function CreateUser({ username, email, onChange, onCreate }) {
    return (
        <div>
            <input
                name="username"
                placeholder="계정명"
                onChange={onChange}
                value={username}
            />
            <input
                name="email"
                placeholder="이메일"
                onChange={onChange}
                value={email}
            />
            <button onClick={onCreate}>등록</button>
        </div>
    );
}

```
{% endtab %}

{% tab title="App Component" %}
```javascript
import React,{useRef, useState} from 'react';
import UserList from "./UserList";
import CreateUser from "./CreateUser";

export default function App() {
    const [inputs,setInputs] = useState({
        username: '',
        email: ''
    });
    const {username, email} = inputs;
    const onChange = e => {
        const {value,name} = e.target;
        setInputs({
            ...inputs,
            [name] : value
        })
    };
    const [users,setUsers] = useState([
        {
            id: 1,
            username: 'godchiken',
            email: 'godchiken@naver.com'
        },
        {
            id: 2,
            username: 'tester',
            email: 'tester@example.com'
        },
        {
            id: 3,
            username: 'liz',
            email: 'liz@example.com'
        }
    ]);

    const nextId = useRef(4);
    const onCreate = () => {
        const user = {
            id : nextId.current,
            username,
            email
        };
        setUsers([...users,user]);
        setInputs({
            username : '',
            email: ''
        });
        nextId.current += 1;
    };
    return (
        <>
            <CreateUser
                username={username}
                email={email}
                onChange={onChange}
                onCreate={onCreate}
            />
            <UserList users={users} />
        </>
    );
}
```
{% endtab %}

{% tab title="UserList Component" %}
```javascript
import React from 'react';

function User({ user }) {
    return (
        <div>
            <b>{user.username}</b> <span>({user.email})</span>
        </div>
    );
}

export default function UserList({ users }) {
    return (
        <div>
            {users.map(user => (
                <User user={user} key={user.id} />
            ))}
        </div>
    );
}
```
{% endtab %}
{% endtabs %}

* 부모컴포넌트 `App`에서 `state` 관리를 하며, `input` 엘리먼트에 필요한 함수들을 `prop` 를 넘겨서 실습.
* `CreateUser` 에서 비구조화 할당을 통하여 `props`에서 필요한 변수, 함수를 전달받는다.
* `App` 에서 자식 컴포넌트 `CreateUser`에서 사용할 `props` 를 관리한다.

#### 배열에 대한 변화를 줄 시 주의점

* 불변성을 지키기 위해 배열의 `push`, `splice`, `sort` 등의 함수를 사용할 경우 새로운 배열에 복사하여 사용한다. 간편하게 사용하고 싶을 때는 다음의 두가지 방법을 따른다.
  * `spread` 연산자
  * `concat`\(\) : 기존 배열을 수정하지 않고 새로운 배열을 생성

### 배열에 항목 제거하기 

{% tabs %}
{% tab title="CreateUser Component" %}
```javascript
import React from 'react';

export default function CreateUser({ username, email, onChange, onCreate}) {
    return (
        <div>
            <input
                name="username"
                placeholder="계정명"
                onChange={onChange}
                value={username}
            />
            <input
                name="email"
                placeholder="이메일"
                onChange={onChange}
                value={email}
            />
            <button onClick={onCreate}>등록</button>
        </div>
    );
}
```
{% endtab %}

{% tab title="App Component" %}
```javascript
import React,{useRef, useState} from 'react';
import UserList from "./UserList";
import CreateUser from "./CreateUser";

export default function App() {
    const [inputs,setInputs] = useState({
        username: '',
        email: ''
    });
    const {username, email} = inputs;
    const onChange = e => {
        const {value,name} = e.target;
        setInputs({
            ...inputs,
            [name] : value
        })
    };
    const [users,setUsers] = useState([
        {
            id: 1,
            username: 'godchiken',
            email: 'godchiken@naver.com'
        },
        {
            id: 2,
            username: 'tester',
            email: 'tester@example.com'
        },
        {
            id: 3,
            username: 'liz',
            email: 'liz@example.com'
        }
    ]);

    const nextId = useRef(4);
    const onCreate = () => {
        const user = {
            id : nextId.current,
            username,
            email
        };
        setUsers([...users,user]);
        setInputs({ username : '', email: '' });
        nextId.current += 1;
    };
    const onRemove = id => {
        setUsers(users.filter(user => user.id !== id))
    };
    return (
        <>
            <CreateUser
                username={username}
                email={email}
                onChange={onChange}
                onCreate={onCreate}
            />
            <UserList users={users} onRemove={onRemove}/>
        </>
    );
}
```
{% endtab %}

{% tab title="UserList Component" %}
```javascript
import React from 'react';

function User({ user, onRemove }) {
    return (
        <div>
            <b>{user.username}</b> <span>({user.email})</span>
            <button onClick={() => onRemove(user.id)}>삭제하기</button>
        </div>
    );
}

export default function UserList({ users, onRemove }) {
    return (
        <div>
            {users.map(user => (
                <User user={user} key={user.id} onRemove={onRemove}/>
            ))}
        </div>
    );
}
```
{% endtab %}
{% endtabs %}

### 배열에 항목 수정하기

#### 간단한 색상 수정기능 

{% tabs %}
{% tab title="User에 active 속성 추가" %}
```javascript
const [users,setUsers] = useState([
    {
        id: 1,
        username: 'godchiken',
        email: 'godchiken@naver.com',
        active : true
    },
    {
        id: 2,
        username: 'tester',
        email: 'tester@example.com',
        active : false
    },
    {
        id: 3,
        username: 'liz',
        email: 'liz@example.com',
        active : false
    }
]);
```
{% endtab %}

{% tab title="onToggle 이벤트 구현" %}
```javascript
const onToggle = id => {
    setUsers(users.map(user =>
            user.id === id
            ? {...user, active: !user.active}
            : user
        )
    )
};
```
{% endtab %}

{% tab title="UserList 컴포넌트에 이벤트 전달" %}
```javascript
<UserList users={users} onRemove={onRemove} onToggle={onToggle}/>
```
{% endtab %}

{% tab title="App.js 전체 코드" %}
```javascript
import React,{useRef, useState} from 'react';
import UserList from "./UserList";
import CreateUser from "./CreateUser";

export default function App() {
    const [inputs,setInputs] = useState({
        username: '',
        email: ''
    });
    const {username, email} = inputs;
    const onChange = e => {
        const {value,name} = e.target;
        setInputs({
            ...inputs,
            [name] : value
        })
    };
    const [users,setUsers] = useState([
        {
            id: 1,
            username: 'godchiken',
            email: 'godchiken@naver.com',
            active : true
        },
        {
            id: 2,
            username: 'tester',
            email: 'tester@example.com',
            active : false
        },
        {
            id: 3,
            username: 'liz',
            email: 'liz@example.com',
            active : false
        }
    ]);

    const nextId = useRef(4);
    const onCreate = () => {
        const user = {
            id : nextId.current,
            username,
            email
        };
        setUsers([...users,user]);
        setInputs({ username : '', email: '' });
        nextId.current += 1;
    };
    const onRemove = id => {
        setUsers(users.filter(user => user.id !== id))
    };
    const onToggle = id => {
        setUsers(users.map(user =>
                user.id === id
                ? {...user, active: !user.active}
                : user
            )
        )
    };

    return (
        <>
            <CreateUser
                username={username}
                email={email}
                onChange={onChange}
                onCreate={onCreate}
            />
            <UserList users={users} onRemove={onRemove} onToggle={onToggle}/>
        </>
    );
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="User toggle 이벤트 구성" %}
```jsx
function User({ user, onRemove, onToggle}) {
    return (
        <div>
            <b
                style={{
                    cursor: 'pointer',
                    color: user.active ? 'green' : 'black'
                }}
                onClick={() => onToggle(user.id)}
            >
                {user.username}
            </b>
            &nbsp;
            <span>({user.email})</span>
            <button onClick={() => onRemove(user.id)}>삭제하기</button>
        </div>
    );
}
```
{% endtab %}

{% tab title="UserList " %}
```jsx
export default function UserList({ users, onRemove, onToggle }) {
    return (
        <div>
            {users.map(user => (
                <User user={user} key={user.id} onRemove={onRemove} onToggle={onToggle}/>
            ))}
        </div>
    );
}
```
{% endtab %}

{% tab title="UserList.js 전체 코드" %}
```javascript
    return (
        <div>
            <b
                style={{
                    cursor: 'pointer',
                    color: user.active ? 'green' : 'black'
                }}
                onClick={() => onToggle(user.id)}
            >
                {user.username}
            </b>
            &nbsp;
            <span>({user.email})</span>
            <button onClick={() => onRemove(user.id)}>삭제하기</button>
        </div>
    );
}

export default function UserList({ users, onRemove, onToggle }) {
    return (
        <div>
            {users.map(user => (
                <User user={user} key={user.id} onRemove={onRemove} onToggle={onToggle}/>
            ))}
        </div>
    );
}
```
{% endtab %}
{% endtabs %}

#### 구현 화면

![&#xAD6C;&#xD604; &#xD654;&#xBA74;](../../.gitbook/assets/captured-1.gif)

### `useEffect`를 사용하여 마운트/언마운트/업데이트시 할 작업 설정하기 

#### `useEffect`를 통한 컴포넌트 마운트, 언마운트 관리

```jsx
useEffect(() =>{
    console.log("컴포넌트 마운트!");
    return () => {
        console.log("컴포넌트가 화면에서 사라짐");
    }
},[]);
```

* `useEffect`를 사용할 때에는 첫번째 파라미터에 함수, 두번째 파라미터에 의존값이 들어있는 배열\(`deps`\)를 넣는다. 
* 첫번째 함수에서 함수를 반환이 가능하며 이 함수를 `cleanup`함수라고 부른다. `cleanup`함수는 `deps`를 생략한 경우 컴포넌트가 사라질 때 사용된다.

#### `mount`시 주요 작업 예시

* `props`로 받은 값을 컴포넌트의 로컬 `state`로 설정.
* 외부 api 요청.
* 라이브러리 사용.
* `setInterval()`,`setTimeout()` 을 통한 작업.

#### `unmount`시 주요 작업 예시

* `setInterval()`,`setTimeout()` 사용한 작업 clear. \(`clearInterval()`, `clearTimeout()`\)
* 라이브러리 인스턴스 제거

#### `deps`에 특정 값 넣기

```jsx
useEffect(() => {
  console.log('user 값이 설정됨');
  console.log(user);
  return () => {
    console.log('user 가 바뀌기 전..');
    console.log(user);
  };
}, [user]);
```

* `deps`에 특정 값을 넣게된다면 컴포넌트가 처음 마운트/언마운트 되거나 지정된 값이 바뀌거나 바뀌기 직전에도 호출이된다.
* `useEffect` 안에서 사용하는 `state`나 `props`가 존재한다면 `deps`에 기입해야하 하는것이 규칙이다.
* 만약 기입을 하지 않게될 경우 `useEffect`에 등록한 함수가 실행 될 때 최신 `props`/`state`를 가르키지 않게되며 다음과 같은 경고문을 보게된다.

{% hint style="warning" %}
React Hook useEffect has a missing dependency: 'user'. Either include it or remove the dependency array react-hooks/exhaustive-deps
{% endhint %}

#### deps 파라미터를 생략하는 경우

```jsx
useEffect(() => {
  console.log(user);
});
```

* 이럴 경우 리렌더링 될때마다 호출이 된다.

![&#xB9AC;&#xB79C;&#xB354;&#xB9C1;&#xB9C8;&#xB2E4; &#xD638;&#xCD9C;](../../.gitbook/assets/image%20%2814%29.png)

* 리액트 컴포넌트는 부모가 리렌더링 되면 자식컴포넌트 또한 리렌더링이 수행된다.
* 실제 `DOM`에 변화가 반영되는것은 바뀐 내용에 해당되는 컴포넌트만 되지만, `Virtual DOM`에서는 모든 `DOM`을 렌더링하므로 컴포넌트의 최적화하는 과정에서 이러한 불필요한 과정의 리소스를 절약하는 것이 가능하다.

### useMemo 를 사용하여 연산한 값 재사용하기 

{% tabs %}
{% tab title="활성 사용자 수" %}
```jsx
const count = useMemo( () => {
    console.log('활성 사용자 수를 세는중...');
    return users.filter(user => user.active).length;
},[users]);
```
{% endtab %}

{% tab title="활성 사용자 수 구성하기" %}
```jsx
return (
    <>
        <CreateUser
            username={username}
            email={email}
            onChange={onChange}
            onCreate={onCreate}
        />
            <UserList users={users} onRemove={onRemove} onToggle={onToggle}/>
            <div>활성사용자 수 : {count}</div>
    </>
);
```
{% endtab %}

{% tab title="App.js" %}
```jsx
import React,{useRef, useState, useMemo} from 'react';
import UserList from "./UserList";
import CreateUser from "./CreateUser";

export default function App() {
    const [inputs,setInputs] = useState({
        username: '',
        email: ''
    });
    const {username, email} = inputs;
    const onChange = e => {
        const {value,name} = e.target;
        setInputs({
            ...inputs,
            [name] : value
        })
    };
    const [users,setUsers] = useState([
        {
            id: 1,
            username: 'godchiken',
            email: 'godchiken@naver.com',
            active : true
        },
        {
            id: 2,
            username: 'tester',
            email: 'tester@example.com',
            active : false
        },
        {
            id: 3,
            username: 'liz',
            email: 'liz@example.com',
            active : false
        }
    ]);

    const nextId = useRef(4);
    const onCreate = () => {
        const user = {
            id : nextId.current,
            username,
            email
        };
        setUsers([...users,user]);
        setInputs({ username : '', email: '' });
        nextId.current += 1;
    };
    const onRemove = id => {
        setUsers(users.filter(user => user.id !== id))
    };
    const onToggle = id => {
        setUsers(users.map(user =>
                user.id === id
                ? {...user, active: !user.active}
                : user
            )
        )
    };
    const count = useMemo( () => {
        console.log('활성 사용자 수를 세는중...');
        return users.filter(user => user.active).length;
    },[users]);
    return (
        <>
            <CreateUser
                username={username}
                email={email}
                onChange={onChange}
                onCreate={onCreate}
            />
            <UserList users={users} onRemove={onRemove} onToggle={onToggle}/>
            <div>활성사용자 수 : {count}</div>
        </>
    );
}
```
{% endtab %}
{% endtabs %}

* 활성 사용자 수를 세는건, users 에 변화가 있을때만 세야되는건데, input 값이 바뀔 때에도 컴포넌트가 리렌더링 되므로 이렇게 불필요할때에도 호출하여서 자원이 낭비된다.
* 이러한 상황에는 `useMemo` 라는 Hook 함수를 사용하면 성능을 최적화 한다. 이전에 계산 한 값을 재사용하는 방법이다.
* `useMemo` 의 첫번째 파라미터에는 어떻게 연산할지 정의하는 함수를 넣어주면 되고 두번째 파라미터에는 `deps` 배열을 넣어주면 되는데, 이 배열 안에 넣은 내용이 바뀌면, 우리가 등록한 함수를 호출해서 값을 연산해주고, 만약에 내용이 바뀌지 않았다면 이전에 연산한 값을 재사용한다.

### useCallback 를 사용하여 함수 재사용하기 

```jsx
import React,{useRef, useState, useMemo, useCallback} from 'react';
import UserList from "./UserList";
import CreateUser from "./CreateUser";

export default function App() {
    const [inputs,setInputs] = useState({
        username: '',
        email: ''
    });
    const {username, email} = inputs;
    const onChange = useCallback(e => {
        const {value,name} = e.target;
        setInputs({
            ...inputs,
            [name] : value
        })
    },[inputs]);
    const [users,setUsers] = useState([
        {
            id: 1,
            username: 'godchiken',
            email: 'godchiken@naver.com',
            active : true
        },
        {
            id: 2,
            username: 'tester',
            email: 'tester@example.com',
            active : false
        },
        {
            id: 3,
            username: 'liz',
            email: 'liz@example.com',
            active : false
        }
    ]);

    const nextId = useRef(4);
    const onCreate = useCallback(() => {
        const user = {
            id : nextId.current,
            username,
            email
        };
        setUsers([...users,user]);
        setInputs({ username : '', email: '' });
        nextId.current += 1;
    },[inputs]);
    const onRemove = useCallback(id => {
        setUsers(users.filter(user => user.id !== id))
    },[users]);
    const onToggle = useCallback(id => {
        setUsers(users.map(user =>
                user.id === id
                    ? {...user, active: !user.active}
                    : user
            )
        )
    },[users]);
    const count = useMemo( () => {
        console.log('활성 사용자 수를 세는중...');
        return users.filter(user => user.active).length;
    },[users]);
    return (
        <>
            <CreateUser
                username={username}
                email={email}
                onChange={onChange}
                onCreate={onCreate}
            />
            <UserList users={users} onRemove={onRemove} onToggle={onToggle}/>
            <div>활성사용자 수 : {count}</div>
        </>
    );
}
```

* `useMemo`와 비슷한 형태의 `Hook`함수이나 확실히 다른점
  * `useMemo()` : 특정한 결과값을 재사용할 때 사용.
  * `useCallback()` :  특정 함수를 재사용할 때 사용.
* 함수 선언자체의 리소스는 큰 부하의 작업이 아니나, 재사용이 가능할 경우 활용하는 것이 중요하다.
  * 컴포넌트에서 `props`가 바뀌지 않았다면 `Virtual DOM`에 새로 랜더링하지 않고 컴포넌트의 결과물을 재사용하는 최적화 작업을 하기 위해서 필수적이다.
* 함수 안에서 사용하는 `state`,`props`가 있다면 꼭 `deps`에 포함시켜야 최신 값을 참조할 수 있다.
* `props` 를 통해 받아온 함수가 있다면, 이또한 `deps`에 포함해야한다.

#### 사실은 `useCallBack`은 `useMemo`기반으로 만들어졌다.

```jsx
const onToggle = useMemo(
  () => () => {
    /* 구현하고자 하는 코드 */
  },
  [users]
);
```

* 다만 이런식으로 적기에는 효율적이지 않아 더욱 편하게 사용하기 위해 등장했다.

#### React DevTool

![hightlight update](../../.gitbook/assets/captured-2.gif)

* v4 버전으로 업그레이드 되면서 _**hightlight update**_ 기능이 보이지 않았다.
* [https://blog.woolta.com/categories/1/posts/159](https://blog.woolta.com/categories/1/posts/159) 에서 그에 대한 대처법을 블로그했다.
* 하지만 4.2버전부터는 원활하게 잘된다.

### React.memo 를 사용한 컴포넌트 리렌더링 방지 

{% tabs %}
{% tab title="CreateUser" %}
```jsx
import React from 'react';

const CreateUser = ({ username, email, onChange, onCreate}) => {
    console.log("createUser");
    return (
        <div>
            <input
                name="username"
                placeholder="계정명"
                onChange={onChange}
                value={username}
            />
            <input
                name="email"
                placeholder="이메일"
                onChange={onChange}
                value={email}
            />
            <button onClick={onCreate}>등록</button>
        </div>
    );
};

export default React.memo(CreateUser);
```
{% endtab %}

{% tab title="UserList" %}
```jsx
import React from 'react';

const User = React.memo(function User({ user, onRemove, onToggle }) {
    console.log("User Rendering");
    return (
        <div>
            <b
                style={{
                    cursor: 'pointer',
                    color: user.active ? 'green' : 'black'
                }}
                onClick={() => onToggle(user.id)}
            >
                {user.username}
            </b>
            &nbsp;
            <span>({user.email})</span>
            <button onClick={() => onRemove(user.id)}>삭제</button>
        </div>
    );
});

function UserList({ users, onRemove, onToggle }) {
    console.log("UserList Rendering");
    return (
        <div>
            {users.map(user => (
                <User
                    user={user}
                    key={user.id}
                    onRemove={onRemove}
                    onToggle={onToggle}
                />
            ))}
        </div>
    );
}

export default React.memo(
    UserList,
    (prevProps, nextProps) => nextProps.users === prevProps.users
);
```
{% endtab %}

{% tab title="App" %}
```jsx
import React,{useRef, useState, useMemo, useCallback} from 'react';
import UserList from "./UserList";
import CreateUser from "./CreateUser";

export default function App() {
    const [inputs,setInputs] = useState({
        username: '',
        email: ''
    });
    const {username, email} = inputs;
    const onChange = useCallback(e => {
        const {value,name} = e.target;
        setInputs(inputs => ({
            ...inputs,
            [name] : value
        }))
    },[]);
    const [users,setUsers] = useState([
        {
            id: 1,
            username: 'godchiken',
            email: 'godchiken@naver.com',
            active : true
        },
        {
            id: 2,
            username: 'tester',
            email: 'tester@example.com',
            active : false
        },
        {
            id: 3,
            username: 'liz',
            email: 'liz@example.com',
            active : false
        }
    ]);

    const nextId = useRef(4);
    const onCreate = useCallback(() => {
        const user = {
            id : nextId.current,
            username,
            email
        };
        setUsers(users => [...users,user]);
        setInputs({ username : '', email: '' });
        nextId.current += 1;
    },[username,email]);
    const onRemove = useCallback(id => {
        setUsers(users => users.filter(user => user.id !== id))
    },[]);
    const onToggle = useCallback(id => {
        setUsers(users => users.map(
            user => user.id === id
            ? {...user, active: !user.active}
            : user
        ));
    },[]);
    const count = useMemo( () => {
        const activeUser = users.filter(user => user.active).length;
        console.log('활성 사용자 수를 세는중...');
        return activeUser;
    },[users]);
    return (
        <>
            <CreateUser
                username={username}
                email={email}
                onChange={onChange}
                onCreate={onCreate}
            />
            <UserList users={users} onRemove={onRemove} onToggle={onToggle}/>
            <div>활성사용자 수 : {count}</div>
        </>
    );
}
```
{% endtab %}
{% endtabs %}

* `React.memo` 의 활용안은 기존 컴포넌트를 담을 수 있게 변수로 할당하고_\(할필요는 없지만 가독성을 위해서\)_ 해당 변수를 `React.memo`에 인자값으로 설정해준다.
* 다른 컴포넌트도 동일하게 적용을 해나갈 수도 있지만 `User`의 경우에 한개만 건드려도 모든 `User`, `CreateUser`까지 리렌더링 되는 현상이 있다.
* 몇 가지 점검을 해야 올바르게 작동한다.
  * `App`에서 `users`배열을 변경시  의존하는 함수들의 `deps`에 대해서 `useState`로 관리하는 `users`의 정보를 함수형으로 업데이트를 하는 구조로 변경이 필요하다.
  * 이러한 이유는 `users`가 변경시 함수가 재 선언되며, 이를 활용하는 컴포넌트들은 리렌더링 과정을 수행하기 때문이다.
  * 방법은 `deps`에 위치한 `users` 를 지우고, `setUsers`에 등록하는 콜백함수의 파라미터에 최신 `users`를 참조하게 변경하면 `deps`에 `users`가 존재하지 않더라도 된다.
* `onChange`의 경우엔 함수형 업데이트를 해도 영향이 가지 않는다고 한다.

#### 완벽하지 않은 React DevTool V4.2

* _**hightlight update**_ 기능이 아직 완벽하게 픽스 된것 같지는 않다. \(2019.11.08 기준\)
* 개발툴에 의존해서 확인하는 것 보다 각 컴포넌트에서 직접 확인해서 체크하는 것도 하나의 방법이다.

#### useMemo의 구성

```javascript
function useMemo(nextCreate, deps) {
  currentlyRenderingComponent = resolveCurrentlyRenderingComponent();
  workInProgressHook = createWorkInProgressHook();
  var nextDeps = deps === undefined ? null : deps;

  if (workInProgressHook !== null) {
    var prevState = workInProgressHook.memoizedState;

    if (prevState !== null) {
      if (nextDeps !== null) {
        var prevDeps = prevState[1];

        if (areHookInputsEqual(nextDeps, prevDeps)) {
          return prevState[0];
        }
      }
    }
  }

  {
    isInHookUserCodeInDev = true;
  }

  var nextValue = nextCreate();

  {
    isInHookUserCodeInDev = false;
  }

  workInProgressHook.memoizedState = [nextValue, nextDeps];
  return nextValue;
}
```

* 사실 이해하기엔 아직 아는게 없지만, 이런 식으로 구성을 한번 살펴보는거도 공부가 될 것이라고 생각한다.

### `useReducer` 를 사용하여 상태 업데이트 로직 분리하기 

#### `useReducer`는 무엇인가?

* 이전까지 상태를 업데이트시 `useState`를 활용했지만 또다른 방법이다.
* 상태관리 + 상태 업데이트 로직 분리가 가능하다.

#### `useReducer`를 활용한 `Counter` 컴포넌트 구현

{% tabs %}
{% tab title="useState & Counter Component" %}
```jsx
import React, {useState} from 'react';

export default function Counter() {
    const [number, setNumber] = useState(0);

    const onIncrease = () => {
        setNumber(prevNumber => prevNumber + 1);
    };
    const onDecrease = () => {
        setNumber(prevNumber => prevNumber - 1);
    };

    return (
        <div>
            <h1>{number}</h1>
            <button onClick={onIncrease}>+</button>
            <button onClick={onDecrease}>-</button>
        </div>
    );
}
```
{% endtab %}

{% tab title="useReducer & Counter Component" %}
```jsx
import React, {useReducer} from 'react';

function reducer(state, action) {
    switch (action.type) {
        case 'INCREMENT' :
            return state + 1;
        case 'DECREMENT' :
            return state - 1;
        default :
            return state;
    }
}

export default function Counter() {
    const [number, dispatch] = useReducer(reducer,0);

    const onIncrease = () => { dispatch({type:'INCREMENT'}); };
    const onDecrease = () => { dispatch({type:'DECREMENT'}); };

    return (
        <div>
            <h1>{number}</h1>
            <button onClick={onIncrease}>+</button>
            <button onClick={onDecrease}>-</button>
        </div>
    );
}
```
{% endtab %}
{% endtabs %}

* "함수만 분리했는데 무엇이 따로 관리하는 것인가" 라는 의문이 들수도 있다.
* 중요한 포인트는 ****`useReducer`_**의**_ `reducer`_**를 다른 파일에 작성해도 무방**_하다.
* 상태관련 업데이트를 따로 분리가 가능하다는 점이다.

#### `useReducer`를 활용한 `App` 컴포넌트 재구성

{% tabs %}
{% tab title="useState & App" %}
```jsx
import React,{useRef, useState, useMemo, useCallback} from 'react';
import UserList from "./UserList";
import CreateUser from "./CreateUser";

export default function App() {
    const [inputs,setInputs] = useState({ username: '', email: '' });
    const {username, email} = inputs;
    const onChange = useCallback(e => {
        const {value,name} = e.target;
        setInputs(inputs => ({
            ...inputs,
            [name] : value
        }))
    },[]);
    const [users,setUsers] = useState([
        { id: 1, username: 'godchiken', email: 'godchiken@naver.com', active : true },
        { id: 2, username: 'tester', email: 'tester@example.com', active : false },
        { id: 3, username: 'liz', email: 'liz@example.com', active : false }
    ]);

    const nextId = useRef(4);
    const onCreate = useCallback(() => {
        const user = {
            id : nextId.current,
            username,
            email
        };
        setUsers(users => [...users,user]);
        setInputs({ username : '', email: '' });
        nextId.current += 1;
    },[username,email]);
    const onRemove = useCallback(id => {
        setUsers(users => users.filter(user => user.id !== id))
    },[]);
    const onToggle = useCallback(id => {
        setUsers(users => users.map(
            user => user.id === id
                ? {...user, active: !user.active}
                : user
        ));
    },[]);
    const count = useMemo( () => {
        const activeUser = users.filter(user => user.active).length;
        console.log('활성 사용자 수를 세는중...');
        return activeUser;
    },[users]);
    return (
        <>
            <CreateUser
                username={username}
                email={email}
                onChange={onChange}
                onCreate={onCreate}
            />
            <UserList users={users} onRemove={onRemove} onToggle={onToggle}/>
            <div>활성사용자 수 : {count}</div>
        </>
    );
}
```
{% endtab %}

{% tab title="useReducer & App" %}
```jsx
import React, {useRef, useReducer, useMemo, useCallback} from 'react';
import UserList from "./UserList";
import CreateUser from "./CreateUser";

const initialState = {
    inputs: {username: '', email: ''},
    users: [
        {id: 1, username: 'godchiken', email: 'godchiken@naver.com', active: true},
        {id: 2, username: 'tester', email: 'tester@example.com', active: false},
        {id: 3, username: 'liz', email: 'liz@example.com', active: false}
    ]
};

function reducer(state, action) {
    switch (action.type) {
        case 'CHANGE_INPUT' :
            return {
                ...state,
                inputs: {
                    ...state.inputs,
                    [action.name]: action.value
                }
            };
        case 'CREATE_USER' :
            return {
                inputs: initialState.inputs,
                users: state.users.concat(action.user)
            };
        case 'TOGGLE_USER':
            return {
                ...state,
                users: state.users.map(user =>
                    user.id === action.id ? {...user, active: !user.active} : user
                )
            };
        case 'REMOVE_USER':
            return {
                ...state,
                users: state.users.filter(user => user.id !== action.id)
            };
        default :
            return state;
    }
}

export default function App() {
    const [state, dispatch] = useReducer(reducer, initialState);
    const {users} = state;
    const {username, email} = state.inputs;
    const nextId = useRef(4);

    const onChange = useCallback(e => {
        const {value, name} = e.target;
        dispatch({
            type: 'CHANGE_INPUT',
            name,
            value
        });
    }, []);

    const onCreate = useCallback(() => {
        dispatch({
            type: 'CREATE_USER',
            user: {
                id: nextId.current,
                username,
                email
            }
        });
        nextId.current++;
    }, [username, email]);

    const onToggle = useCallback(id => {
        dispatch({
            type: 'TOGGLE_USER',
            id
        })
    }, []);

    const onRemove = useCallback(id => {
        dispatch({
            type: 'REMOVE_USER',
            id
        })
    }, []);
    const count = useMemo(() => users.filter(user => user.active).length, [users]);

    return (
        <>
            <CreateUser
                username={username}
                email={email}
                onChange={onChange}
                onCreate={onCreate}
            />
            <UserList users={users} onToggle={onToggle} onRemove={onRemove}/>
            <div>활성사용자 수 : {count}</div>
        </>
    );
}
```
{% endtab %}
{% endtabs %}

* 조금 더 많은 기능이 존재하는 `App`의 `useState` -&gt;`useReducer`변환 코드.
* 개인적으론 상태관리를 신경쓰는 **관심사**가 분리되어 더욱 가독성이 좋아진 것 같다.
* `switch~case` 분기를 위해 작성한 값은 `typeScript`를 도입해서 `enum`으로 대체하고 싶다.

#### 그래서 `useState`, `useReducer`는 언제 구분해서 활용하는가?

* 정답은 없다고 한다.
* 상태관리를 함에 있어서 두가지 관점으로 보자면 다음과 같다.
  1. 내가 관리하는 상태 값이 한개 인가?
  2. 내가 관리하는 상태 값이 1~n 개 인가?
  3. 혹은 현재는 단일 상태 값만 관리하지만 유동적일 가능성이 있는가?
* 1번 이라면 `useState`를 권장한다.
* 2,3번 이라면 `useReducer`를 권장한다.

#### 그래서 본인은 `useState`  or `useReducer`?

* 상태값이 몇개 인지에서 생각하는 관점이 아니라, 기능 상으로 동일하다면 해당 로직에 더욱 집중을 하도록 애초부터 분리를 하여 집중할 수 있도록 분리하는 `useReducer`를 택할 것이다.
* 그런데 상태 관리에 관한 여러 기술들 `Context API`, `Redux`, `MobX`등이 있다곤 한다.

### 커스텀 Hooks 만들기 

#### 공통되는 로직을 관리해보자.

{% tabs %}
{% tab title="useState & useInputs\(custom\)" %}
```jsx
import { useState, useCallback } from 'react';

function useInputs(initialForm) {
  const [form, setForm] = useState(initialForm);
  // change
  const onChange = useCallback(e => {
    const { name, value } = e.target;
    setForm(form => ({ ...form, [name]: value }));
  }, []);
  const reset = useCallback(() => setForm(initialForm), [initialForm]);
  return [form, onChange, reset];
}

export default useInputs;
```
{% endtab %}

{% tab title="App" %}
```jsx
import React, {useRef, useReducer, useMemo, useCallback} from 'react';
import UserList from "./UserList";
import CreateUser from "./CreateUser";
import useInputs from "../hooks/useInputs";

const initialState = {
    inputs: {username: '', email: ''},
    users: [
        {id: 1, username: 'godchiken', email: 'godchiken@naver.com', active: true},
        {id: 2, username: 'tester', email: 'tester@example.com', active: false},
        {id: 3, username: 'liz', email: 'liz@example.com', active: false}
    ]
};

function reducer(state, action) {
    switch (action.type) {
        case 'CREATE_USER' :
            return {
                inputs: initialState.inputs,
                users: state.users.concat(action.user)
            };
        case 'TOGGLE_USER':
            return {
                ...state,
                users: state.users.map(user =>
                    user.id === action.id ? {...user, active: !user.active} : user
                )
            };
        case 'REMOVE_USER':
            return {
                ...state,
                users: state.users.filter(user => user.id !== action.id)
            };
        default :
            return state;
    }
}

export default function App() {
    const [{username, email}, onChange, reset] = useInputs(initialState.inputs);
    const [state, dispatch] = useReducer(reducer, initialState);
    const nextId = useRef(4);
    const {users} = state;

    const onToggle = useCallback(id => {
        dispatch({
            type: 'TOGGLE_USER',
            id
        });
    }, []);

    const onCreate = useCallback(() => {
        dispatch({
            type: 'CREATE_USER',
            user: {
                id: nextId.current,
                username,
                email
            }
        });
        reset();
        nextId.current++;
    }, [username, email, reset]);

    const onRemove = useCallback(id => {
        dispatch({
            type: 'REMOVE_USER',
            id
        })
    }, []);
    const count = useMemo(() => users.filter(user => user.active).length, [users]);

    return (
        <>
            <CreateUser
                username={username}
                email={email}
                onChange={onChange}
                onCreate={onCreate}
            />
            <UserList users={users} onToggle={onToggle} onRemove={onRemove}/>
            <div>활성사용자 수 : {count}</div>
        </>
    );
}
```
{% endtab %}
{% endtabs %}

* 컴포넌트를 생성하다보면, 공통적인 로직이 발생하기 마련이다.
* `custom Hooks` 를 생성하여 재사용하는 방법에 대해서 실습했다. 

#### custom hooks : `useState` -&gt; `useReducer` 전환 연습하기

{% tabs %}
{% tab title="초기 풀이" %}
```jsx
import {useReducer, useCallback} from 'react';

function reducer(state, action) {
    const { name, value } = action;
    switch (action.type) {
        case 'FORM_CHANGE':
            return { ...state, [name] : value };
        case 'FORM_RESET':
            const resetObject = {...state};
            resetObject.username = '';
            resetObject.email = '';
            return resetObject;
        default : throw new Error("DO NOT ANYTHING");
    }
}

export default function useInputs(initialForm) {
    const [state, dispatch] = useReducer(reducer, initialForm);

    const onChange = useCallback(e => {
        const {name, value} = e.target;
        dispatch({ type : 'FORM_CHANGE', name, value});
    },[]);

    const reset = useCallback(() => {
        dispatch({ type : 'FORM_RESET'});
    },[]);

    return [state, onChange, reset];
}
```
{% endtab %}

{% tab title="1차 개선\(for...in 활용\)" %}
```jsx
import {useReducer, useCallback} from 'react';

function reducer(state, action) {
    const { name, value } = action;
    switch (action.type) {
        case 'FORM_CHANGE':
            return { ...state, [name] : value };
        case 'FORM_RESET':
            const resetObject = {...state};
            for(const target in resetObject){
                resetObject[target] = '';
            }
            return resetObject;
        default : throw new Error("DO NOT ANYTHING");
    }
}

export default function useInputs(initialForm) {
    const [state, dispatch] = useReducer(reducer, initialForm);

    const onChange = useCallback(e => {
        const {name, value} = e.target;
        dispatch({ type : 'FORM_CHANGE', name, value});
    },[]);

    const reset = useCallback(() => {
        dispatch({ type : 'FORM_RESET'});
    },[]);

    return [state, onChange, reset];
}
```
{% endtab %}

{% tab title="2차 개선\(reduce\(\) 활용\)" %}
```jsx
import {useReducer, useCallback} from 'react';

function reducer(state, action) {
    const { name, value } = action;
    switch (action.type) {
        case 'FORM_CHANGE':
            return { ...state, [name] : value };
        case 'FORM_RESET':
            return Object.keys(state).reduce((accumulator, currentValue) => {
                    accumulator[currentValue] = '';
                    return accumulator;
            }, {});
        default : throw new Error("DO NOT ANYTHING");
    }
}

export default function useInputs(initialForm) {
    const [state, dispatch] = useReducer(reducer, initialForm);

    const onChange = useCallback(e => {
        const {name, value} = e.target;
        dispatch({ type : 'FORM_CHANGE', name, value});
    },[]);

    const reset = useCallback(() => {
        dispatch({ type : 'FORM_RESET'});
    },[]);

    return [state, onChange, reset];
}

```
{% endtab %}
{% endtabs %}

#### 본인은 어떤 식으로 전환했나요?

* 기존의 `App Component`에서 작성한 `reducer` 함수를 토대로 동일하게 작성을 시도했다.
* 하지만 `case 'FORM_RESET'` 에서 `input` 의 상태값을 초기화 함에 있어서 비슷한 구조의 코드가 반복되면 줄여나가야 한다고 파악하여 `for..in` 구문을 통해서 문제를 풀어나갔다.

#### 벨로퍼트는 어떻게 전환했는가?

* `Object.keys()`를 통해 `state`의 `key` 값을 추출
* `reduce()`를 통해서 최초 초기값으로 객체 리터럴`{}` 에 초기화된 상태값을 연관배열을 통해 초기화하고 집계했다.

#### 그래서 본인과 벨로퍼트의 전환 방식에 대해서 느낀 점은?

* 취향 차이라고 볼수도 있으나 본인도 메서드 체이닝, 함수형 프로그래밍으로 풀어나가는 것을 더 선호하기에 `reduce()`에 대해서 더 공부하기로 마음먹었다.

#### Array.prototype.reduce를 잠깐 짚고 넘어가자

*  **`reduce()`** 메서드는 배열의 각 요소에 대해 주어진 **리듀서**\(reducer\) 함수를 실행하고, 하나의 결과값을 반환한다.
* 크게 `callback`, `initialValue`로 나뉜다.
* `callback` 
  * 컬렉션의 각 요소에 대해 실행할 함수. 4개의 인수를 가지고 있다.
    * 필수요소
      * `accumulator` : 콜백의 반환값을 누적한다.. 콜백의 이전 반환값 또는, 콜백의 첫 번째 호출과정에서 `initialValue`를 제공한 경우에는 `initialValue`의 초기값 이다.
      * `currentValue` : 처리할 현재 요소.
    * 옵션
      * `currentIndex`  : Optional처리할 현재 요소의 인덱스. `initialValue`를 제공한 경우 0, 아니면 1부터 시작합니다.
      * `array` : Optional`reduce()`를 호출한 배열.
* `initialValue` 
  * Optional`callback`의 최초 호출에서 첫 번째 인수에 제공하는 값.
  * 초기값을 제공하지 않으면 배열의 첫 번째 요소를 사용한다. 
  * 빈 배열에서 초기값 없이 `reduce()`를 호출하면 오류가 발생한다.

### Context API 를 사용한 전역 값 관리 

#### 부모에게서 자식으로 "값"을 전달할 경우 고찰..

* 현재까지 실습을 한 컴포넌트의 구성을 살펴본다면 해당 컴포넌트에서 사용하지 않는데도 불구하고 다음 세대의  자식 컴포넌트에 전달하기위한 중간다리 역할을 하기위해 "값"을 전달하는 형태로 구현했다.
* `one-depth` 의 구조의 컴포넌트면 신경을 안 쓸수도 있으나 우리가 앞으로 실무에서 구현해야 하는 컴포넌트는 그렇게 만만하지 않다.
* 이전부터 누누히 언급했었던 상태관리에 대한 기술 중 하나읜 `Context API`를 해당 챕터에서 실습했다.

#### Context API

* `state` 뿐만 아니라 외부라이브러리 인스턴스, `DOM`이 될 수있다.
* 전역적으로 프로젝트의 상태를 관리하는 용도로 사용된다.

#### 기본 코드

```jsx
const UserDispatch = React.createContext(null);
```

* Context의 기본값을 설정할 수 있다.
* 내부 컴포넌트로 `Provider` 라는 컴포넌트가 구성되어있고 `Context`의 값을 지정할 수 있다.
  *  `attribute`로 `value`를 설정이 가능하다. 본 챕터에서는 `fragment` 대신 교체가 되고 `useReducer`에서 추출한 `dispatch`를 `value`로 설정했다.

#### 실습코드

{% tabs %}
{% tab title="App" %}
```jsx
import React, {useRef, useReducer, useMemo, useCallback} from 'react';
import UserList from "./UserList";
import CreateUser from "./CreateUser";
import useInputs from "../hooks/useInputs";

const initialState = {
    inputs: {username: '', email: ''},
    users: [
        {id: 1, username: 'godchiken', email: 'godchiken@naver.com', active: true},
        {id: 2, username: 'tester', email: 'tester@example.com', active: false},
        {id: 3, username: 'liz', email: 'liz@example.com', active: false}
    ]
};

function reducer(state, action) {
    switch (action.type) {
        case 'CREATE_USER' :
            return {
                inputs: initialState.inputs,
                users: state.users.concat(action.user)
            };
        case 'TOGGLE_USER':
            return {
                ...state,
                users: state.users.map(user =>
                    user.id === action.id ? {...user, active: !user.active} : user
                )
            };
        case 'REMOVE_USER':
            return {
                ...state,
                users: state.users.filter(user => user.id !== action.id)
            };
        default :
            return state;
    }
}

export const UserDispatch = React.createContext(null);

export default function App() {
    const [{username, email}, onChange, reset] = useInputs(initialState.inputs);
    const [state, dispatch] = useReducer(reducer, initialState);
    const nextId = useRef(4);
    const {users} = state;

    const onCreate = useCallback(() => {
        dispatch({
            type: 'CREATE_USER',
            user: {
                id: nextId.current,
                username,
                email
            }
        });
        reset();
        nextId.current++;
    }, [username, email, reset]);

    const count = useMemo(() => users.filter(user => user.active).length, [users]);

    return (
        <UserDispatch.Provider value={dispatch}>
            <CreateUser
                username={username}
                email={email}
                onChange={onChange}
                onCreate={onCreate}
            />
            <UserList users={users}/>
            <div>활성사용자 수 : {count}</div>
        </UserDispatch.Provider>
    );
}
```
{% endtab %}

{% tab title="UserList" %}
```jsx
import React, {useContext} from 'react';
import {UserDispatch} from "./App";

const User = React.memo(function User({ user }) {
    console.log("User Rendering");

    const dispatch = useContext(UserDispatch);

    return (
        <div>
            <b  style={{
                    cursor: 'pointer',
                    color: user.active ? 'green' : 'black'
                }}
                onClick={() => {
                    dispatch({type: 'TOGGLE_USER' , id: user.id});
                }}
            >
                {user.username}
            </b>
            &nbsp;
            <span>({user.email})</span>
            <button onClick={() => {
                        dispatch({type:'REMOVE_USER', id: user.id });
                    }}
            >
                삭제
            </button>
        </div>
    );
});

function UserList({ users }) {
    console.log("UserList Rendering");
    return (
        <div>
            {users.map(user => (
                <User user={user}
                      key={user.id}
                />
            ))}
        </div>
    );
}

export default React.memo(
    UserList,
    (prevProps, nextProps) => nextProps.users === prevProps.users
);
```
{% endtab %}
{% endtabs %}

* `App`
  * onToggle, onRemove의 삭제.
  * Context API 사용.
  * fragment -&gt; Context 로 교체.
* `UserList`
  * App 에서 삭제된 onToggle, onRemove 구현.
  * App 에서 선언한 Context 활
  * 기존 User에 전달하기 위한 onToggle, onRemove props 제거.

#### 벨로퍼트의 숙제 완료 코드\(숙제 그만 좀..\)

* 요구사항     

{% hint style="success" %}
User 컴포넌트에게 따로 `onToggle` / `onRemove` 를 `props`로 전달하지 않고 바로 `dispatch` 를 사용했던 것 처럼, `CreateUser` 컴포넌트에서도 `dispatch` 를 직접 하도록 구현을 해보세요.

* `CreateUser` 에게는 아무 `props` 도 전달하지 마세요.
* `CreateUser` 컴포넌트 내부에서 `useInputs` 를 사용하세요.
* `useRef` 를 사용한 `nextId` 값을 `CreateUser` 에서 관리하세요.
{% endhint %}

* 실습분

{% tabs %}
{% tab title="App" %}
```jsx
import React, {useReducer, useMemo} from 'react';
import UserList from "./UserList";
import CreateUser from "./CreateUser";

const initialState = {
    users: [
        {id: 1, username: 'godchiken', email: 'godchiken@naver.com', active: true},
        {id: 2, username: 'tester', email: 'tester@example.com', active: false},
        {id: 3, username: 'liz', email: 'liz@example.com', active: false}
    ]
};

function reducer(state, action) {
    switch (action.type) {
        case 'CREATE_USER' :
            return {
                inputs: initialState.inputs,
                users: state.users.concat(action.user)
            };
        case 'TOGGLE_USER':
            return {
                ...state,
                users: state.users.map(user =>
                    user.id === action.id ? {...user, active: !user.active} : user
                )
            };
        case 'REMOVE_USER':
            return {
                ...state,
                users: state.users.filter(user => user.id !== action.id)
            };
        default :
            return state;
    }
}

export const UserDispatch = React.createContext(null);

export default function App() {
    const [state, dispatch] = useReducer(reducer, initialState);
    const {users} = state;

    const count = useMemo(() => users.filter(user => user.active).length, [users]);

    return (
        <UserDispatch.Provider value={dispatch}>
            <CreateUser/>
            <UserList users={users}/>
            <div>활성사용자 수 : {count}</div>
        </UserDispatch.Provider>
    );
}
```
{% endtab %}

{% tab title="CreateUser" %}
```jsx
import React, {useContext, useRef} from 'react';
import {UserDispatch} from "./App";
import useInputs from "../hooks/useInputs";

const CreateUser = () => {
    const [{username, email}, change, reset] = useInputs(initialState.inputs);
    const dispatch = useContext(UserDispatch);
    const nextId = useRef(4);
    const onCreate = () => {
        dispatch(createDispatcherProperty(nextId,username,email));
        reset();
        nextId.current++;
    };

    return (
        <div>
            <input name="username"  placeholder="계정명" onChange={change} value={username} />
            <input name="email"     placeholder="이메일" onChange={change} value={email} />
            <button onClick={onCreate}>등록</button>
        </div>
    );
};

export default React.memo(CreateUser);

const initialState = { inputs: {username: '', email: ''} };
const createDispatcherProperty = (nextId, username, email) => {
    return {
        type: 'CREATE_USER',
        user: { id: nextId.current, username, email }
    };
};
```
{% endtab %}

{% tab title="UserList" %}
```jsx
import React, {useContext} from 'react';
import {UserDispatch} from "./App";

const User = React.memo(function User({ user }) {
    const dispatch = useContext(UserDispatch);
    return (
        <div>
            <b  style={{
                    cursor: 'pointer',
                    color: user.active ? 'green' : 'black'
                }}
                onClick={() => {
                    dispatch({type: 'TOGGLE_USER' , id: user.id});
                }}
            >
                {user.username}
            </b>
            &nbsp;
            <span>({user.email})</span>
            <button onClick={() => {
                        dispatch({type:'REMOVE_USER', id: user.id });
                    }}
            >
                삭제
            </button>
        </div>
    );
});

function UserList({ users }) {
    return (
        <div>
            {users.map(user => (
                <User user={user}
                      key={user.id}
                />
            ))}
        </div>
    );
}

export default React.memo(
    UserList,
    (prevProps, nextProps) => nextProps.users === prevProps.users
);
```
{% endtab %}
{% endtabs %}

* Context API 를 통해서 각 컴포넌트의 "값"의 변화는 필요한 컴포넌트에서 관리하게 되는 이점을 얻게 되었다. 

### `Immer`를 사용한 더 쉬운 불변성 관리 

#### 불변성의 중요성

* 리액트로 함수형 스타일로 구현해 나가면서 데이터의 불변성을 지켜주는 것은 더욱 중요해졌다.
* 불변성을 지키기 해서는 기존 데이터에서 변형이 필요할 때 수정이 아닌 새로 생성해 나가면 된다.

#### 그러나 불변성을 쉽게 지켜나가게 도와주는 `Immer`

{% tabs %}
{% tab title="예시 데이터" %}
```jsx
const state = {
  posts: [
    {
      id: 1,
      title: '제목입니다.',
      body: '내용입니다.',
      comments: [
        {
          id: 1,
          text: '와 정말 잘 읽었습니다.'
        }
      ]
    },
    {
      id: 2,
      title: '제목입니다.',
      body: '내용입니다.',
      comments: [
        {
          id: 2,
          text: '또 다른 댓글 어쩌고 저쩌고'
        }
      ]
    }
  ],
  selectedId: 1
};
```
{% endtab %}

{% tab title="수정하기 위해서는?" %}
```jsx
const nextState = {
  ...state,
  posts: state.posts.map(post =>
    post.id === 1
      ? {
          ...post,
          comments: post.comments.concat({
            id: 3,
            text: '새로운 댓글'
          })
        }
      : post
  )
};
```
{% endtab %}

{% tab title="Immer를 사용해보자" %}
```jsx
const nextState = produce(state, draft => {
  const post = draft.posts.find(post => post.id === 1);
  post.comments.push({
    id: 3,
    text: '와 정말 쉽다!'
  });
});
```
{% endtab %}
{% endtabs %}

* 비교적 직관적으로 코드가 보인다. 

#### `Immer` 설치

```lua
yarn add immer

npm add immer
```

* 둘 중 하나를 고르면 된다.

#### `produce()`

```jsx
produce(변경하고자하는 상태, 업데이트 하고자하는 함수)
```

* 심플하게 해하고 넘어간다.

#### 기존 예제의 `Reducer`에서 활용해보기

```jsx
import React, {useReducer, useMemo} from 'react';
import UserList from "./UserList";
import CreateUser from "./CreateUser";
import produce from 'immer';

const initialState = {
    users: [
        {id: 1, username: 'godchiken', email: 'godchiken@naver.com', active: true},
        {id: 2, username: 'tester', email: 'tester@example.com', active: false},
        {id: 3, username: 'liz', email: 'liz@example.com', active: false}
    ]
};

function reducer(state, action) {
    switch (action.type) {
        case 'CREATE_USER' :
            return produce(state, draft => {
               draft.users.push(action.user);
            });
        case 'TOGGLE_USER':
            return produce(state, draft => {
                const user = draft.users.find(user => user.id === action.id);
                user.active = !user.active;
            });
        case 'REMOVE_USER':
            return produce(state, draft => {
                const removeIndex = draft.users.findIndex(user => user.id === action.id);
                draft.users.splice(removeIndex,1);
            });

        default :
            return state;
    }
}

export const UserDispatch = React.createContext(null);

export default function App() {
    const [state, dispatch] = useReducer(reducer, initialState);
    const {users} = state;

    const count = useMemo(() => users.filter(user => user.active).length, [users]);

    return (
        <UserDispatch.Provider value={dispatch}>
            <CreateUser/>
            <UserList users={users}/>
            <div>활성사용자 수 : {count}</div>
        </UserDispatch.Provider>
    );
}
```

#### Immer와 함수형 업데이트

{% tabs %}
{% tab title="useState의 함수형 업데이트" %}
```jsx
const [todo, setTodo] = useState({
  text: 'Hello',
  done: false
});

const onClick = useCallback(() => {
  setTodo(todo => ({
    ...todo,
    done: !todo.done
  }));
}, []);
```
{% endtab %}

{% tab title="파라미터 1개의  produce 활용" %}
```jsx
const todo = {
  text: 'Hello',
  done: false
};

const updater = produce(draft => {
  draft.done = !draft.done;
});

const nextTodo = updater(todo);

console.log(nextTodo);
// { text: 'Hello', done: true }
```
{% endtab %}

{% tab title="응용을 해본다면" %}
```jsx
const [todo, setTodo] = useState({
  text: 'Hello',
  done: false
});

const onClick = useCallback(() => {
  setTodo(
    produce(draft => {
      draft.done = !draft.done;
    })
  );
}, []);
```
{% endtab %}
{% endtabs %}

* `useState`를 활용한 곳에서 우리 직접 업데이트하는 코드를 작성함으로 `deps`에 `todo`를 추가하지 않아도 었다. `produce`에서 한개의 인자값을 활용하면 반환 값은 새로운 상태가 아닌 상태를 업데이트 해주는 함수가 된다.

#### 편하긴 하나 주의해야할 점

* 성능
  * 인간이 시각적으로 인지하는 기준은 13ms
    *  `Immer`가 5만개의 원소중 5천개를 업데이트하는 속도는 약 31ms
    * `Native Reducer`가 업데이트하는 속도는 약 6ms
  * 성능적으로 Navtive Reducer 보다 좋지 않다. 
  * 그러므로 데이터의 양이 적거나, 구조가 복잡할 때 복잡도를  방지하는 차원의 용도로 권장한다.
  *  Immer 는 JavaScript 엔진의 [Proxy](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Proxy) 라는 기능을 사용
    * 구형 브라우저 및 react-native 같은 환경에서는 미지원
      * 위 환경에서 ES5 fallback 을 사용하게 되고 191ms 정도로, 꽤나 느려지게 다.
    * Proxy 처럼 작동하지만 Proxy는 아니다.

### 클래스형 컴포넌트 

#### 함수형 컴포넌트가 하지 못하는 작업을 처리하자

* 클래스형 컴포넌트의 유지보수, 함수형 컴포넌트에서 아직까지 하지 못하는 2가지 작업에 대해서 클래스형 컴포넌트로 풀어나가는 과정을 학습한다.
* 오래된 라이브러리일 경우 Hooks의 지원이 안된다.
* react-native 관련 라우터 라이브러리 react-native-navigation도 클래스형 컴포넌트를 써야한다.

{% tabs %}
{% tab title="클래스형 컴포넌트의 기본 선언방식" %}
```jsx
import React, {Component} from 'react'

export default class Hello extends Component {
    render() {
        const {color,name,isSpecial} = this.props;
        return (
            <div style={{color}}>
                {isSpecial && <b>*</b>}
                안녕하세요 {name}
            </div>
        )
    }
}
Hello.defaultProps = {
    name: '이름 없음'
};
```
{% endtab %}

{% tab title="static 을 활용한 defaultProps" %}
```jsx
import React, {Component} from 'react'

export default class Hello extends Component {
    static defaultProps = {
        name : '이름없음'
    };
    render() {
        const {color,name,isSpecial} = this.props;
        return (
            <div style={{color}}>
                {isSpecial && <b>*</b>}
                안녕하세요 {name}
            </div>
        )
    }
}
```
{% endtab %}
{% endtabs %}

* 클래스형 컴포넌트에서는 `render()` 메서드가 꼭 있어야 하고 렌더링하고 싶은 JSX 를 반환하시면 됩니다. 그리고, `props` 를 조회 해야 할 때에는 `this.props` 를 조회한다.
* `defaultProps` 를 설정하는 것은 이전 함수형 컴포넌트에서 했을 때와 동일하나  클래스 내부에 `static` 키워드와 함께 선언하는 방법도 존재한다.

#### 클래스형 컴포넌트의 다양한 상황 해결방법

{% tabs %}
{% tab title="컴포넌트의 인스턴스를 this가 가르키지 않는 상황" %}
```jsx
import React, {Component} from 'react'

export default class Counter extends Component{
    handleIncrease() {
        console.log('increase');
        console.log(this);                 // 컴포넌트 인스턴스를 가르키지 않음.
    }

    handleDecrease() {
        console.log('decrease');
    }

    render() {
        return (
            <div>
                <h1>0</h1>
                <button onClick={this.handleIncrease}>+1</button>
                <button onClick={this.handleDecrease}>-1</button>
            </div>
        );
    }
}
```
{% endtab %}

{% tab title="생성자 해결" %}
```jsx
import React, {Component} from 'react'

export default class Counter extends Component{

    constructor(props) {
        super(props);
        this.handleIncrease = this.handleIncrease.bind(this);
        this.handleDecrease = this.handleDecrease.bind(this);
    }

    handleIncrease() {
        console.log('increase');
        console.log(this);                 // 컴포넌트 인스턴스를 가르키지 않음.
    }

    handleDecrease() {
        console.log('decrease');
    }

    render() {
        return (
            <div>
                <h1>0</h1>
                <button onClick={this.handleIncrease}>+1</button>
                <button onClick={this.handleDecrease}>-1</button>
            </div>
        );
    }
}
```
{% endtab %}

{% tab title="Arrow function 해결" %}
```jsx
import React, {Component} from 'react'

export default class Counter extends Component{

    handleIncrease = () => {
        console.log('increase');
        console.log(this);
    };

    handleDecrease = () => {
        console.log('decrease');
    };

    render() {
        return (
            <div>
                <h1>0</h1>
                <button onClick={this.handleIncrease}>+1</button>
                <button onClick={this.handleDecrease}>-1</button>
            </div>
        );
    }
}
```
{% endtab %}

{% tab title="직접 함수 전달" %}
```jsx
import React, {Component} from 'react'

export default class Counter extends Component{

    handleIncrease() {
        console.log('increase');
        console.log(this);
    }

    handleDecrease() {
        console.log('decrease');
    }

    render() {
        return (
            <div>
                <h1>0</h1>
                <button onClick={() => this.handleIncrease()}>+1</button>
                <button onClick={() => this.handleDecrease()}>-1</button>
            </div>
        );
    }
}
```
{% endtab %}
{% endtabs %}

* 클래스 컴포넌트에서 이벤트를 등록하는 과정에서 각 메서드와 컴포넌트 인스턴스의 관계가 바인딩이 되지 않는 상황이 존재한다.
* 해결법은 생성자 바인, Arrow Function, 직접 함수를 전달하는 방식이 존재한다.
* CRA\(Create-React-App\)으로 생성한 프로젝트에서 클래스 컴포넌트 내부에 Arrow Function 선언을 가능하게 해주는 class-properties 라는 문법을 사용이 가능하나 정식 자바스크립트 문법이 아니니 주의해야한다.
* 직접 전달하는 법은 랜더링 때마다 함수도 새로 생성되기 때문에 컴포넌트 최적화를 위해선 배제해야한다.

#### 상태 선언, 업데이트 하기

{% tabs %}
{% tab title="state 선언" %}
```jsx
import React, {Component} from 'react'

export default class Counter extends Component {
    constructor(props) {
        super(props);
        this.state = {
            counter: 0
        };
    }
    handleIncrease = () => {
        console.log('increase');
        console.log(this);
    };

    handleDecrease = () => {
        console.log('decrease');
    };

    render() {
        return (
            <div>
                <h1>{this.state.counter}</h1>
                <button onClick={this.handleIncrease}>+1</button>
                <button onClick={this.handleDecrease}>-1</button>
            </div>
        );
    }
}
```
{% endtab %}

{% tab title="CRA에서의 상태 선언" %}
```jsx
import React, {Component} from 'react'

export default class Counter extends Component {
    state = {
        counter: 0
    };
    handleIncrease = () => {
        console.log('increase');
        console.log(this);
    };

    handleDecrease = () => {
        console.log('decrease');
    };

    render() {
        return (
            <div>
                <h1>{this.state.counter}</h1>
                <button onClick={this.handleIncrease}>+1</button>
                <button onClick={this.handleDecrease}>-1</button>
            </div>
        );
    }
}
```
{% endtab %}

{% tab title="state에 별도의 프로퍼티가 존재한다면?" %}
```jsx
import React, {Component} from 'react'

export default class Counter extends Component {
    state = {
        counter: 0,
        fixed: 1
    };
    handleIncrease = () => {
        this.setState({
            counter: this.state.counter + 1
        });
    };

    handleDecrease = () => {
        this.setState({
            counter: this.state.counter - 1
        });
    };

    render() {
        return (
            <div>
                <h1>{this.state.counter}</h1>
                <button onClick={this.handleIncrease}>+1</button>
                <button onClick={this.handleDecrease}>-1</button>
                <p>고정된 값: {this.state.fixed}</p>
            </div>
        );
    }
}
```
{% endtab %}
{% endtabs %}

* 클래스형 컴포넌트의 state는 객체형태여야 한다.
* 특별히 관리하지 않는 프로퍼티도 값은 유지된다.  하지만, 클래스형 컴포넌트의 `state` 에서 객체 형태의 상태를 관리해야 한다면, 불변성을 관리해줘가면서 업데이트를 해야한다.

#### setState의 함수형 업데이

{% tabs %}
{% tab title="setState 함수형 업데이트" %}
```jsx
import React, {Component} from 'react'

export default class Counter extends Component {
    state = {
        counter: 0,
        fixed: 1
    };
    handleIncrease = () => {
        this.setState(state => ({
            counter: state.counter + 1
        }));
    };

    handleDecrease = () => {
        this.setState(state => ({
            counter: state.counter - 1
        }));
    };

    render() {
        return (
            <div>
                <h1>{this.state.counter}</h1>
                <button onClick={this.handleIncrease}>+1</button>
                <button onClick={this.handleDecrease}>-1</button>
                <p>고정된 값: {this.state.fixed}</p>
            </div>
        );
    }
}
```
{% endtab %}

{% tab title="2번 업데이트 되지 않는경우" %}
```jsx
import React, {Component} from 'react'

export default class Counter extends Component {
    state = {
        counter: 0,
        fixed: 1
    };
    handleIncrease = () => {
        this.setState({
            counter: this.state.counter + 1
        });
        this.setState({
            counter: this.state.counter + 1
        });
    };

    handleDecrease = () => {
        this.setState(state => ({
            counter: state.counter - 1
        }));
    };

    render() {
        return (
            <div>
                <h1>{this.state.counter}</h1>
                <button onClick={this.handleIncrease}>+1</button>
                <button onClick={this.handleDecrease}>-1</button>
                <p>고정된 값: {this.state.fixed}</p>
            </div>
        );
    }
}
```
{% endtab %}

{% tab title="2번되는 경우" %}
```jsx
import React, {Component} from 'react'

export default class Counter extends Component {
    state = {
        counter: 0,
        fixed: 1
    };
    handleIncrease = () => {
        this.setState(state => ({
            counter: state.counter + 1
        }));
        this.setState(state => ({
            counter: state.counter + 1
        }));
    };

    handleDecrease = () => {
        this.setState(state => ({
            counter: state.counter - 1
        }));
    };

    render() {
        return (
            <div>
                <h1>{this.state.counter}</h1>
                <button onClick={this.handleIncrease}>+1</button>
                <button onClick={this.handleDecrease}>-1</button>
                <p>고정된 값: {this.state.fixed}</p>
            </div>
        );
    }
}
```
{% endtab %}

{% tab title="콜백함수 적용" %}
```jsx
import React, {Component} from 'react'

export default class Counter extends Component {
    state = {
        counter: 0,
        fixed: 1
    };
    handleIncrease = () => {
        this.setState(
            {
                counter: this.state.counter + 1
            },
            () => {
                console.log(this.state.counter);
            }
        );
    };

    handleDecrease = () => {
        this.setState(state => ({
            counter: state.counter - 1
        }));
    };

    render() {
        return (
            <div>
                <h1>{this.state.counter}</h1>
                <button onClick={this.handleIncrease}>+1</button>
                <button onClick={this.handleDecrease}>-1</button>
                <p>고정된 값: {this.state.fixed}</p>
            </div>
        );
    }
}
```
{% endtab %}
{% endtabs %}

* `setState` 를 한다고 해서 상태가 바로 바뀌지 않는다.
* `setState` 는 단순히 상태를 바꾸는 함수가 아니라 상태로 바꿔달라고 요청해주는 함수이기 때문이다.[\(참고\)](https://ko.reactjs.org/docs/react-component.html#setstate)
* 성능적인 이유 때문에 리액트에서는 상태가 바로 업데이트 되지 않고 비동기적으로 업데이트가 된다고 한다.
* 만약에, 상태가 업데이트 되고 나서 어떤 작업을 하고 싶다면 다음과 같이 `setState` 의 두번째 파라미터에 콜백함수를 넣어 활용한다.        

### LifeCycle Method 

#### 무엇을 공부하든 중요한 생명주기

* 컴포넌트가 브라우저 상에서 흐르는 생명주기에 관련된 메서드는 매우 중요하다.
* 클래스형 컴포넌트에서만 사용이 가능하다.
* 깊게 공부하는 시간은 따로 개인적으로 준비를 한다.

#### 생명주기 호출 과정

![&#xC0DD;&#xBA85;&#xC8FC;&#xAE30; &#xBA54;&#xC11C;&#xB4DC; &#xD750;&#xB984;](../../.gitbook/assets/image%20%2834%29.png)

#### 마운트

* constructor : 컴포넌트의 생성자 역활

```jsx
constructor(props) {
    super(props);
    console.log("constructor");
}
```

* getDerivedStateFromProps
  * props 로 받아온 값을 state에서 관리하고자 할 때 사용
  * 다른 생명주기와 달리 static 키워드가 필요하며, 내부에서 this를 조회하는 것은 불가능하다.
  * 이 생명주기에서 별도의 특정 객체를 반환하면 해당 객체의 프로퍼티가 state에 설정이 된다.
  * 반면 null 일 경우 아무런 변화가 없다.

```jsx
static getDerivedStateFromProps(nextProps, prevState) {
    console.log("getDerivedStateFromProps");
    if (nextProps.color !== prevState.color) {
        return { color: nextProps.color };
    }
    return null;
}
```

* render : 컴포넌트 렌더링을 당하는 메서드.
* componentDidMount : 컴포넌트의 첫 렌더링 후 호출되는 메서드
  * 외부 라이브러리, 컴포넌트에 필요한 데이터 요청, 직접적인 DOM의 속성을 파악 및 변경하는 용도로 사용된다.

#### 업데이트

* getDerivedStateFromProps
* shouldComponentUpdate
  * 컴포넌트의 리렌더링 여부를 결정하는 메서
  * 최적화 용도로 사용되며 React.memo와 그 기능이 유사하다.

```jsx
shouldComponentUpdate(nextProps, nextState) {
    console.log("shouldComponentUpdate", nextProps, nextState);
    // 숫자의 마지막 자리가 4면 리렌더링하지 않습니다
    return nextState.number % 10 !== 4;
}
```

* render
* getSnapshotBeforeUpdate
  * getSnapshotBeforeUpdate 는 컴포넌트에 변화 직전의 DOM 상태를 가져와서 특정 값을 반환하여 그 다음 발생하게 되는 componentDidUpdate 에서 받아와서 활용을 할 수 있습니다.

```jsx
getSnapshotBeforeUpdate(prevProps, prevState) {
  console.log("getSnapshotBeforeUpdate");
  if (prevProps.color !== this.props.color) {
    return this.myRef.style.color;
  }
  return null;
}
```

* componentDidUpdate
  * componentDidUpdate 는 리렌더링 이후, 변화가 모두 반영되고 난 뒤 호출되는 메서드입니다. 
  * 3번째 파라미터로 getSnapshotBeforeUpdate 에서 반환한 값을 조회가 가능하.

```jsx
componentDidUpdate(prevProps, prevState, snapshot) {
    console.log("componentDidUpdate", prevProps, prevState);
    if (snapshot) {
        console.log("업데이트 되기 직전 색상: ", snapshot);
    }
}
```

#### getSnapshotBeforeUpdate의 활용 예 \(스크롤바 유지하기\)

```jsx
import React, { Component } from "react";
import "../../css/ScrollBox.css";

class ScrollBox extends Component {
    id = 2;
    state = { array: [1] };

    handleInsert = () => {
        this.setState({ array: [this.id++, ...this.state.array] });
    };

    /*
        DOM 업데이트가 일어나기 직전의 시점.
        새 데이터가 상단에 추가되어도 스크롤바를 유지하는 기능을 구현.
        scrollHeight 는 전 후를 비교해서 스크롤 위치를 설정하기 위함이고,
        scrollTop 은, 이 기능이 크롬에 이미 구현이 되어있는데,
        이미 구현이 되어있다면 처리하지 않도록 하기 위함.
        이곳에서 리턴 되는 데이터는 componentDidMount 에서 snapshot 값으로 받아올 수 있습니다.
    */
    getSnapshotBeforeUpdate(prevProps, prevState) {
        if (prevState.array !== this.state.array) {
            const { scrollTop, scrollHeight } = this.list;
            return { scrollTop, scrollHeight };
        }
    }
    // 브라우저마다 scrollTop 이 구현된 것도 존재하기 때문에 기능이 이미 구현되어있다면 처리하지 않습니다.
    componentDidUpdate(prevProps, prevState, snapshot) {
        if (snapshot) {
            const { scrollTop } = this.list;
            if (scrollTop !== snapshot.scrollTop) return;
            const diff = this.list.scrollHeight - snapshot.scrollHeight;
            this.list.scrollTop += diff;
        }
    }

    render() {
        const rows = this.state.array.map(number => (
            <div className="row" key={number}>
                {number}
            </div>
        ));

        return (
            <div>
                <div
                    ref={ ref => { this.list = ref; }}
                    className="list"
                >
                    {rows}
                </div>
                <button onClick={this.handleInsert}>Click Me</button>
            </div>
        );
    }
}

export default ScrollBox;
```

#### 언마운트

* componentWillAmount
  * DOM 요소가 완전히 제거되기 직전에 발생되는 메서드로 언마운트 단계에서는 단 하나만 존재한다.
  * 주로 해당 DOM 요소에 활용하였던 비동기 메서드 및 외부 라이브러리를 이곳에서 해제한다.

```jsx
componentWillUnmount() {
    console.log("componentWillUnmount");
}
```

### componentDidCatch 로 에러 잡아내기

#### Sentry 연동 

* 본인은 GitHub 계정을 통해서 [Sentry.io](https://sentry.io/) 에 방문하여 다음 동영상과 같이 시작 절차를 진행했다.

{% file src="../../.gitbook/assets/2019-12-03-8.48.36.mov" caption="센트리 따라해보기" %}

* npm 을 통한 sentry 설치 진행

```bash
$ npm install @sentry/browser
```

* Sentry에 SDK 연결을 수행해야 한다. Sentry에서 프로젝트 설정을 완료하면 Sentry는 _DSN_ 또는 _Data Source Name_ 이라는 값을 제공합니다 . 표준 URL과 비슷하지만 Sentry SDK에 필요한 구성을 나타내며 프로토콜, 공개 키, 서버 주소 및 프로젝트 식별자를 포함하여 몇 가지로 구성된다. 해당 스크린샷을 참조하여 index.js 를 수정하여 최종적인 센트리 등록을 마치자. 

```jsx
import React from "react";
import "./css/styles.css";
import * as Sentry from "@sentry/browser";

Sentry.init({dsn: "https://ae06363733c640dfa0c19ef206eacba2@sentry.io/1842911"});
ReactDOM.render(<App />, document.getElementById('root'));
```

* 위 과정까지 마쳤다면 다음과 같이 시작하기 페이지가 변동이 되있다.

{% tabs %}
{% tab title="인증 전" %}
![](../../.gitbook/assets/image%20%287%29.png)
{% endtab %}

{% tab title="인증 후" %}
![](../../.gitbook/assets/image%20%283%29.png)
{% endtab %}
{% endtabs %}

* 그 이후 다음과 같은 Sentry UI를 확인 할 수 있다.

![](../../.gitbook/assets/image%20%2817%29.png)

#### 그래서  실제로 프로덕션 환경에서 어떻게 활용하

* 먼저 ErrorBoundary 클래스 컴포넌트를 만들어보자

```jsx
import React, { Component } from 'react';

class ErrorBoundary extends Component {
  state = {
    error: false
  };

  componentDidCatch(error, info) {
    console.log('에러가 발생했습니다.');
    console.log({
      error,
      info
    });
    this.setState({
      error: true
    });
  }

  render() {
    if (this.state.error) {
      return <h1>에러 발생!</h1>;
    }
    return this.props.children;
  }
}

export default ErrorBoundary;
```

* 주요적으로 보야하는 메서드는 `componentDidCatch` 이며 파라미터는 각각 에러의 내용과 위치 정보를 제공한다.
* velopert 는 yarn 을 활용하여 production 환경에 대한 빌드를 진행했다. 그러나 필자는 npm 을 활용하여 설치를 진행했다.

```bash
$ npm run build
$ npm install -g serve
$ serve -s build
```

* 위 명령어를 통해 모든 과정을 마치면, **localhost:5000/** 에서도 정상적으로 센트리와 연동이 완료된다. 

### 리액트 개발 할 때 사용하면 편리한 도구들

#### [Prettier](https://prettier.io/)

* 본인만의 스타일로 코드의 형태를 정할 수 있는 **code formatter** 이다.
* vscode 를 통해서는 velopert의 강좌를 따라서 진행하면된다.
* intellij 설치 과정은 다음 탭의 과정을 순서대로 진행하면 된다.

{% tabs %}
{% tab title="plugin 설치" %}
![](../../.gitbook/assets/image%20%2820%29.png)
{% endtab %}

{% tab title="prettier config 생성" %}
![](../../.gitbook/assets/image%20%2818%29.png)
{% endtab %}

{% tab title="본인 취향의 문법 포맷 지정" %}
![](../../.gitbook/assets/image.png)
{% endtab %}

{% tab title="최종적으로 To use it 따라하기" %}
![](../../.gitbook/assets/image%20%2836%29.png)
{% endtab %}
{% endtabs %}

* 변경 전과 후를 비교해본다.

{% tabs %}
{% tab title="변경 전" %}
```bash
import React, {useReducer} from 'react';

function reducer(state, action) {
    switch (action.type) {
        case 'INCREMENT' :
            return state + 1;
        case 'DECREMENT' :
            return state - 1;
        default :
            return state;
    }
}

export default function Counter() {
    const [number, dispatch] = useReducer(reducer,0);

    const onIncrease = () => { dispatch({type:'INCREMENT'}); };
    const onDecrease = () => { dispatch({type:'DECREMENT'}); };

    return (
        <div>
            <h1>{number}</h1>
            <button onClick={onIncrease}>+</button>
            <button onClick={onDecrease}>-</button>
        </div>
    );
}
```
{% endtab %}

{% tab title="변경 후" %}
```bash
import React, {useReducer} from 'react';

function reducer(state, action) {
    switch (action.type) {
        case 'INCREMENT' :
            return state + 1;
        case 'DECREMENT' :
            return state - 1;
        default :
            return state;
    }
}

export default function Counter() {
    const [number, dispatch] = useReducer(reducer,0);

    const onIncrease = () => { dispatch({type:'INCREMENT'}); };
    const onDecrease = () => { dispatch({type:'DECREMENT'}); };

    return (
        <div>
            <h1>{number}</h1>
            <button onClick={onIncrease}>+</button>
            <button onClick={onDecrease}>-</button>
        </div>
    );
}
```
{% endtab %}
{% endtabs %}

#### ESLint

* create-react-app 으로 개발 시에는 미리 경험을 해본 관계로 생략한다.

#### Snippet 

* 코드관련 템플릿에 관한 것이며, vscode 기준이기도 하며 다양한 플러그인을 intellij에서도 지원하니 취향거 선택하여 사한다.



