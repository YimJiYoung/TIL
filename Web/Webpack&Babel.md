## Babel
ES6 이상의 최신 문법을 지원하지 않는 브라우저에 대해서 동작할 수 있도록 이전 문법으로 변환해주는 **Transpiler**
- typescript, jsx와 같은 브라우저에서 직접적으로 이해할 수 없는 언어를 JavaScript로 변환하는 기능(`preset-typescript`, `preset-react`)
- `preset-env`같이 필수적인 플러그인들을 모아둔 preset을 사용하여 쉽게 최신 자바스크립트를 사용할 수 있도록 지원


### Polyfill
새롭게 추가된 전역 객체(Map, Set, Promise 등)이나 내장 메소드(Array.prototype.includes() 등)를 지원하지 않는 브라우저에서도 동작할 수 있도록 Polyfill을 추가해야 한다.
- `corejs` : 런타임 버전의 폴리필 (사용하지 않는 폴리필까지 다 추가하는 babel-polyfill과 달리 폴리필을 추가하기 전에 해당 기능이 있는지 확인)
- `regenerator-runtime`: Generator function 폴리필
