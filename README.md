# React
React Study and Job
> 해당 글은 Learning React [Book] - O'Reilly ver.Eng 보고 작성한 것 입니다.

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
