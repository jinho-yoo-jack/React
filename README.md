# React
React Study and Job
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
- Immutability 불변성, Purity 순수성, Data transformation 데이터 변환, Higher-order-functions, and Recursion 재귀.
* Immutability
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


  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
```
