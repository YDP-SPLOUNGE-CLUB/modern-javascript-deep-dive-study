# 49.1 Babel

Babel은 ES6+/ES.NEXT로 구현된 최신 사양의 소스코드를 IE 같은 구형 브라우저에서도 동작하는 ES5 사양의 소스코드로 변환(트랜스파일링)할 수 있다.

```bash
# package.json 설정
npm init -y
# babel-core, babel-cli 설치
npm install --save-dev @babel/core @babel/cli
```

Babel을 사용하려면 @babel/preset-env 를 설치해야 한다.
@babel/preset-env는 함께 사용되어야 하는 Babel 플러그인을 모아 둔 것으로 Babel 프리셋이라고 부른다.

Babel이 제공하는 공식 Babel 프리셋은 다음과 같다.
- @babel/preset-env
- @babel/preset-flow
- @babel/preset-react
- @babel/preset-typescript

@babel/preset-env는 필요한 플로그인들을 프로젝트 지원 환경에 동적으로 결정해 준다.
프로젝트 지원 환경은 Browserslist 형식으로 .browserslistrc 파일에 상세히 설정할 수 있다.
만약 프로젝트 지원환경 설정 작업을 생략하면 기본값으로 설정된다.

@babel/preset-env가 설치가 완료되면 루트 폴더에 babel.config.json 설정 파일을 생성하고 다음과 같이 작성한다.
지금 설치한 @babel/preset-env를 사용하겠다는 의미이다.

```json
{
	"presets": ["@babel/preset-env"]
}
```

Babel을 사용하여 ES6+/ES.NEXT 사양의 소스코드를 ES5 사양의 소스코드로 트랜스파일링해보자.
Babel CLI 명령어를 사용할 수도 있으나 트랜스 파일링할 때마다 매번 Babel CLI 명령어를 입력하는 것은 번거로우므로 npm scripts에 Babel CLI 명령어를 등록하여 사용하자.

package.json 파일에 scripts를 추가한다.

```json
{
	"scripts": {
		"build": "babel src/js -w -d dist/js"
	}
}
```

위 npm scripts의 build는 src/js 폴더(타깃 폴더)에 있는 모든 자바스크립트 파일들을 트랜스파일링한 후, 그 결과물을 dist/js 폴더에 저장한다. 사용한 옵션의 의미는 다음과 같다.

- -w
    - 타깃 폴더에 있는 모든 자바스크립트 파일들의 변경을 감지하여 자동으로 트랜스파일한다.(--watch 옵션의 축약형)
- -d
    - 트랜스파일링된 결과물이 저장될 폴더를 지정한다. 만약 지정된 폴더가 존재하지 않으면 자동 생성한다. (--out-dir 옵션의 축약형)

이제 트랜스파일링을 테스트하기 위해 ES6 사양의 코드를 작성한후 터미널에서 다음과 같이 명령어를 입력하여 트랜스파일링을 실행한다.

```bash
npm run build
```

private 필드 정의 제안에서 에러가 발생할 수 있다.
발생한다면 현재 제안 단계에 있는 사양에 대한 플로그인을 지원하지 않기 때문인데, 별도의 플로그인을 설치하여 해결할 수 있다.

```bash
npm install --save-dev @babel/plugin-proposal-class-properties
```

이 플로그인은 public/private 클래스 필드를 지원한다.

설치 후 babel.config.json에 다음과 같이 추가한다.

```json
{
	"presets": ["@babel/preset-env"],
	"plugins": ["@babel/plugin-proposal-calss-properties"]
}
```

다시 터미널에서 다음과 같이 명령어를 입력하여 트랜스파일링을 실행한다.

트랜스파일링에 성공하면 프로젝트 루트 폴더에 dist/js 폴더가 자동적으로 생성되고 트랜스파일링된 main.js lib.js 가 저장된다.

브라우저에서 CommonJS 방식의 require 함수를 지원하지 않으므로 위에서 트랜스파일링된 결과를 그대로 브라우저에서 실행하면 에러가 발생한다.
프로젝트 루트 폴더에 다음과 같이 index.html을 작성하여 트랜스파일링된 자바스크립트 파일을 브라우저에서 실행해야 한다.

```html
<body>
	<script src="dist/js/lib.js"></script>
	<script src="dist/js/main.js"></script>
</body>
```

브라우저의 ES6 모듈(ESM)을 사용하도록 Babel을 설정할 수도 있으나 앞서 설명한 바와 같이 ESM을 사용하는 것은 문제가 있다.

Webpack을 통해 이러한 문제를 해결해보자.

# 49.2 Webpack

Webpack은 의존 관계에 있는 자바스크립트, CSS, 이미지 등의 리소스들을 하나(또는 여러 개)의 파일로 번들링하는 모듈 번들러다.

Webpack을 사용하면 의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈 로더가 필요 없다.
그리고 여러 개의 자바스크립트 파일을 하나로 번들링하므로 HTML 파일에서 script 태그로 여러 개의 자바스크립트 파일을 로드해야 하는 번거로움도 사라진다.

Webpack 은 다음과 같이 설치한다.

```bash
npm install --save-dev webpack webpack-cli
```

Webpack 이 모듈을 번들링할 때 Babel을 사용하여 ES6+/ES.NEXT 사양의 소스코드를 ES5 사양의 소스코드로 트랜스파일링하도록 babel-loader를 설치한다.

```bash
npm install --save-dev babel-loader
```

npm scripts를 변경하여 Babel 대신 Webpack을 실행하도록 수정하자.

```json
{
	"scripts": {
		"build": "webpack -w"
	}
}
```

webpack.config.js 는 Webpack이 실행될 때 참조하는 설정 파일이다.
루트 폴더에 webpack.config.js 파일을 생성하고 다음과 같이 작성한다.

```js
const path = require('path');
moudel.exports = {
	entry: './src/js/main.js',
	output: {
		path: path.resolve(__dirname, 'dist'),
		filename: 'js/bundel.js'
	},
	module: {
		rules: [
			{
				test: /\.js$/,
				include: [
					path.resolve(__dirname, 'src/js')
				],
				exclude: /node_moudels/,
				use: {
					loader: 'babel-loader',
					options: {
						presets: ['@babel/preset-env'],
						plugins: ['@babel/plugin-proposal-class-properties']
					}
				}
			}
		]
	},
	devtool: 'source-map',
	mode: 'development'
}
```

이제 Webpack을 실행하여 트랜스 파일링 및 번들링을 실행해보자.

실행된 결과 dist/js 폴더에 bundle.js 가 생성되었다.
이 파일을 index.html을 다음과 같이 수정하고 브라우저에서 실행해보자.

```html
<body>
	<script src="dist/js/bundle.js"></script>
</body>
```


Babel을 사용하여 ES6+/ES.NEXT 사양의 소스 코드를 ES5 사양의 소스코드로 트랜스파일링해도 브라우저가 지원하지 않는 코드가 남아 있을 수 있다.

예를들어 Promise, Object.assign, Array.from 는 ES5 사양에 대체할 기능이 없기 때문에 트랜스파일링 되지 못하고 그대로 남는다.

이런 메서드를 사용하기 위해서는 babel-polyfill 을 설치해 해결할 수 있따.

```bash
npm install @babel/polyfill
```

@babel/polyfill 은 개발 환경에서만 사용하는 것이 아니라 실제 운영환경에서도 사용해야 한다.
따라서 개발 의존성으로 설치하는 --save-dev 옵션을 지정하지 않는다.

ES6 import를 사용하는 경우에는 진입점의 선두에서 먼저 폴리필을 로드하도록 한다.

```js
import "@babel/polyfill"
import { pi, power, Foo } from './lib';
```

Webpack을 사용하는 경우에는 위 방법 대신 webpack.config.js 파일의 entry 배열에 폴리필을 추가한다.

```js
const path = require('path');
moudel.export = {
	entry: ['@babel/polyfill', './src/js/main.js']
}
```

이후 스크립트를 실행한다.

```bash
npm run build
```

# 알고 가기
---
- [ ] Babel, Webpack 에 대해 간략히 설명해주세요.
