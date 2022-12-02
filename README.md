# eslint

## 시작하기 전에

분명 우테코 주간 피드백에 eslint 를 사용해보라는 말이 있었던 것을 알고 있었는데, 어째서인지 프리티어면 되겠지 생각했었습니다. 프리코스가 끝나니까 '아 그게 있었지' 싶어서 이번에 접근을 해 보았습니다.

> ## VSCode 확장기능 VS npm install

처음 eslint 를 적용하려 했을때 익스텐션에 이미 eslint 를 적용시킬 수 있다는 걸 알고 있었습니다. 문득 확장기능에서의 eslint 와 노드 패키지 매니저를 통한 eslint 설치에는 어떤 차이가 있는걸까 궁금해 졌습니다.많은 정보 글에서는 직접 npm을 사용하여 설치를 하거나 두개 다 설치 하거든요.

## 일단 확장기능 부터 설치 해 보았습니다.

다음과 같은 코드가 있는데요,

```javascript
const forNoUse = "사용하지 않는 변수";

console.log("foo");;;;;;;
console.log("bar")
```

확장기능 설치 만으로는 eslint 가 작동하는것 처럼 보이지 않습니다.  
기대처럼 작동한다면 중복되는 세미콜론과 사용하지 않는 변수 그리고 세미콜론이 없는것 등을 잡아 주어야 하거든요.

## 이번에는 Eslint 설치

터미널에   
`npm install --save-dev eslint`  
입력해 주고 나면

```javascript
package.json >

"devDependencies": {
    "eslint": "^8.28.0",
  },
```

package.json 파일에 디팬던시가 추가됩니다. 역시 이상태에서도 아무런 동작을 안하는것 같습니다.

![exampleNo1](https://user-images.githubusercontent.com/105046423/204788385-f5e67734-5ac2-4ac6-ad12-ba027b2cefaa.png)  
(아무런 변화가 없음)

## eslint 설정을 적용해야 한다.

eslint 를 프로젝트에 적용 시키기 위해서 설정을 해 주어야 합니다.

`npm init @eslint/config`

입력을 하면 아래와 같이 터미널에서 여러가지 환경에 대한 선택을 하게 됩니다.

![exampleNo3](https://user-images.githubusercontent.com/105046423/204788389-78a0b187-64d6-49ad-a6ef-dc416681c61f.png)

몇개의 선택지가 있으니 상황에 맞게 선택하시면 될것같습니다.

## 위에서 물어본 질문들을 전부 대답해 주고나면

![errorExampleNo1](https://user-images.githubusercontent.com/105046423/204788439-89775b07-cdf4-4d3f-8612-602b4f29a6d7.png)
작은 따옴표를 사용해야 하고, 새 라인이 필요하고, 세미콜론이 없고  
오우....  

![errorExampleNo2](https://user-images.githubusercontent.com/105046423/204788444-7d565fc3-47ae-4e6d-bb84-e0517e14f47c.png)  
세미콜론이 많다고 불평하네요.

> ## 그럼 VScode 익스텐션은 기능이 뭐야??

여기까지 하니 vscode 의 익스텐션의 기능이 무엇인지 궁금해 지는데요,
익스텐션 기능을 disable 해보겠습니다.

![eslintDisable](https://user-images.githubusercontent.com/105046423/204788448-e8b4ce10-9e67-4e87-b583-af2606c732e0.png)

익스텐션을 disable 해보니, 오류가 사라졌습니다. 그렇다면 맨 처음에는 왜 오류를 잡아내지 못한 걸까요??

## VScode 익스텐션은 편의기능이다.

사실 eslint 의 기능은 명령어를 통해서 작동합니다.

```javascript
"scripts": {
    "test": "cypress run --browser chrome",
    "lint" : "eslint ./"
  },
```

와 같이 스크립트를 추가 해주고

터미널에   
`npm run lint`  
를 입력시 eslint 가 영역내 파일을 모두 검사하여 오류를 보내주게 되어있습니다.  
하지만 매번 파일을 작성 할 때마다 우리가 터미널에 명령어를 입력해야 한다는 것은 귀찮은 일이겠죠.

VScode 의 eslint 익스텐션기능은 명령어 없이 실시간으로 텍스트 에디터에 오류 여부를 표시해 주는 기능이었기 때문에 eslint 의 설정 파일없이는 아무런 일도 하지 않았던 것이었죠.


## 이번편은 기본편
이정도 까지의 과정으로 기본적인 eslint 를 적용하는데 어려움이 없을것이라 생각합니다. 제가 조금 더 공부를 해서 lint 를 조금 더 커스텀 할 수 있는 방법을 작성 해보도록 하겠습니다. 

[eslint설정하기](https://github.com/powercording/eslint/tree/main/src)