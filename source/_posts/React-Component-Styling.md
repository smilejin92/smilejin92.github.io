---
title: React Component Styling 1
thumbnail: /thumbnails/react.png
tag: React
categories:
- [TIL, React]
---

## Style Loaders

`create-react-app` (CRA)으로 생성한 React 앱의 컴포넌트를 스타일링 하는 방법은 기본적으로 네 가지가 있다.  `npm run eject` 하여 webpack.config.js 파일을 살펴보면 각 방법에 대해 아래와 같이 정의되어있다.

(코드가 매우 많으므로 필요한 부분만 발췌)

#### 1. CSS

```javascript
// webpack.config.js

const cssRegex = /\.css$/;
const cssModuleRegex = /\.module\.css$/;

module.exports = function(webpackEnv) {
  
  // common function to get style loaders
  const getStyleLoaders = (cssOptions, preProcessor) => {
    const loaders = [
      // style-loader,
      // css-loader,
      // postcss-loader
    ].filter(true);

    return loaders;
  };

  return {
    module: {
      rules: [
        {
          oneOf: [
            {
              test: cssRegex,
              exclude: cssModuleRegex,
              use: getStyleLoaders({
                importLoaders: 1,
                sourceMap: isEnvProduction && shouldUseSourceMap,
              }),
              sideEffects: true
            },
          ]
        }
      ]
    }
  };
}
```

`cssRegex` 정규표현식으로 파일 확장자를 테스트하여 `getStyleLoaders` 함수를 통해 style loader를 불러온다.

```jsx
// App.jsx
import './App.css';
```
#### 2. CSS Module

```javascript
// webpack.config.js

const cssRegex = /\.css$/;
const cssModuleRegex = /\.module\.css$/;

module.exports = function(webpackEnv) {
  
  // common function to get style loaders
  const getStyleLoaders = (cssOptions, preProcessor) => {
    const loaders = [
      // style-loader,
      // css-loader,
      // postcss-loader
    ].filter(true);

    return loaders;
  };

  return {
    module: {
      rules: [
        {
          oneOf: [
            {
              test: cssModuleRegex,
              use: getStyleLoaders({
                importLoaders: 1,
                sourceMap: isEnvProduction && shouldUseSourceMap,
                modules: {
                  getLocalIdent: getCSSModuleLocalIdent,
                },
              }),
            },
          ]
        }
      ]
    }
  };
}
```

`cssModuleRegex` 정규표현식으로 파일 확장자를 테스트하여 `getStyleLoaders` 함수를 통해 style loader를 불러온다.

```javascript
import styles from './App.module.css';
```
#### 3. Sass

```javascript
// webpack.config.js

const sassRegex = /\.(scss|sass)$/;
const sassModuleRegex = /\.module\.(scss|sass)$/;

module.exports = function(webpackEnv) {
  
  // common function to get style loaders
  const getStyleLoaders = (cssOptions, preProcessor) => {
    const loaders = [
      // style-loader,
      // css-loader,
      // postcss-loader
    ].filter(true);

    return loaders;
  };

  return {
    module: {
      rules: [
        {
          oneOf: [
            {
              test: sassRegex,
              exclude: sassModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 2,
                  sourceMap: isEnvProduction && shouldUseSourceMap,
                },
                'sass-loader'
              ),
              sideEffects: true,
            },
          ]
        }
      ]
    }
  };
}
```

`sassRegex` 정규표현식으로 파일 확장자를 테스트하여 `getStyleLoaders` 함수를 통해 style loader를 불러온다. .scss 혹은 .sass 파일을 사용하기 위해서는 `node-sass` 모듈을 설치해야한다.

```javascript
import './App.scss';
import './App.sass';
```

> **.scss vs. .sass**
>
> 두 확장자 모두 pre-processor scripting language 파일이며 트랜스파일링 이후 CSS로 변환된다. 문법적 차이는 있지만 제공하는 기능은 동일하다. 어떠한 기능을 제공하는지는 [Sass 공식문서](https://sass-lang.com/documentation)에서 확인할 수 있다.

#### 4. Sass Module

```javascript
// webpack.config.js

const sassRegex = /\.(scss|sass)$/;
const sassModuleRegex = /\.module\.(scss|sass)$/;

module.exports = function(webpackEnv) {
  
  // common function to get style loaders
  const getStyleLoaders = (cssOptions, preProcessor) => {
    const loaders = [
      // style-loader,
      // css-loader,
      // postcss-loader
    ].filter(true);

    return loaders;
  };

  return {
    module: {
      rules: [
        {
          oneOf: [
            {
              test: sassModuleRegex,
              use: getStyleLoaders(
                {
                  importLoaders: 2,
                  sourceMap: isEnvProduction && shouldUseSourceMap,
                  modules: {
                    getLocalIdent: getCSSModuleLocalIdent,
                  },
                },
                'sass-loader'
              ),
            },
          ]
        }
      ]
    }
  };
}
```

`sassModuleRegex` 정규표현식으로 파일 확장자를 테스트하여 `getStyleLoaders` 함수를 통해 style loader를 불러온다.

```javascript
import styles from './App.module.scss';
import styles from './App.module.sass';
```
<br />

<hr />

## CSS, Sass 적용

#### App.css (ver.1)
```javascript
// App.jsx
import './App.css'
```
![](/images/component-styling/app-css-1.PNG)

#### App.css (ver.2)
```javascript
// App.jsx
import './App.css'
```
![](/images/component-styling/app-css-2.PNG)

#### App.scss
```javascript
// App.jsx
import './App.scss'
```
![](/images/component-styling/app-scss.PNG)

**Sass는 스타일의 중첩관계를 표현할 수 있다.** Sass를 사용하기 위해서는 `node-sass` 모듈을 설치해야한다.

```bash
$ npm i node-sass
```
<br />

<hr />

## CSS 방법론

CSS의 활용도가 높아짐에 따라 코드의 재사용성을 높이고, 유지보수의 시간을 최소화하기 위해 다양한 방법론들이 등장하였다.

#### 1. [SMACSS](http://smacss.com/)

#### 2. [CSS Guidelines](https://cssguidelin.es/)

#### 3. [OOCSS](http://oocss.org/)

#### 4. [BEM](https://en.bem.info/methodology/)

#### 5. [idiomatic CSS](https://github.com/necolas/idiomatic-css)

<br />

<hr />

## [CSS Module, SASS Module 적용](https://create-react-app.dev/docs/adding-a-css-modules-stylesheet)

> Note: `react-scripts@2.0.0` 이상에서만 지원

CRA로 생성한 React 앱에서는 파일 이름이 `[name].module.css` 로 지정된 스타일시트를 CSS 모듈로서 사용할 수 있다. CSS 모듈은 CSS 클래스 이름을 `[filename]\_[classname]\_\_[hash]` 형식으로 변환하므로서 중복되지 않는 클래스 이름을 생성한다. 따라서 모듈 단위로 CSS 스코프를 생성할 수 있다.

> **Tip**: Sass로 스타일시트를 preprocess한 후 module로서 사용하고 싶다면, 스타일시트의 확장자를 `[name].module.scss` 혹은 `[name].module.sass`로 변경해야한다.

CSS 모듈은 동일한 CSS 클래스 이름을 서로 다른 파일 내에서 충돌 없이 사용할 수 있게한다.

![](/images/component-styling/app-css-module.PNG)

위의 예제처럼 CSS 모듈을 하나의 스타일 객체로서 import하여, 스타일 객체(`styles`)의 프로퍼티 값을 대괄호 표기법으로 접근할 수 있다. 

<br />

다음 포스트에서는 스타일 모듈을 React 컴포넌트에 적용하는 방법에 대해 알아보자.

