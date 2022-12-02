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
  "rules": {},
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
  "files.autoSave": "afterDelay",   // 이 두 라인은
  "files.autoSaveDelay": 2000,      // 취향
  "eslint.enable": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "eslint.alwaysShowStatus": true
}
```
여기까지 하면 환경 설정이 거의 다 된것입니다. 해당 내용은 변경내용 저장시 자동으로 포맷터가 작동하는것이므로, 아마 많은 분들이 하고 계시는 중일텐데요, 해당 내용중 빠진 내용이 있으면 적어주시면 될것 같습니다. 
환경 설정에서 디폴트 포맷터가 프리티어로 되어 있는지도 확인해주세요.

## eslint 한번 더 자동화 하기.