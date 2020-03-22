---
title: React Component Styling 2
thumbnail: /thumbnails/react.png
tag: React
categories:
- [TIL, React]
---

## React 컴포넌트에 CSS 모듈 적용하기

이전 포스트에서 살펴보았듯 스타일시트 파일 이름을 `[name].module.css` 형식으로 지정하면 CSS 모듈로서 사용할 수 있다. CSS 모듈은 CSS 클래스 이름을 `[name]\_[classname]\_\_[hash]` 형식으로 변환하여 새로운 CSS 클래스 이름을 생성한다.

아래는 CRA로 생성한 React 앱에서 Button.module.css를 `styles` 객체로 `import`하여 `Button` 컴포넌트에 적용하여 렌더한 예제이다. 

![](/images/component-styling/button-module.PNG)

브라우저에 렌더된 버튼을 개발자 도구로 살펴보면, Button.module.css의 `button` 클래스가 `Button_button__1Acsh` 로 변환되어 적용된 것을 확인할 수 있다.

클래스 컴포넌트의 상태에 따라 스타일을 변경할 수 있다. 아래는 이전의 `Button` 컴포넌트를 확장한 예제이다.

![](/images/component-styling/class-button-module.PNG)

버튼이 클릭될 때마다 `Button_clicked__[hash]` 클래스가 토글되어 스타일이 반전된다.

<br />

<hr />
## [classnames](https://github.com/JedWatson/classnames) 모듈

이전 버튼 컴포넌트를 살펴보면 상태가 변경될때마다 `className`을 동적으로 변경해주었다. 문제는 `className`에 할당하려는 CSS class 이름이 너무 길다는 것이다. `classnames` 모듈을 활용하여 리팩토링 해보자.

```bash
$ npm i classnames
```

classnames 모듈은 조건이 변경될때마다 `className`에 할당되는 CSS class 이름을 하나의 String으로 붙여 반환해주는 모듈이다.

```javascript
import classNames from 'classnames';

classNames('foo', 'bar'); // "foo bar"
classNames('foo', 'bar', 'baz'); // "foo bar baz"
classNames({ foo: true }, { bar: false }); // "foo"
classNames(null, false, 'bar', undefined, 0, 1, { bax: null }, ''); // "bar 1"
```

이전 버튼 컴포넌트에서 `import`한 `styles` 객체의 프로퍼티를 `classNames` 모듈에서 사용할 수 있다.

```javascript
import classNames from 'classnames';
import styles from './Button.module.css';

classNames(styles.button, styles.clicked);
// "Button_button__1Acsh Button_clicked__1YQBe"
```

이를 활용하여 버튼 컴포넌트 예제를 수정해보자.

```jsx
import React, { Component } from 'react';
import ReactDOM from 'react-dom';
import styles from './Button.module.css';
import classNames from 'classnames';

class Button extends Component {
  state = {
    clicked: false
  };

  toggleButton = () => {
    this.setState(prev => ({ clicked: !prev.clicked }));
  }

  render() {
    const { clicked } = this.state;
    return (
      <button
        className={
          clicked 
            ? classNames(styles.button, styles.clicked)
            : styles.button
        }
        onClick={this.toggleButton}
        {...this.props}
      />
    );
  }
}

ReactDOM.render(
  <Button>버튼</Button>, 
  document.getElementById('root')
);
```

`classNames`를 사용해도 그닥 달리진 것이 없어보인다. 이번엔 `classnames/bind`를 활용하여 `styles` 객체를 `bind`해보자.

```javascript
import React, { Component } from 'react';
import classNames from 'classnames/bind';
import styles from './Button.module.css';

const cx = classNames.bind(styles);

cx('button', 'clicked'); // "Button_button__1Acsh Button_clicked__1YQBe"
cs('button', { clicked: false }); // "Button_button__1Acsh"

class Button extends Component {
  state = {
    clicked: false
  };

  toggleButton = () => {
    this.setState(prev => ({ clicked: !prev.clicked }));
  }

  render() {
    const { clicked } = this.state;
    return (
      <button
        className={cx('button', { clicked })}
        onClick={this.toggleButton}
        {...this.props}
      />
    );
  }
}
```

`classNames`에 `styles` 객체를  `bind`하면 코드가 매우 간결해진다.



## [styled-components](https://github.com/styled-components/styled-components)

`styled-components` 모듈은 React 컴포넌트를 스타일하기 위한 CSS 코드를 직접 사용할 수 있게 해주는 모듈이다. 다양한 예제를 통해 사용 방법을 익혀보자.

```bash
$ npm i styled-components
```

#### styled.<태그>\`스타일\`

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import styled from styled-components;

const StyledButton = styled.button`
  background: transparent;
  border-radius: 3px;
  border: 2px solid palevioletred;
  color: palevioletred;
  margin: 0 1em;
  padding: 0.25em 1em;
`;

function App() {
  return (
    <StyledButton>버튼</StyledButton>
  );
}

ReactDOM.render(<App />, document.getElementId('root'));
```

#### styled('태그')\`스타일\` = styled.<태그>\`스타일`

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import styled from 'styled-components';

const StyledButton = styled('button')`
  background: transparent;
  border-radius: 3px;
  border: 2px solid palevioletred;
  color: palevioletred;
  margin: 0 1em;
  padding: 0.25em 1em;
`;

function App() {
  return (
    <StyledButton>버튼</StyledButton>
  );
}

ReactDOM.render(<App />, document.getElementId('root'));
```

#### ${props => css\`스타일\`}

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import styled, { css } from 'styled-component';

const StyledButton = styled.button`
  background: transparent;
  border-radius: 3px;
  border: 2px solid palevioletred;
  color: palevioletred;
  margin: 0 1em;
  padding: 0.25em 1em;
	
	${props => props.primary && css`
		background: palevioletred;
    	color: white;
	`}
`;

function App() {
  return (
    <>
      <StyledButton>버튼</StyledButton>
      <StyledButton primary>버튼</StyledButton>
    </>
  );
}

ReactDOM.render(<App />, document.getElementId('root'));
```

#### ${props => props.color || ''}

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import styled from 'styled-component';

const StyledButton = styled.button`
  background: transparent;
  border-radius: 3px;
  border: 2px solid ${props => props.color || 'palevioletred'};
  color: ${props => props.color || 'palevioletred'};
  margin: 0 1em;
  padding: 0.25em 1em;
`;

function App() {
  return (
    <>
      <StyledButton>버튼</StyledButton>
      <StyledButton color="green">버튼</StyledButton>
    </>
  );
}

ReactDOM.render(<App />, document.getElementId('root'));
```

#### styled(스타일 컴포넌트)

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import styled from 'styled-component';

const StyledButton = styled.button`
	background: transparent;
  border-radius: 3px;
  border: 2px solid palevioletred;
  color: palevioletred;
  margin: 0 1em;
  padding: 0.25em 1em;
`;

const PrimaryStyledButton = styled(StyledButton)`
	background: palevioletred;
	color: white;
`;

function App() {
  return (
    <>
      <StyledButton>버튼</StyledButton>
      <PrimaryStyledButton>버튼</PrimaryStyledButton>
    </>
  );
}

ReactDOM.render(<App />, document.getElementId('root'));
```

#### styled(컴포넌트)

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import styled from 'styled-component';

const MyButton = ({ className, children }) => (
	<button className={className}>{children}</button>
);

const StyledButton = styled(MyButton)`
	background: transparent;
  border-radius: 3px;
  border: 2px solid palevioletred;
  color: palevioletred;
  margin: 0 1em;
  padding: 0.25em 1em;
`;

function App() {
  return (
    <>
      <MyButton className="button">버튼</MyButton>
      <StyledButton className="styled-button">버튼</StyledButton>
    </>
  );
}

ReactDOM.render(<App />, document.getElementId('root'));
```

#### :hover {스타일}, ::before {스타일}

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import styled from 'styled-component';

const StyledButton = styled.button`
	background: transparent;
  border-radius: 3px;
  border: 2px solid palevioletred;
  color: palevioletred;
  margin: 0 1em;
  padding: 0.25em 1em;
	
	:hover {
		border: 2px solid red;
	}

	::before {
		content: '@';
	}
`;

function App() {
  return (
    <StyledButton>버튼</StyledButton>
  );
}

ReactDOM.render(<App />, document.getElementId('root'));
```

#### &:hover {스타일}, &.클래스 {스타일}, .클래스 {스타일}

```jsx
import React from 'react';
import ReactDOM from 'react-dom';
import styled from 'styled-component';

// &는 부모 선택자이다. 
// &:hover는 StyledButton:hover과 동일하다.
// &.orange는 StyledButton.orange와 동일하다.
// .orange는 StyledButton > .orange와 동일하다.
const StyledButton = styled.button`
	background: transparent;
  border-radius: 3px;
  border: 2px solid palevioletred;
  color: palevioletred;
  margin: 0 1em;
  padding: 0.25em 1em;
	
	&:hover {
		border: 2px solid red;
	}
	
	&.green {
		border: 2px solid orange;
	}
	
	.orange {
		color: orange;	
	}
`;

function App() {
  return (
    <StyledButton className="green">
      <a className="orange">버튼</a>
    </StyledButton>
  );
}

ReactDOM.render(<App />, document.getElementId('root'));
```

#### createGlobalStyle\`스타일\`

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import styled, { createGlobalStyle } from 'styled-component';

const StyledButton = styled.button`
	background: transparent;
  border-radius: 3px;
`;

// 렌더되는 컴포넌트 상단에 배치하면 하위 컴포넌트에 해당 CSS 속성이 부여된다.
// 특정 컴포넌트를 대상으로만 스타일을 추가할 수도 있다.
const GlobalStyle = createGlobalStyle`
	* {
		padding: 0;
	}
	
	button${StyledButton} {
		border: 2px solid palevioletred;
		color: palevioletred;
		margin: 0 1em;
    padding: 0.25em 1em;
	}
`;

function App() {
  return (
    <>
      <GlobalStyle />
      <StyledButton>버튼</StyledButton>
    </>
  );
}

ReactDOM.render(<App />, document.getElementId('root'));
```

#### styled.<태그>.attrs(props => ({ 속성들 }))\`스타일`

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import styled, { attrs } from 'styled-components';

const StyledA = styled.a.attrs(props => ({
  href: props.href || 'defaulturl.com',
  color: props.color || 'palevioletred',
  target: '_BLANK'
}))`
	color: ${props => props.color};
`;

function App() {
  return (
    <>
      <StyledA>기본 링크</StyledA>
      <StyledA color="red">빨간 링크</StyledA>
    </>
  );
}

ReactDOM.render(<App />, document.getElementId('root'));
```

#### keyframes\`키프레임\`

```javascript
import React from 'react';
import ReactDOM from 'react-dom';
import styled, { keyframes } from 'styled-components';

const slide = keyframes`
	from {
		margin-top: 0em;
	}

	to {
		margin-top: 1em;
	}
`;

const StyleButton = styled.button`
	display: inline-block;
  color: palevioletred;
  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
  animation: ${slide} 0.3s ease-in;
`;

function App() {
  return (
    <StyleButton />
  );
}

ReactDOM.render(<App />, document.getElementId('root'));
```

