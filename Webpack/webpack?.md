### Webpack ??

웹사이트를 구성할 때 여러 개의 파일을 하나의 파일로 합쳐주는 **모듈 번들러**이다.

만약 웹사이트에 접속할때 여러개의 파일(js, css, images)들이 다운로드 되는 데 이때 서버의 자원을 소모하고
느리게 로딩된다. <br/> 또한, 많은 자바스크립트 패키지들이 같은 이름과 함수를 사용하면서 깨질 수 있다. 이러한 현상을 해결하기 위해 번들러가 등장.

webpack은 모던 JavaScript 애플리케이션을 위한 정적 모듈 번들러이다.

### 쉽게 말하면 Webpack은 여러 파일을 하나 이상의 파일로 합쳐주는 자바스크립트 번들러이다!
<br/>

## 왜 필요한가?

#### 성능 최적화 & 자동화
코드 축소와 더불어 사용하지 않는 코드를 제거하는 Tree shaking과 같은 최적화를 수행해 HTTP 리퀘스트 수를
감소하여 웹사이트 성능을 궁극적으로 향상시키고, 로딩속도를 빠르게 해준다.

#### 파일 단위의 자바스크립트 모듈 관리
HTML, CSS, JS, Images, Font 등 모든 파일 하나하나 나눠서 모듈화하여 웹을 구성.

#### 번들러가 제공하는 편의성
CSS가 아닌 Sass나 Stylus 등을 사용하는 경우, 또는 TypeScript 사용시 번들러가 컴파일 과정에서 필요한
플러그인을 추가하고 번들러를 실행해준다.

#### Dependency Issue(종속성 문제) 해결
서버와 브라우저 모두에서 최대한 원활하게 작동할 수 있는 코드의 상당 부분을 빌드시 모든 종속성과 함께 번들하는 데 도움을 준다.

## Webpack의 핵심 요소

**Entry** <br/>
엔트리 속성은 웹팩에서 웹 자원을 변환하기 위해 필요한 최초 진입점이다.
```js
  module.export = {
    entry: './src/index.js'
  }

  module.exports = {
    entry: {
      login: './src/LoginView.js,
      main: './src/MainView.js
    }
  }
```
최초 진입점이 되는 대상 파일은 웹 애플리케이션의 전반적인 구조와 내용이 담겨있어야한다.
웹팩이 해당 파일을 토대로 웹 애플리케이션의 모듈들의 연관관계에 대해 이해하고 분석하고 합치기 때문.
<br/>
<br/>

**Output** <br/>
웹팩을 실행하고 빌드하고 난 후 결과물의 파일 경로를 의미한다.
```js
var path = require('path');
module.exports = {
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, './dist')
  }
}
output: './dist/bundle.js'
```
filename은 웹팩으로 빌드한 파일의 이름을 의미,
path는 해당 파일의 경로를 의미한다.
<br/>
<br/>

**Loader** <br/>
웹팩이 애플리케이션을 해석할때 자바스크립트를 제외한 파일(html, css, images, font 등)을 변환할 수 있게 도와주는 속성.

웹팩은 모든 파일을 모듈로 취급하는데 사실상 자바스크립트 파일만 알고 있어 로더를 이용해 다른 파일들을 변경해줘야한다.

**로더로 설정을 지정하지 않으면 웹팩이 해당 파일을 읽을 수 없기 때문에 에러가 발생한다**
```js
import './common.css';

console.log('css loaded');

p {
  color: blue;
}

module.exports = {
  entry: './app.js',
  output: {
    filename: 'bundle.js',
  }
}
```
이 파일을 빌드하면 오류가 발생한다. 왜?

Loader가 필요하다. <br/>

보통 사용되는 로더 종류 <br/>
- CSS Loader
- Babel Loader
- Sass Loader
- File Loader
- Vue Loader
- TS Loader
<br/>
아래와 같이 rules라는 객체로 속성을 지정하며,
test => 로더를 적용할 파일 유형 (일반적으로 정규 표현식)
use => 해당 파일에 적용할 로더의 이름

아래 코드는 해당 프로젝트의 모든 CSS 파일과 TS파일에 대해서 로더를 적용하겠다는 의미로 해석할 수 있다.
```js
module.exports = {
  module: {
    rules: [
      { test: /\.css$/, use: 'css-loader' },
      { test: /\.ts$/, use: 'ts-loader' },
      // ...
    ]
  }
}
```
<br/>
<br/>

**Plugin** <br/>
웹팩의 기본적인 동작에 추가적인 기능을 제공하는 속성이다.
로더는 파일을 해석하고 변환하는 과정에 관여한다면,
플러그인은 **해당결과물의 형태를 바꾸는 역할**을 한다고 볼 수 있다.

```js
var webpack = require('webpack');
var HtmlWebpackPlugin = require('html-webpack-plugin');

module.exports = {
  plugins: [
    new HtmlWebpackPlugin(),
    new webpack.ProgressPlugin()
  ]
}
```

























