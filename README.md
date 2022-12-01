# eslint

## 시작하기 전에

분명 우테코 주간 피드백에 eslint 를 사용해보라는 말이 있었던 것을 알고 있었는데, 어째서인지 프리티어면 되겠지 생각했었습니다. 프리코스가 끝나니까 '아 그게 있었지' 싶어서 이번에 접근을 해 보았습니다.

> ## VSCode 확장기능 VS npm install

처음 eslint 를 적용하려 했을때 익스텐션에 이미 eslint 를 적용시킬 수 있다는 걸 알고 있었습니다. 문득 확장기능에서의 eslint 와 노드 패키지 매니저를 통한 eslint 설치에는 어떤 차이가 있는걸까 궁금해 졌습니다.많은 정보 글에서는 직접 npm을 사용하여 설치를 하거나 두개 다 설치 하거든요.

> ## 일단 확장기능 부터 설치 해 보았습니다.

---

다음과 같은 코드가 있는데요,

```javascript
const forNoUse = "사용하지 않는 변ㅅ";

console.log("foo");
console.log("bar");
```

확장기능 설치 만으로는 eslint 가 작동하는것 처럼 보이지 않습니다.  
기대처럼 작동한다면 중복되는 세미콜론과 사용하지 않는 변수 그리고 세미콜론이 없는것 등을 잡아 주어야 하거든요.

> ## 이번에는 npm install --save-dev eslint 설치

---

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

---

> ## eslint 설정을 적용해야 한다. npm init @eslint/config

![exampleNo3](https://user-images.githubusercontent.com/105046423/204788389-78a0b187-64d6-49ad-a6ef-dc416681c61f.png)
eslint 의 사용 목적을 묻고 있습니다.

![exampleNo4](https://user-images.githubusercontent.com/105046423/204788391-3831a8d9-ce2f-4060-b6f0-7f1dd4ba9dd9.png)
CommonJS 와 Javascript 모듈중 어느것을 사용할지 묻고 있습니다.
저는 CommonJS 를 선택 하였습니다.

![exampleNo5](https://user-images.githubusercontent.com/105046423/204788394-7d7024d9-9298-465c-87a2-b4dbfbb8513c.png)
프레임워크가 있나요?

![exampleNo6](https://user-images.githubusercontent.com/105046423/204788398-10c04ae6-a965-47a4-90f4-ea6d0ab9910a.png)
타입스크립트를 사용중이신가요?

![exampleNo7](https://user-images.githubusercontent.com/105046423/204788399-2521c0d7-1424-4043-bfeb-a7b90f5517ab.png)
코드는 어디에서 작동합니까(브라우저/노드) 저는 둘다 선택. (space or A key)

![exampleNo8](https://user-images.githubusercontent.com/105046423/204788401-3790cf36-8ac9-4094-ac45-cbf13484acf2.png)
대중적인 스타일 가이드 사용을 하겠습니다.

![exampleNo9](https://user-images.githubusercontent.com/105046423/204788404-5cfdde4e-63e4-4b69-9b93-22b4fb9cc874.png)
에어비엔비를 고르고

![exampleNo10](https://user-images.githubusercontent.com/105046423/204788409-69df07a7-82c4-4ed7-8b78-cdb70b39585a.png)
congif 파일을 어떠한 형식으로 작성할지 묻습니다. JSON 이나 자바스크립트 선택.

![exampleNo11](https://user-images.githubusercontent.com/105046423/204788412-52674f69-3549-440e-8934-bb13829a1a59.png)
설정을 적용하기 위해서 받아야할것들이 있네요.

![exampleNo12](https://user-images.githubusercontent.com/105046423/204788417-2d376890-2f4a-4abe-8705-a3523a41e428.png)
어떤 매니저를 선택해서 파일을 받을지 물어봅니다.

---

> ## 위 과정을 전부 다 거치고 나니!

![errorExampleNo1](https://user-images.githubusercontent.com/105046423/204788439-89775b07-cdf4-4d3f-8612-602b4f29a6d7.png)
작은 따옴표를 사용해야 하고, 새 라인이 필요하고, 세미콜론이 없고  
오우....

![errorExampleNo2](https://user-images.githubusercontent.com/105046423/204788444-7d565fc3-47ae-4e6d-bb84-e0517e14f47c.png)  
세미콜론이 많다고 불평하네요.

---

> ## 그럼 VScode 익스텐션은 기능이 뭐야??

여기까지 하니 vscode 의 익스텐션의 기능이 무엇인지 궁금해 지는데요,
익스텐션 기능을 disable 해보겠습니다.

![eslintDisable](https://user-images.githubusercontent.com/105046423/204788448-e8b4ce10-9e67-4e87-b583-af2606c732e0.png)

익스텐션을 disable 해보니, 오류가 사라졌습니다. 그렇다면 맨 처음에는 왜 오류를 잡아내지 못한 걸까요??

---

> ## VScode 익스텐션은 편의기능이다.

사실 eslint 의 코드 기능은 명령어를 통해서 작동합니다.

```javascript
"scripts": {
    "test": "cypress run --browser chrome",
    "lint" : "eslint ./"
  },
```

와 같이 스크립트를 추가 해주고

`npm run lint`

를 입력시 eslint 가 영역내 파일을 모두 검사하여 오류를 보내주게 되어있습니다. 