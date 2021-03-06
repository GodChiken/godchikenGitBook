# 컴포넌트 스타일링

### SASS

#### 개요  

* Sass \(Syntactically Awesome Style Sheets: 문법적으로 짱 멋진 스타일시트\) 는 CSS pre-processor\(전처리기\) 로서, 복잡한 작업을 쉽게 할 수 있게 해주고, 코드의 재활용성을 높여줄 뿐 만 아니라, 코드의 가독성을 높여주어 유지보수를 쉽게 한다.
* 본 강의에서 기초적인 수준만 다루기 때문에 조금 더 살펴 보려면 다음 글을 확인한다
  *  [벨로퍼트 포스트](https://velopert.com/1712), [Sass 공식 가이드](https://sass-guidelin.es/ko/) 참고
* 다음 명령어로 설치
  * ```
    $ npm install node-sass
    ```
* 그리고 다음 코드를 실습

{% tabs %}
{% tab title="Button" %}
```jsx
import React from 'react';
import './Button.scss';

export default function Button({ children }) {
    return <button className="Button">{children}</button>;
}
```
{% endtab %}

{% tab title="Button SCSS" %}
```css
 $blue: #228be6; // 주석 선언

.Button {
  display: inline-flex;
  color: white;
  font-weight: bold;
  outline: none;
  border-radius: 4px;
  border: none;
  cursor: pointer;

  height: 2.25rem;
  padding-left: 1rem;
  padding-right: 1rem;
  font-size: 1rem;

  background: $blue; // 주석 사용
  &:hover {
    background: lighten($blue, 10%); // 색상 10% 밝게
  }

  &:active {
    background: darken($blue, 10%); // 색상 10% 어둡게
  }
}
```
{% endtab %}

{% tab title="App" %}
```jsx
import React from 'react';
import './App.scss';
import Button from './Button';

export default function App() {
    return (
        <div className="App">
            <div className="buttons">
                <Button>BUTTON</Button>
            </div>
        </div>
    );
}
```
{% endtab %}

{% tab title="App SCSS" %}
```css
.App {
  width: 512px;
  margin: 0 auto;
  margin-top: 4rem;
  border: 1px solid black;
  padding: 1rem;
}
```
{% endtab %}
{% endtabs %}

![&#xCCAB; SCSS &#xCEF4;&#xD3EC;&#xB10C;&#xD2B8;](../../.gitbook/assets/image%20%2861%29.png)

#### 버튼 사이즈 조정해보기

* default props를 통해 기본 사이즈를 지정하고 그외에 앞으로 만들 다양한 class attribute를 조건에 따라서 다양하게 주입하기 위한 classnames를 설치 후에 진행해보자.

```bash
$ npm install classnames
```

* 위 라이브러리를 사용하면 다음과 같은 이점을 취할 수 있다.

```jsx
// classnames 미적용시 
className={['Button', size].join(' ')}
className={`Button ${size}`}

// classnames 사용법

classNames('foo', 'bar'); // => 'foo bar'
classNames('foo', { bar: true }); // => 'foo bar'
classNames({ 'foo-bar': true }); // => 'foo-bar'
classNames({ 'foo-bar': false }); // => ''
classNames({ foo: true }, { bar: true }); // => 'foo bar'
classNames({ foo: true, bar: true }); // => 'foo bar'
classNames(['foo', 'bar']); // => 'foo bar'

// 여러개의 타입을 다양하게 받아올 수 있다.
classNames('foo', { bar: true, duck: false }, 'baz', { quux: true }); // => 'foo bar baz quux'

// false, null, 0, undefined 는 무시됩니다.
classNames(null, false, 'bar', undefined, 0, 1, { baz: null }, ''); // => 'bar 1'
```

#### 간격 조정하기

```css
// 버튼과 버튼 사이 간격
& + & {
  margin-left: 1rem;
}  
```

#### 색상 주입하기

{% tabs %}
{% tab title="Button" %}
```jsx
import React from 'react';
import classNames from 'classnames';
import './Button.scss';

export default function Button({ children, size, color }) {
    return <button className={classNames('Button', size, color)}>{children}</button>;
}

Button.defaultProps = {
    size: 'medium',
    color : 'blue'
}; 
```
{% endtab %}

{% tab title="Button CSS" %}
```css
$blue: #228be6;

.Button {
  display: inline-flex;
  color: white;
  font-weight: bold;
  outline: none;
  border-radius: 4px;
  border: none;
  cursor: pointer;

  // 사이즈 관리
  &.large {
    height: 3rem;
    padding-left: 1rem;
    padding-right: 1rem;
    font-size: 1.25rem;
  }

  &.medium {
    height: 2.25rem;
    padding-left: 1rem;
    padding-right: 1rem;
    font-size: 1rem;
  }

  &.small {
    height: 1.75rem;
    font-size: 0.875rem;
    padding-left: 1rem;
    padding-right: 1rem;
  }

  // 색상 관리
  &.blue {
    background: $blue;
    &:hover {
      background: lighten($blue, 10%);
    }

    &:active {
      background: darken($blue, 10%);
    }
  }

  &.gray {
    background: $gray;
    &:hover {
      background: lighten($gray, 10%);
    }

    &:active {
      background: darken($gray, 10%);
    }
  }

  &.pink {
    background: $pink;
    &:hover {
      background: lighten($pink, 10%);
    }

    &:active {
      background: darken($pink, 10%);
    }
  }
  // 버튼과 버튼 사이 간격
  & + & {
    margin-left: 1rem;
  }
}
```
{% endtab %}

{% tab title="결과" %}
![](../../.gitbook/assets/image%20%281%29.png)
{% endtab %}
{% endtabs %}

#### 다양한 실습

* [x] @mixin 실습
* [x] Button 세로간격 지정.
* [x] outline 만들기.
* [x] full width 만들기.
* [x] onEvent rest로 전

{% tabs %}
{% tab title="Button.scss @mixin 적용" %}
```css
$blue: #228be6;
$gray: #495057;
$pink: #f06595;

@mixin button-color($color){
  background-color: $color;
  &:hover {
    background : lighten($color, 10%);
  }
  &:active {
    background: darken($color, 10%);
  }  
}
```
{% endtab %}

{% tab title="App.scss 세로간격 적용" %}
```css
.App {
  width: 512px;
  margin: 0 auto;
  margin-top: 4rem;
  border: 1px solid black;
  padding: 1rem;
  .buttons + .buttons {
    margin-top: 1rem;
  }
}
```
{% endtab %}

{% tab title="outline 만들기" %}
```css
@mixin button-color($color){
  background-color: $color;
  &:hover {
    background : lighten($color, 10%);
  }
  &:active {
    background: darken($color, 10%);
  }
  &.outline {
    color: $color;
    background: none;
    border: 1px solid $color;
    &:hover {
      background: $color;
      color: white;
    }
  }
}
```
{% endtab %}

{% tab title="full width 만들기" %}
```css
&.fullWidth {
    width: 100%;
    justify-content: center;
    & + & {
      margin-left: 0;
      margin-top: 1rem;
    }
}
```
{% endtab %}

{% tab title="onEvent rest로 전달" %}
```jsx
import React from 'react';
import classNames from 'classnames';
import './Button.scss';

export default function Button({ children, size, color, outline, fullWidth, ...rest }) {
    return (
        <button className={classNames('Button', size, color, { outline, fullWidth })} {...rest} >
            {children}
        </button>
    );
}

Button.defaultProps = {
    size: 'medium',
    color : 'blue'
};
```
{% endtab %}
{% endtabs %}

* 전체 코드는 다음 링크의 28.SASS branch를 참고한다.
* [https://github.com/GodChiken/fast-campus-react/tree/28.SASS](https://github.com/GodChiken/fast-campus-react/tree/28.SASS)

![&#xC2E4;&#xC2B5; &#xC644;&#xB8CC;&#xBD84;](../../.gitbook/assets/image%20%2845%29.png)

### CSS Module

리액트 프로젝트에서 컴포넌트를 스타일링 할 때 CSS Module 이라는 기술을 사용하면 CSS 클래스가 중첩되는 것을 완벽히 방지할 수 있다고 소개된다. 사용법은 CRA 로 만든 프로젝트에서 CSS Module 를 사용 할 때에는, CSS 파일의 확장자를 `.module.css` 로 하면 되는데요, 예를 들어서 다음과 같이 `Box.module.css` 라는 파일을 만들고 컴포넌트에서 활용하게 되면 css 파일 내부에서 선언한 클래스 이름들이 모두 고유해진다. 고유 CSS 클래스 이름이 만들어지는 과정에서는 파일 경로, 파일 이름, 클래스 이름, 해쉬값 등이 사용 될 수 있다. 

{% tabs %}
{% tab title="Box.module.css" %}
```css
.Box {
  background: black;
  color: white;
  padding: 2rem;
}
```
{% endtab %}

{% tab title="Box Component" %}
```jsx
import React from "react";
import styles from "./Box.module.css";

function Box() {
  return <div className={styles.Box}>{styles.Box}</div>;
}

export default Box;
```
{% endtab %}
{% endtabs %}

`className` 을 설정 할 때에는 `styles.Box` 이렇게 `import`로 불러온 `styles` 객체 안에 있는 값을 참조해야 합니다.

### Styled-component

### 번외 : Emotion

