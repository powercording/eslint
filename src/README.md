# ESlint 설정하기

해당 글을 읽으시기 전에 ESlint를 처음 적용하시려는 분은 아래 링크로 이동해 주세요.  
[ESlint 처음 만나는 분들은 여기로.](https://github.com/powercording/eslint)

## 우선 .eslintignore 파일 만들기

이그노어 대상이 없다면 린트는 우리의 노드모듈까지 검사할 수도 있습니다. 시간과 에러가 엄청나오겠죠. 루트 디렉토리(최상단위치) 에
` .eslintignore` 를 만들어 주고 .gitignore 처럼 아래와같이 내용을 입력합니다.
`node_modules`

또는 .eslintignore 파일 없이 .eslintrc.json (또는 js) 에서 아래와 같이 설정 할 수 있습니다.

```JSON
{
  "env": {
    "browser": true,
    "commonjs": true,
    "es2021": true,
    "node": true
  },
  "ignorePatterns" : ["node_modules/"]
}
```

이로써 우리의 린터가 불필요한 파일에 접근하는 것을 막을수 있게 되었습니다. 불필요한 파일은 그때그때 추가해 주시면 됩니다.

## Prettier 와 ESlint 를 함께 사용하기.

eslint와 prettier 를 같이 사용하도록 합시다. 그렇게 하기 위해서는 우선
prettier 가 있어야 합니다.

```npm
npm i --save-dev prettier
```

를 입력하여 프리티어를 설치 해 줍니다.

> 익스텐션 Prettier 와 다른가요?

ESlint 의 규칙을 프리티어에 적용하기 위해서는 ESlint 가 Prettier 를 찾을 수 있어야 합니다. 익스텐션으로써의 prettier 는 에티터의 기능이므로 프로젝트에 적용 되는 프리티어를 설치해 줍니다.  
프리티어 설치를 끝냈다면 최상단 위치에서 프리티어의 환경설정을 해줍니다.

```javascript
.prettierrc.js

module.exports = {
  singleQuote: true,   // 작은 따옴표
  semi: true,          // 코드의 끝에 세미콜론 붙임.
  useTab: false,       // 들여쓰기를 스페이스로
  tabWidth: 2,         // 들여쓰기 간격
  trailingComma: 'all',// 객체 등에서 벨류뒤에 콤마 붙이기
  printWidth: 80,      // 한 줄의 글자 갯수제한
  arrowParens: 'avoid',// 애로우 펑션의 파라미터가 한개라면 괄호 생략
};
```

여기까지는 프리티어를 프로젝트에 적용하는 부분이었습니다.  
이제 다음으로는 프리티어와 eslint 가 같이 작동하도록 만들어야 하는데요, 그러기 위해서는 eslint 의 일부 기능을 프리티어에게 맡기고, 프리티어는 eslint 의 룰을 알아야 할 필요가 있습니다.

```npm
npm i --save-dev eslint-config-prettier
npm i --save-dev eslint-pulgin-prettier
```

해당 명령어 이후 `package.json` 에서 내용을 확인 해 줍니다.

```json
 "devDependencies": {
    "eslint": "^8.28.0",
    "eslint-config-airbnb-base": "^15.0.0",
    "eslint-config-prettier": "^8.5.0",
    "eslint-plugin-import": "^2.26.0",
    "eslint-plugin-prettier": "^4.2.1",
 }
```

이제 해당 플러그인을 eslint 설정 파일에 알려주어야 합니다.

```json
{
  "env": {
    "browser": true,
    "commonjs": true,
    "es2021": true,
    "node": true
  },
  "extends": ["airbnb-base", "plugin:prettier/recommended"],
  "overrides": [],
  "parserOptions": {
    "ecmaVersion": "latest"
  },
  "rules": {}
}
```

여기까지 잘 따라 오셨다면 한가지만 더 작성하러 갑시다.
vscode 의 setting.json 파일입니다.

```json
{
  "editor.formatOnSave": true,
  "[javascript]": {
    "editor.formatOnSave": false
  },
  "files.autoSave": "afterDelay", // 이 두 라인은
  "files.autoSaveDelay": 2000, // 취향
  "eslint.enable": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "eslint.alwaysShowStatus": true
}
```

여기까지 하면 환경 설정이 거의 다 된것입니다. 해당 내용은 변경내용 저장시 자동으로 포맷터가 작동하는것이므로, 아마 많은 분들이 하고 계시는 중일텐데요, 위 내용에서 자신에게 없는 내용이 있으면 적어주시면 될것 같습니다.
환경 설정에서 디폴트 포맷터가 프리티어로 되어 있는지도 확인해주세요.

## eslint 한번 더 자동화 하기.

이제 우리는 실시간으로 코드의 오류를 잡아내고, 저장 할 때마다 프리티어가 eslint 내용대로 포멧팅을 해줄 것이라 기대할 수 있습니다. 그렇지만 만약에 우리가 오류가 있는 파일을 저장하기 전에 커밋을 하는 실수를 저지를 수도 있습니다. 만약에라도 말이죠.그러한 상황을 막기 위해서
커밋 이전에 지정한 명령어가 실행 되도록 셋팅 할 수 있습니다.

> 여기서 부터는 부가적인 영역이므로 윗 내용에만 만족하셔도 좋습니다.

~~## husky 와 lint-staged 사용하기~~

~~제가 eslint 를 적용시키는 연습을 하며 가장 많이 마주쳤던 조합입니다.~~

- ~~husky 를 사용하여 커밋 이전에 원하는 명령어를 동작 시킬 수 있습니다.~~
- ~~lint-staged 는 스테이지된 파일만 검사하게 합니다. eslint는 모든 파일을 검사하므로 시간을 조금이라도 절약 할 수 있겠죠.~~

```npm
npx mrm@2 lint-staged
```

해당 명령어 이후에 package.json 은 이렇게 됩니다.

```json
 "devDependencies": {
    "eslint": "^8.28.0",
    "eslint-config-airbnb-base": "^15.0.0",
    "eslint-config-prettier": "^8.5.0",
    "eslint-plugin-import": "^2.26.0",
    "eslint-plugin-prettier": "^4.2.1",
    "husky": "^8.0.2",
    "lint-staged": "^13.0.4",
    "prettier": "^2.8.0"
  },
  "dependencies": {
    "save-dev": "0.0.1-security"
  },
  "lint-staged": {
    "*.{js,jsx}": "prettier --write",
    "*.js": "eslint"
  }
```

~~`line-staged` 의 검사 파일 확장자를 상황에 맞게 변경하시고, 벨류에는 커밋 이전에 실행했으면 하는 명령어를 입력 할 수 있습니다.~~

~~이제 모든 커밋에 대하여 커밋이 실행되기 전에~~
~~`npx eslint .` , `npx prettier --write` 를 하는것과 같습니다. 만약 오류가 발생 한다면 커밋이 진행되지 않습니다.~~

## ~~발생 할수 있는 오류 잡기~~

- ~~커밋시의 eslint 가 중첩되서 실행되는 현상이 있을 수 있습니다. husky 폴더의 pre-commit 파일에 해당 내용을 추가합니다.~~

```
#!/bin/sh
. "$(dirname "$0")/_/husky.sh"

exec >/dev/tty 2>&1      <<<< 요 라인을 추가해 주세요.

npx lint-staged
```

~~이렇게 하면 다중실행 문제는 잡히는데, 발생한 원인이나 오류를 해결하는 원리는 아직 모르겠습니다.~~

- Delete'CR' 이라는 오류를 내뿜을 수 있습니다. `.eslintrc.json` 의 `rules`에 해당 내용을 추가합니다.

```json
 "rules": {
    "prettier/prettier": ["error", { "endOfLine": "auto" }]
  },
```

## 2022-12-07 추가

위의 오류를 위처럼 하지 않고 이렇게 해도 된다고 한다.

```javascript
module.exports = {
  singleQuote: true,
  semi: true,
  useTab: false,
  tabWidth: 2,
  trailingComma: 'all',
  printWidth: 80,
  arrowParens: 'avoid',
  endOfLine: 'auto', // <<< 이 라인 추가.
};
```

## 2022-12-12 추가

파일 임포트나 익스포트오류 또는 클래스 안에서 메소드 작성시
this 가 없을시 오류로 인식하는 등의 문제 해결을 위하여

```json
.eslintrc.json

"parser": "@babel/eslint-parser"
```

를 추가 해준다.

이렇게 하면 lint-staged 가 노드 모듈에서 다시 오류를 일으키는 문제와 더불어 husky 와 lint-staged 가 보이지 않는 오류로 인해서
커밋을 방해하는 경우가 잦기 때문에 만약 husky 와 lint-staged 를 완전하게 다룰 수 해당 기능을 삭제하는것을 추천합니다.

```
npm uninstall husky
npm uninstall lint-staged
```
