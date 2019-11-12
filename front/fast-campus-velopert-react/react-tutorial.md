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

![&#xB9AC;&#xC561;&#xD2B8;&#xC758; DOM Rendering &#xACFC;&#xC815;](../../.gitbook/assets/image%20%286%29.png)

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

* 다음과 같이  `Hello` 컴포넌트에 `defaultProps` 를 활용하는 법을 배웠다.

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

### useState 를 통해 컴포넌트에서 바뀌는 값 관리하기 

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

### input 상태 관리하기 

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

### 여러개의 input 상태 관리하기 

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

![\[name\] , name &#xC758; &#xCC28;&#xC774;&#xC810;](../../.gitbook/assets/image%20%2825%29.png)

* 궁금할때 직접 찍어보는 것도 하나의 답이 된다.

### useRef 로 특정 DOM 선택하기 

#### DOM을 선택해야하는 상황과 리액트에서 처리하는 방향

* 리액트에서 직접적으로 `DOM`을 선택해야 하는 상황이 있다.
  * 엘리먼트의 크기, 위치, 포커스 설정
  * 외부 라이브러리
* 위와 같은 상황일 경우 리액트에서는 `ref` 라는 것을 사용한다.
* 함수형 컴포넌트의 경우 `Hook`의 `useRef` 함수를 사용.
* 클래스형 컴포넌트의 경우 `React.createRef` 함수를 사용.

#### 특정 DOM의 포커싱에 대해 처리해보자

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

![key props &#xC124;&#xC815;&#xC744; &#xC548;&#xD55C; &#xACBD;&#xC6B0;](../../.gitbook/assets/image%20%283%29.png)

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

![&#xB9AC;&#xB79C;&#xB354;&#xB9C1;&#xB9C8;&#xB2E4; &#xD638;&#xCD9C;](../../.gitbook/assets/image%20%288%29.png)

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
* custom Hooks 를 생성하여 재사용하는 방법에 대해서 실습했다. 

#### custom hooks : useState -&gt; useReducer 전환 연습하기

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

#### 본인은 어떤 식으로 전환했나요

* 기존의 App Component에서 작성한 reducer 함수를 토대로 동일하게 작성을 시도했다.
* 하지만 case 'FORM\_RESET' 에서 input 의 상태값을

### Context API 를 사용한 전역 값 관리 



### Immer 를 사용한 더 쉬운 불변성 관리 



### 클래스형 컴포넌트 



### LifeCycle Method 



### componentDidCatch 로 에러 잡아내기 / Sentry 연동 



### 리액트 개발 할 때 사용하면 편리한 도구들 - Prettier, ESLint, Snippet 



