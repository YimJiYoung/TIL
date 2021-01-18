## Babel
ES6 이상의 최신 문법을 지원하지 않는 브라우저에 대해서 동작할 수 있도록 이전 문법으로 변환해주는 **Transpiler**
- typescript, jsx와 같은 브라우저에서 직접적으로 이해할 수 없는 언어를 JavaScript로 변환하는 기능(`preset-typescript`, `preset-react`)
- `preset-env`같이 필수적인 플러그인들을 모아둔 preset을 사용하여 쉽게 최신 자바스크립트를 사용할 수 있도록 지원


### Polyfill
새롭게 추가된 전역 객체(Map, Set, Promise 등)이나 내장 메소드(Array.prototype.includes() 등)를 지원하지 않는 브라우저에서도 동작할 수 있도록 Polyfill을 추가해야 한다.
- `corejs` : 런타임 버전의 폴리필 (사용하지 않는 폴리필까지 다 추가하는 babel-polyfill과 달리 폴리필을 추가하기 전에 해당 기능이 있는지 확인)
- `regenerator-runtime`: Generator function 폴리필


### ES Module
module이란 한 파일에서 선언된 기능을 다른 파일에서 접근하여 사용할 수 있도록 하는 것을 의미한다. ES6에 ES Module이 도입되기 전에는 <script> 태그를 사용하여 여러 자바스크립트 파일을 로드했는데
  파일 간의 의존성 관리를 직접해야한다는 불편함이 있었고, 전역 공간에서 같은 이름의 메서드가 오버라이딩 될 수 있다는 문제점이 있었다.
  
## Webpack
프로젝트에 포함된 여러 모듈간의 의존성을 파악하여 내부적으로 하나 이상의 번들로 만들어주는 **Module Bundler**이다.

### Loader
Wepack은 기본적으로 JavaScript와 JSON 파일만을 이해할 수 있다. Loader는 Webpack이 image, jsx, css, sass와 같은 파일을 처리할 수 있도록 변환하는 작업을 한다.
- 예시: `babel-loader`, `css-loader` 등

### Plugin
환경 변수 주입이나 번들 최적화 등의 처리를 할 수 있게 해준다. 주로 번들링된 결과에 대한 후속 처리 담당.
- `html-webpack-plugin`: generates an HTML file for your application by injecting automatically all your generated bundles.
