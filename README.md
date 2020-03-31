# React
React Study and Job
> 해당 글은 Learning React [Book] - O'Reilly ver.Eng 보고 작성한 것 입니다.

## React 이론
1. Hello React
- 리액트는 선언형(Declaration) 컨셉으로 개발되었기 때문에, 진행순서에 초점을 맞춘 명령형 타입과 다르게, *결과 값*에 초점을 맞춰져 있다.
그래서 개발자가 View를 Controll 할 필요가 없고, ReactDOM의 State가 변경되면 자동으로 React가 리렌더링(Re-rendering) 한다.
2. Component Class and Function(+Hooks)
3. Props(=Properties)
4. State
5. Syntax JSX
6. Component Lifecycle Event
* 라이프사이클 이벤트란?(Lifecycle Event)
- 컴포넌트를 좀 더 세밀하게 제어하기 위한 방법으로 컴포넌트 인스턴스 생성 전에 필요한 로직을 구현한 후 새로운 속성을 제공해서 인스턴스를 재생성하는 방법을
생각해볼 수 있지만, 이 방법으로는 *독립적인 컴포넌트*를 생성할 수 없다.
- 가장 좋은 방법은 컴포넌트 라이프사이클 이벤트(Lifecycle Event)를 사용하는 것이다.
- 마운팅(Mounting) 이벤트를 이용해서 컴포넌트에 필요한 로직을 주입(Injection)할 수 있다. 그외에도 다른 이벤트를 이용한다면, 뷰(View)가 렌더링하는 것을
결정하는 특별한 로직을 제공해서 좀 더 똑똑한 컴포넌트를 만들 수 있다.
* 이벤트 종류
- 마운트 이벤트(Mounting Event) : React Element를 DOM 노드에 추가할 때 발생하는 이벤트.
- 갱신 이벤트(Updaing Event) : 속성이나 상태가 변경되어 React 엘리먼트를 갱신할 때 발생하는 이벤트.
- 언마운팅 이벤트(Unmounting Event) : React 엘리먼트를 DOM에서 제거할 때 발생하는 이벤트
* Class Type Lifecycle event 실행 순서
- constructor() : 엘리먼트를 생성하여 기본속성(Properties)와 상태(State)를 설정할 때 실행된다.<br>
*[Mount]*<br>
- componentWillMount() : DOM에 삽입하기 전에 실행된다.<br>
- componentDidMount() : DOM에 삽입되어 렌더링이 완료된 후 실행된다.<br>
- 실행 단계 : constructor --> componentWillMount() --> render() --> componentDidMount()<br>
*[Update]*<br>
- componetWillReceiveProps(nextProps) : 컴포넌트가 속성을 받기 직전에 실행된다.<br>
- shouldComponentUpdate(nextProps, nextState) : 컴포넌트가 갱신되는 조건을 정의해서 재렌더링을 최적화할 수 있다. Bool 값을 반환한다.<br>
- componetWillUpdate(nextProps, nextState) : 컴포넌트가 갱신되기 직전에 실행된다.<br>
- 컴포넌트 속성 갱신 : componentWillReceiveProps() --> shouldComponentUpdate() --> componentWillUpdate() --> render() --> componentDidUpdate()
- 컴포넌트 상태 갱신 : shouldComponentUpdate() --> componentWillUpdate() --> render() --> componentDidUpdate()
*[Unmount]*<br>
- componentWillUnmount() : 컴포넌트를 DOM에서 제거하기 전에 실행되며, 구독한 이벤트를 제거하거나 다른 정리 작업을 수행한다.<br>
- 실행 순서 : componentWillUnmount();
![스크린샷 2020-03-23 오후 6 17 10](https://user-images.githubusercontent.com/58014147/77434073-a470fb80-6e23-11ea-920e-6cb116c64653.png)

* Functional Type Lifecycle
- Hooks을 사용하여 단순 Display용으로 사용하던 함수형 컴포넌트에서 'State'와 'LifeCycle', 'Reference'등의 클래스형 Component의 기능의 구현한 개념.
- 또한 Memoization등의 기능 또한 포함되어 클래스형보다 더 *직관적*이라는 장점을 가지고 있다.
- const \[_stateVariable, _setStateFunction]\ = useState(...initStateValue)<br>
: 함수형 컴포넌트에서 React의 State의 값을 사용할 수 있게 해주는 Hooks의 핵심 인터페이스이며, useState()의 첫번째 인자로 State의 초기값을 설정 할 수 있다.
```javascript
//#Set StateVariable
const [count, setCount] = useState(0);
const [fruit, setFruit] = useState('banana');
const [todos, setTodos] = useState({text : 'Learn Hooks'});
```
- useEffect(_callback, _arrayStateVariable) <br>
: Hooks을 통해 함수형 컴포넌트는 Lifecycle을 구현할 수 있다. 클래스형 컴포넌트와 다르게, Hooks에서는 useEffect로 Lifecycle을 관리 할 수 있다. 기본적으로 *render()* 가 실행되면 *useEffect* 가 함께 실행된다고 보면 된다. 그리고 Return되는 callBack을 통해 *componentWillUnmount()* 를 구현할 수 있다.<br>실제로는 모든 render요청에 대해, useEffect가 실행되니 사실 이 method의 퍼포먼스는 중요하다. 이를 위해 *useEffect*의 두번째 인자로 변경 사항이 있는 값들을 배열로 할당하여 해당 값들이 변경될 때만 Effect를 실행하게 할 수 있다.
```javascript
function HksLifecycle() {
    const [count, setCount] = useState(0);

    // useEffect, First Parameter is CallBack function
    useEffect(() => {
            // 컴포넌트가 마운트 되고, setTimeout 함수를 호출한다.
            setTimeout(() => {
                document.title = `You Clicked ${count} times`;
            }, 3000);
            return () => window.removeEventListener('scroll', onScroll); // <--- Return callBack을 통해서 componetWillUnmount구현
        },
        [count]); // <-- Second Parameter is 'StateVariable'
                        //  이렇게 하면 useEffect는 count state를 지켜보다가 count가 갱신되면 첫번째 callBack를 실행하게 된다.
                        //  변경 사항이 있는 값들(State) 배열로 보내어, 해당 값들이 변경될 때만 useEffect를 실행하게 만들면 된다.
        //[]); //<--- Second Parameter is 'Empty Array'
                  // useEffect 함수를 마운트되고 한번만 실행하게 하려면 두번째 인자로 빈 배열을 넣어주면 된다.
                  // 그렇지 않으면 state 값이 업데이트 될 경우 다시 한번 Rendering 해준다.

    return(
        <div>
            <p>You Clicked {count} times</p>
            <button onClick={() => setCount(count+1)}>
                Click Me
            </button>
        </div>
    )
};

export default Lifecycle;
```
- useRef(ClassType: React.createRef())
what is 'ref'?
- 리액트 프로젝트 내부에서 DOM에 이름을 다는 방법을 Reference(ref)라고 한다.
why used 'ref'?
- *DOM을 꼭 직접적으로 건드려야 할 때* 사용한다.

 함수형 컴포넌트 내에서 컴포넌트 Reference를 참조 할 수 있다. 또한 단순한 DOM이나 컴포넌트를 참조하는 것이 아니라, 클래스 컴포넌트의 인스턴스처럼 사용 할 수 있다.하지만 초기에만 실행 시켜줄 수 있는 콜백형식으로 전달하지 못하기 때문에 아래와 같이 setInterval에 레퍼런스를 할당하고자 할때에는 useEffect등을 이용해서 별도로 설설정해야 한다.<br>
 컴포넌트를 작성하다 보면 태그(Tag)를 직접 다루어야 하는 경우가 존재한다. *document.getElementById* 메서드로 직접 DOM의 Element를 불러와서 사용할 수 있다. 또는 스크롤바의 위치를 가져와야 하는 경우, 또는 width/height 값을 가져와야하는 경우 사용하면 된다.
```javascript
import React, {useState, useRef} from 'react';

function InputSample(){
    const [inputs, setInputs] = useState({
        name : '',
        nickname : ''
        });
    const {name, nicknamne} = inputs;
    const nameInputBox = useRef();
    
    const onChange = (event) => {
        const {name, value} = event.target;
        setInputs({
            ...inputs,
            [name] : value
        });
        // 이렇게 ReactDOM에 선언한 ref객체(Element)에 직접적으로 접근할 수 있다.
        nameInputBox.current.focus();
    };
    
    return(
        <div>
            <input
                name='name'
                value={name} // State의 Variable인 inputs의 name 값으로 할당.
                ref={nameInputBox} // useRef()으로 생성한 ref객체를 할당.
                onChange={onChange} // onChange 이벤트 구독 및 Handler 할당.
            />
        </div>
    )

}
```

- useContext
- useMemo & useCallback
*useMemo*,*useCallback*은 memoize를 함수형 컴포넌트에서 사용할 수 있도록 만들어진 기능이다. 둘의 사용처는 비슷한데 차이점은 *useMemo*는 [계산된 값]을 가지고 있고 *useCallback*은 콜백으로 실행 할 수 있다는 차이를 가지고 있다. 두 API 다 첫번째 인자로 값을 리턴하는 콜백을 가져오고, 두번째 인자로 변경 여부를 평가하는 값들의 배열을 가져온다.




* 마운팅 이벤트(Mounting Event)
- 마운팅 이벤트 유형은 모두 실제 DOM에 컴포넌트를 추가하는 것에 대한 이벤트다.
- componentWillMount()
컴포넌트 라이프사이클에서 componentWillMount()는 *단 한번만* 실행된다. 실행 시점은 초기 렌더링 직전이다.ReactDOM.render()를 호출해서 React 엘리먼트를 브라우저에 렌더링하는 시점에서 componentWillMount()가 실행된다. 

* 컴포넌트 반복
- 자바스크립트 배열객체의 내장함수 *mapm()* 함수를 사용하여 반복되는 컴포넌트를 렌더링 할 수 있다.
@arr.map(callback, \[thisArg\]);
callback => *새로운 배열의 요소*를 *생성하는 함수*로 파라미터는 3가지를 갖는다.
- currentValue : 현재 처리하고 있는 배열의 요소
- index : 현재 처리하고 있는 배열 요소의 index
- array : 현재 처리하고 있는 배열
thisArg : callback 함수 내부에서 사용할 this 레퍼런스
```javascript
const numbers = [1,2,3,4,5];
const processed = numbers.map((number,index) => {
    return number * number;
});
console.log(processed); // 출력결과 : 1,4,9,16,25
```
- 동일한 원리로 기존 배열로 컴포넌트로 구성된 배열을 생성할 수 있다.
=> *Component가 요소가 되는 배열*을 생성할 수 있다.
```javascript
// @JSX코드로 된 배열을 생성
import React from 'react';

const IterationSample = () => {
    const names = ['봄','여름','가을','겨울'];
    const namesList = names.map((name,index) => <li key={index}>{name}</li>);
    
    return <ul>{namesList}</ul>;
}

export default IterationSample;
```
또는
- 1. List Component 정의
```javascript
// @PATH : /component/List.js
import React from 'react';

const List = (props) => {
    return <li key={props.key}>{props.children}</li>
};

export default List;
```
<br>
- 2. Parnet Component에서 List Component import
```javscript
import React, {useState} from 'react';
import List from './List';

const ListExample = () => {
    const names = ['Spring', 'Summer', 'Fall', 'Winter'];
    const namesList = names.map((name,index) => {
        return <List key={index}>{name}</List>
    });

    return <ul>{namesList}</ul>

};

export default ListExample;
```

- key란?
 리액트에서 key는 Component 배열을 렌더링 했을 때 어떤 원소에 변동이 있었는지 알아내려고 사용한다. 예를 들어 유동적인 데이터를 다룰 때는 원소를 새로 생성할 수 도, 제거할 수 도, 수정할 수도 있다. key가 없을 때는 Virtual DOM을 비교하는 과정에서 리스트를 순차적으로 비교하면서 변화를 감지해야 한다.<br>
하지만, key가 존재하면 이 값을 사용하여 어떤 변화가 일어났는지 더욱 빠르게 확인 할 수 있다.

- key값 설정
 key값을 설정할 때는 map 함수의 인자로 전달되는 함수 내부에서 컴포넌트 props를 설정하듯이 설정하면 된다. key 값은 언제나 유일해야 한다. 따라서 데이터가 가진 고유값을 key 값으로 설정해야 한다.
 게시판의 게시물을 렌더링한다면 게시물 번호(ID)를 key 값으로 설정한다.
```javascript
const articleList = articles.map(article => (
    <Article
        title={article.title}
        writer={article.writer}
        key={article.id}
    />
));


```
- 응용
 inputbox와 button 각각 1개 씩 추가한다. inputBox에 기입한 Text를 button을 클릭하게 되면 List(ul)태그에 li태그로 추가한다.<br>
여기서 중요한 부분은 *names 배열에 새 항목을 추가할 때 기존 배열에 추가하는 것이 아니라, 새로운 배열을 생성해서 다시 재 할당하는 것이다.*
> 불변성(Immutable) 유지라는 React Concept을 벗어나지 않기 위한 규칙
[기존 배열은 원본을 유지하고, 새로운 배열에 추가]
```javascript
const srcArray = [1,2,3,4,5];
const nextArray = srcArray.concat(6);
console.log(srcArray);  // 출력결과 : 1, 2, 3, 4, 5
console.log(nextArray); // 출력결과 : 1, 2, 3, 4, 5, 6
```
또는 ES6의 새롭게 추가된 Spread Syntax를 사용하면 좀 더 간결하게 코딩할 수 있다.
```javascript
const srcArray = [1,2,3,4,5];
const nextArray = [
    ...srcArray,
    6,
    7
];
console.log(srcArray);  // 출력결과 : 1, 2, 3, 4, 5
console.log(nextArray); // 출력결과 : 1, 2, 3, 4, 5, 6
```

[전체코드]
```javascript
import React, {useState} from 'react';
import List from './List';

const ListUp = () => {
    const [names, setNames] = useState(
        [
            {id: 1, text: 'Spring'},
            {id: 2, text: 'Summer'},
            {id: 3, text: 'Fall'},
            {id: 4, text: 'Winter'}
        ]
    );

    const [inputText, setInputText] = useState('');
    const handleChange = event => setInputText(event.target.value);

    const [nextId, setNextId] = useState(names.length);
    const handleButtonClick = () => {
        // 첫번째 State Variable "names"
        setNames([
            ...names,
            {id: nextId + 1, text: inputText}
        ]);

        setNextId(nextId + 1);
        setInputText('');
    }

    const namesList = names.map((name) => {
        return <li key={name.key}>{name.text}</li>
        //return <List key={name.id}>{name.text}</List>
    });

    return (
        <div>
            <ul>{namesList}</ul>
            <input
                type="text"
                value={inputText}
                onChange={handleChange}
            />
            <button
                onClick={handleButtonClick}
            >추가</button>

        </div>

    )

};

export default ListUp;
```



## Environment 구축
- Jack's Thinking<br>
Babel은 ES6+코드를 ES5이하의 Syntax로 변환해주는 역할을 한다. 그래서 아직 지원하지 않는 "New Syntax"를 사용할 수 있게 해준다.<br>
그렇다면 Node.js/Express에서 사용하게 된다면 router에 존재하는 \*.js 파일들을 변환해서 사용할 수 있게 해준다?<br>
/app.js에서 routes 경로를 정의 해주는 부분에 정의를 해준다.

1. *Transpiler* 
- babel : Javascript compiler, ES6+의 new syntax를 순수 javascript code로 변환해주는 컴파일러다.
- install Guide :
```sh
# Make Project Directory
$ mkdir es6-project && cd es6-project
# Create package.json
$ npm init -y
# Install @babel/core @babel/cli
$ npm install --save-dev @babel/core @babel/cli

# 만약에 install 중에 “No Xcode or CLT version detected” Error 발생 시에,
# https://cleody.com/fix-no-xcode-or-clt-version-detected-when-running-npm-install/
$ sudo rm -r -f /Library/Developer/CommandLineTools
$ xcode-select --install
```
- Babel을 사용하여 ES6+ 코드를 ES5이하의 코드로 트랜스파일링하기 위해 Babel CLI 명령어를 사용할 수도 있으나 npm script를 사용하여 트랜스파일링 적용.
1. package.json파일에 script를 추가한다.
추가할 코드 : "build": "babel src/js(Target Folder) -w -d dist/js"
```json
{
  "name": "react",
  "version": "0.0.0",
  "private": true,
  "scripts": {
    "build": "babel src/js -w -d dist/js"
    "start": "node ./bin/www"
  },
  "dependencies": {
    "cookie-parser": "~1.4.4",
    "debug": "~2.6.9",
    "ejs": "~2.6.1",
    "express": "~4.16.1",
    "http-errors": "~1.6.3",
    "morgan": "~1.9.1"
  },
  "devDependencies": {
    "@babel/cli": "^7.8.4",
    "@babel/core": "^7.8.7",
    "@babel/preset-env": "^7.8.7",
    "webpack": "^4.42.0",
    "webpack-cli": "^3.3.11"
  }
}
```





2. *Module Bundler* Webpack : 번들링하는 모듈 번들러. Webpack을 사용하면 의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈 Loader 필요 없어지게 된다.


## Welcome to React
* Who are You? React.
- 리액트는 User Interface(UI)에 사용하기 위해 만들어진 라이브러리이다.
- 리액트는 라이브러리(Library)이다. 그러나, 전통적인 javascript framework처럼  모든 기능을 지원하지 않는다.
- 새로운 ECMAScript Syntax를 사용한다.
- 함수형 javascript 기반이다?

## Emerging Javascript(ECMScript6+)
* Declaring Variable in ES6
- ES6 이전의 문법은 Only *"var"* keword만 존재했다.
- ES6 부터는 const/let keywrod가 지원된다.
- const : A constant is a variavle that cannot be changed.
```javascript
// Prior to ES6, Declaring Variable
var pizza = true;
pizza = false;
console.log(pizza); // print "false" at console;

// ES6 const
const pizza = true;
pizza = false;  // Uncaught TypeError : Assigment to constant variable;
```
- let : Lexical Variable scoping. 보통의 프로그래밍 언어에서 사용되는 변수이며, Scope는 block 안에서 유효하다.
```javascript
// Only var keyword
var topic = "Javascript";
if(topic){
  var topic = "React";
  console.log('block ',topic) // print 'block React';
}
console.log('global ',topic) // print 'Global React';

// let keyword
// let 키워드를 사용해서 변수를 선언하게 되면, 유효범위는 블럭(Block)내부에서만 유효하다.
// 그래서 let 키워드를 사용하면 전역변의 값을 보호할 수 있다.
var topic = "Javascript";
if(topic){
  let topic = "React";
  console.log('block ',topic) // print 'block React';
}
console.log('global ',topic) // print 'Global Javascript';
```
* Template Strings
```javascript
// ES6
let firstName = 'Yoo';
let lastName = 'JIN HO';
console.log(`${lastName}, ${firstName}`);
```

* Default Parameters at Arguments of Function
```javascript
function logActivity(name="Jack.cpt", activity="games"){
  console.log('${name} loves ${activity}`);
}
```
* Arrow Function : More Simple Declare Function
```javascript
// Traditional Function
const lordify = function(firstname){
  return `${firstname} of Canterbury`;
};

// ES6 Arrow Function
const lordify_ES6 = (firstname) => {
  return `${firstname} of Canterbury`;
}

// OR Parameter Only One
// Arg가 한개이고, 하나의 리턴값만 있다면 아래처럼 선언하면,
// `${firstname} of Canterbury`의 결과가 리턴 된다.
// return keyword는 생략할 수 있다.
const oneParameter = firstname => `${firstname} of Canterbury`;
```

* ES6 Object and Arrays
```javascript
const sanwich = {
  bread : 'dutch crunch',
  meat : 'tuna',
  cheese : 'swiss',
  toppings : ['lettuce','tomato','mustard']
};

let {bread, meat} = sandwich;
console.log(bread, meat); // print 'dutch crunch tuna'

let [firstResort] = ['first','second','third'];
console.log(firstResort); // print 'first'

let [,,third] = ['first','second','third'];
console.log(third); // print 'third'
```

* Object Literal Enhancement
```javscript
let name = 'Jin Ho';
let sound = 'Good';
// Old
const skier = {
  name : name,
  sound : sound,
  powderYell: function () {
    let yell = this.sound.toUpperCase()
    console.log(`${yell} ${yell} ${yell}!!!`) 
  },
  speed : function(mph) { 
    this.speed = mph
    console.log('speed:', mph) }
  }
};

// New
const skier = {
  name,
  sound,
  powderYell() {
    let yell = this.sound.toUpperCase()
    console.log(`${yell} ${yell} ${yell}!!!`) 
  },
  speedcio(mph) { 
    this.speed = mph
    console.log('speed:', mph) }
  }
};
```

* Modules
```javascript
// mTest.js
export const print = (message) => log(message, new Date());
export const log = (message, date) => {
  console.log(`${message} ${date}`);
};

// main.js
import { print, log } from `${rootpath}` + /module/mTest.js;

print('Come Message', new Date());
```
* Spread Operator
- Spread Operator는 "..."(3 dot)로 표현한다.
```javascript
// Array의 join()와 동일한 Action
// First The spread operator allows us to combine the contents of Array.
var peaks = ['Tallac', 'Ralston', 'Rose'];
var canyons = ['Ward','Blackwood'];
var tahoe = [...peaks, ...canyons];

// Collect function arguments as an Array.
function directions(...args) {
  var [start, ...remaining] = args;
  var [finish, ...stops] = remaining.reverse();
  console.log(`drive through ${args.length} towns`);
  console.log(`start in ${start}`);
  console.log(`the destination is ${finish}`);
  console.log(`stopping ${stops.length} times in between`);
};

directions(
     "Truckee",
    "Tahoe City",
    "Sunnyside",
    "Homewood",
    "Tahoma"
);

// Combine Objects
var morning = {
  breakfast : 'oatmeal',
  lunch : 'peanut butter and jelly'
};

var dinner = 'mac and cheese'

var backpackingMeals = {
  ...morning,
  dinner
};

console.log(backpackingMeals);
// print '{ reakfast: "oatmeal", lunch: "peanut butter and jelly", dinner: "mac and cheese"}


```

## Functional Programming with Javascript
* React는 functional programming 이기 때문에, javascript의 functional programming에 대해 자세한 이해가 선행적으로 필요하다.
* What it Means to Be Functional?
- Javascript 함수는 *"First Class Citizens(1등급 시민)"* 이다. 1등급 시민이라는 것은 *"함수를 변수처럼 사용할 수 있다"*라는 의미이다.
```javascript
const log = message => console.log(message);
const obj = {
  message : 'They can be added to objects like variable',
  log(message){
    console.log(message);
  }
};

obj.log(obj.message);

// Add functions to Array.
const messages = [
  'They can be inserted into arrays'.
  message => console.log(message),
  'like variables',
  message => console.log(message)
];

messages[1](messages[0]); // print 'They can be inserted into arrays'
messages[3](messages[2]); // print 'like variables.'

// Also can be sent to other functions as arguments just like other variables;
// const insideFn = logger => logger('They can be sent to other functions as arguments');
const insideFn = logger => logger(logger);
insideFn(message => console.log(message));

// Return from other functions, just Like Variables
var createScream = function(logger) {
  return function(message) {
    logger(message.toUpperCase() + "!!!") 
  }
};

// createScream은 Function을 return 한다.
// 그래서 scream 변수에는 function(message){ logger(message.toUpperCase() + '!!!'); }가 담겨 있다.
const scream = createScream(message => console.log(message));
scream('functions can be returned from other functions');
scream('createScream returns a function');
scream('scream invokes that returned function');

```

## Imperative(명령형 or 절차적) vs Declarative(선언형)
- 절차적으로 코드를 작성하게 되면 그에 해당하는 설명을 열심히 주석으로 처리를 해야한다. 그런데 선언적 함수형 프로그래밍을 하게 되면 그것 자체로 어떠한 동작을 하는지에 대한 선언이 되어 있다.

## Functional Concepts
- Immutability 불변성, Purity 순수성, Data transformation 데이터 변환, Higher-order-functions, and Recursion 재귀호출<br>
* Immutability<br>
- 'Uncahnageable', 변할수 없는, data는 변할 수 없다. data은 절대 변할 수 없다.<br>
- 원본의 데이터는 그대로 살려두고 복사본을 만들어서 데이터를 수정하고 그것을(copies of those data structures) 사용한다.<br>
(Instead of changing tho original data structures, we *build changed copies of those data structures and
use* them(Original Data) instead.)
```javascript

// @Object
// 1. Declare Original Data Structures.
let color_lawn = {
  title  : 'lawn',
  color  : '#00FF00',
  rating : 0
};

// 2. function to change the rating of the color object
function rateColor(color, rating){
  color.rating = rating;
  return color;
};

// Result
console.log(rateColor(color_lawn, 5).rating); // 5
console.log(color_lawn.rating); // 5 <== Original Data mutate

// Javascript, funtcion arguments are references to the actual data.
// 그래서 original color object을 변경 할 수 있기 때문에 좋지 않은 function 이다.
// 우리는 rateColor function을 다시 작성 할 수 있다. how? ==> original goods에 변화를 주지 않게
// Old ver.
var rateColor = function(color, rating){
  return Object.assign({}, color, {rating : rating});
  // 빈 객체를 생성하고 'color'객체를 복사하고 rating을 변경한다.
  // Object.assing is the cop machine; it take a blank object, copies the color to that object,
  // without having to change the original
};

console.log(rateColor(color_lawn, 5).rating); // 5
console.log(color_lawn); // 0

// New ver.
var rateColor = (color, rating) => ({
  // spread operator는 "color"객체를 복사한다.
  ...color,
  // And argument 'rating'을 rating에 overwrite 한다.
  rating
});


// @Array
var list = [
  { title : 'Rad Red' },
  { title : 'Lawn' },
  { title : 'Party Pink' }
];

var addColor = function(title, colors){
  colors.push({title : title});
  return colors;
};

console.log(addColor('Glam Green', list).length); // 4
console.log(list.length); // 4

// Array.push is not an immutable function.
// addColor 함수는 original array를 변경한다. adding another field
// 그래서 우리는 Array.concat를 사용해야만 한다.
const addColor = (title, colors) => array.concat({title});

console.log(addColor('Glam Green', list).length); // 4
console.log(list.length); // 3

// Array.concat concatenates arrays. In this case, it take a new Object, with a new color title,
// and adds it to a copy of the original array.
// New ES6 syntax
// "{title}" 코드는 "{title : "title"}과 동일한 syntax
const addColor = (title, list) => [...list, {title}];
let newList = addColor('newColor', list);
console.log(newList); // 4
console.log(list); // 3
```
* Pure Functions
- return a value that is computed based on its arguments.
- Take at least one argument, and always return a value or another function.
(아무리 적어도, 하나의 인자 값을 가지고 항상 값을 리턴한다. 값이나, 함수형태의 값을
- 전역변수나, application의 상태에 어떠한 영향도 미치지 않는다.
- Pure Function 선언 시 지켜야 할 규칙 3가지<br>
  1. The function should take in at least one argument.<br>
  2. The function should return a value or another function.<br>
  3. The function should not change or mutate any of its arguments.<br>
```javascript
// #1. inpure Functions
var frederick = {
  name : 'Fredericks Douglass',
  candRead : false,
  canWrite : false
};

function selfEducate(){
  frederick.canRead = true;
  frederick.canWrite = true;
  return frederick
};

selfEducate();

console.log(frederick);

// #2. pure function
const frederick = {
  name : 'Fredericks Douglass',
  candRead : false,
  canWrite : false
};

// 이 함수는 argument를 기반으로 연산을 수행하고 하나의 argument를 가지고 있으며 하나의 새로운 person객체를 리턴한다.
// 전달받은 argument의 mutating이 없고, has no side effect
const selfEducate = person => ({
  ...person,
  person.canRead = true,
  person.canWrite = true
});

console.log(selfEducate(frederick)); // print 'name : Frdericks Douglass, canRead : true, canWrite : true'
console.log(frederick); // print 'name : Frdericks Douglass, canRead : false, canWrite : false'

// Examine an impure function that mutates the DOM;
function Header(text){
  let h1 = document.createElement('h1');
  h1.innerText = text;
  document.body.appendChild(h1);
};

Header("Header() caused sid effects");

// @React, The UI is expressed with pure function. 
// React에서 UI는 순수한 기능으로 표현됩니다. 다음 샘플에서 헤더는 이전 예제와 같은 요소 인 제목을 만드는 데 사용할 수있는 순수한 함수입니다. 
// 그러나 이 함수 자체는 DOM을 변경하지 않기 때문에 부작용을 일으키지 않습니다. 이 함수는 heading-one 요소를 생성하며 해당 요소를 사용하여 
// DOM을 변경하는 것은 응용 프로그램의 다른 부분에 달려 있습니다.
const Header = (props) => <h1>{props.title}</h1>;
```

* Data Transformations
- 그렇다면 데이터가 불변하다면, Application의 변화를 어떻게 할 줄 수 있는가?
- 함수형 프로그래밍은 *데이터를 한 형태(One Form)에서 다른 형태(Another Form)으로로 변환*하는 것입니다.
- We will produce 
