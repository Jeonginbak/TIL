## 49. Group Anagrams
> Given an array of strings, group anagrams together.
&nbsp;
**Example**
```
Input: ["eat", "tea", "tan", "ate", "nat", "bat"],
Output:
[
  ["ate","eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```
**Note**
All inputs will be in lowercase.
The order of your output does not matter.

<br>

## 풀이 과정
주어진 배열의 알파벳이 같은 것끼리 묶어서 배열안에 배열로 저장하는 문제였다. 내가 생각했던 방법은 중첩 ```for```문을 작성하고 string 하나하나 인덱스 값을 넣어 비교하여 배열에 넣는 것 이었는데 string의 순서가 같지 않기 때문에 이를 정렬해서 순서를 맞춰야한다고 생각했다. 내가 풀지 못했던 부분은 정렬하는 부분과 배열을 나눠서 넣는 것 이었는데 알고리즘을 풀면서 못 풀면서 받는 스트레스와 다른 할 일을 못하기 때문에 어느 정도 까지해서 안 풀리면 바로 답을 찾자고 생각했다. 아직 익숙하지 않기도 하고 스스로 풀어서 답을 내는 것이 최고 겠지만 반대로 코드를 뜯어 보면서도 얻을 수 있는게 있다고 생각했다. 그러나 이번 문제의 경우는 좀 더 도전했으면 풀 수 있었을 것 같은데 아쉽다. 지레 짐작해서 겁먹지 말고 최대한 도전을 해보자!

#### 내가 찾은 풀이
```javascript
var groupAnagrams = function(strs) {
    let answerMap = new Map();
    
    strs.forEach((word) => {
        let sortedWord = word.split("").sort().join("");
        if (answerMap.has(sortedWord))
            answerMap.get(sortedWord).push(word)
        else
            answerMap.set(sortedWord, [word])
    })
    
    let result = []
    answerMap.forEach((value, key) => {
        result.push(value)
    })
    
    return result;
};
```
<br>



<br>

## 다른 풀이
풀이들을 계속 뜯어보면서 ```split("").sort().join("")```는 다 공통적으로 사용되었고
string의 순서를 정렬한 후 같은 것끼리 어떻게 배열을 담는지 사용하는 방법이 달랐다. 개인적으로 ```Object.values()```를 활용하는 방법이 간단하고 깔끔하다고 생각했다.
### 1. leetcode 게시판에서 map과 for문을 이용한 방법
```javascript
var groupAnagrams = function(strs) {
    const mapStrs = new Map();
    for (let i = 0; i < strs.length; i++) {
        let str = strs[i];
        let sortedStr = str.split('').sort().join('');
        if(mapStrs.size > 0 && mapStrs.has(sortedStr)){
            mapStrs.get(sortedStr).push(str);
        } else { mapStrs.set(sortedStr,[str])
             }
    } let result = []; for(let list of mapStrs){
        result.push(list[1]);
    } return result; };
```

### 2. 중첩 for문을 이용한 풀이
동기가 풀었던 풀이 방법이 이다. 내가 생각한 방법과 비슷했다.
```javascript
var groupAnagrams = function(strs) {
    let result = [];
    let yes = false;
    let newThang = [];
    
    for (let i in strs) {
        for (let j in result) {
            if (result[j][0].split("").sort().join("") === strs[i].split("").sort().join("")) {
                result[j].push(strs[i]);
                yes = true;
            }
        }
        if (!yes) {
            newThang.push(strs[i]);
            result.push(newThang);
        }
        newThang = [];
        yes = false;
    }
    
    return result;
}
```
### 3-1. Object.values()를 활용
``` javascript
var groupAnagrams = function(strs) {
  // 아나그램 가져오기
  const getAnagram = (str) => {
    return str.split("").sort().join("");
  };

  // 아나그램 저장 객체
  let anagramObj = {};

  strs.forEach((str) => {
    // 새로운 아나그램일 경우 키, 벨류 값 생성  ex) aet: [], ant: [], abt: []
    if (anagramObj[getAnagram(str)] === undefined) {
      anagramObj[getAnagram(str)] = [];
    }

    // 해당 아나그램 배열(벨류)에 str 추가
    anagramObj[getAnagram(str)].push(str);
  });

  // 객체의 values (배열) 배열로 모아서 return
  return Object.values(anagramObj);
};
```

### 3-2. Object.values()를 활용
동기들이 뽑은 잘 풀었다고 생각하는 둘째날 알고리즘 풀이 방법이다.
```javascript
var groupAnagrams = function(strs) {
  const result = {};
  strs.forEach(str => {
    let al = str.split('').sort();
    result[al] ? result[al].push(str) : result[al] = [str];
    //만약에 키값이 있으면, 그 키 value에 str을 push, 없으면, key: value쌍을 새로 만들자
  })
  return Object.values(result) //객체의 value만 나오게 해주자!
};
```
<br>

## 활용되었던 javascript 정리
### forEach()
```forEach()``` 메서드는 주어진 함수를 배열 요소 각각에 대해 실행합니다.
```javascript
const array1 = ['a', 'b', 'c'];

array1.forEach(element => console.log(element));

// expected output: "a"
// expected output: "b"
// expected output: "c"
```
[mdn공식문서 foreach()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)

```javascript
arr.forEach(callback(currentvalue[, index[, array]])[, thisArg])
// callback : 각 요소에 대해 실행할 함수. 세 가지 매개변수(요소 값, 요소 인덱스, 순회중인 배열)를 받는다.
// currentvalue : 처리할 현재 요소
// index(optional) : 처리할 현재 요소의 인덱스
// array(optional) : foreach()를 호출한 배열
// thisArg(optional) : callback을 실행할 때 this로 사용할 값
// 반환값 : undefind
```
- ```forEach()```는 주어진 callback을 배열에 있는 각 요소에 대해 오름차순으로 한 번씩 실행한다.
- 예외를 던지지 않고는 ```forEach()```를 중간에 멈출 수 없다. 중간에 멈춰야 한다면 ```forEach()```가 적절한 방법이 아닐수도 있다.
- ```foreach()```는 반복전에 배열의 복사본을 만들지 않는다.
- ```for```반복문을 ```foreach()```로 바꾸기
```javascript
const items = ['item1', 'item2', 'item3'];
const copy = [];

// 이전
for (let i=0; i<items.length; i++) {
  copy.push(items[i]);
}

// 이후
items.forEach(function(item){
  copy.push(item);
});
```
<br>

### split()
```split()``` 메서드는 String 객체를 지정한 구분자를 이용하여 여러 개의 문자열로 나눕니다.
```javascript
let str = "eat"
let splitStr = str.split("");
console.log(splitStr); // 결과 [ 'e', 'a', 't' ]
```
[mdn공식문서 split()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/String/split)
```javascript
str.split([separator[, limit]])
```
- **separator(optional) **
원본 문자열을 끊어야 할 부분을 나타내는 문자열을 나타냅니다. 실제 문자열이나 정규표현식을 받을 수 있습니다. 문자열 유형의 separator가 두 글자 이상일 경우 그 부분 문자열 전체가 일치해야 끊어집니다. separator가 생략되거나 str에 등장하지 않을 경우, 반환되는 배열은 원본 문자열을 유일한 원소로 가집니다. separator가 빈 문자열일 경우 str의 각각의 문자가 배열의 원소 하나씩으로 변환됩니다.

- **limit(optional) ** 
끊어진 문자열의 최대 개수를 나타내는 정수입니다. 이 매개변수를 전달하면 split() 메서드는 주어진 separator가 등장할 때마다 문자열을 끊지만 배열의 원소가 limit개가 되면 멈춥니다. 지정된 한계에 도달하기 전에 문자열의 끝까지 탐색했을 경우 limit개 미만의 원소가 있을 수도 있습니다. 남은 문자열은 새로운 배열에 포함되지 않습니다.

- 주어진 문자열을 separator마다 끊은 부분 문자열을 담은 Array 반환

<br>

### sort()
```sort()``` 메서드는 배열의 요소를 적절한 위치에 정렬한 후 그 배열을 반환합니다.
정렬한 배열. 원 배열이 정렬된다. 복사본이 만들어지지 않는다.
```javascript
const months = ['March', 'Jan', 'Feb', 'Dec'];
months.sort();
console.log(months);
// expected output: Array ["Dec", "Feb", "Jan", "March"]

const array1 = [1, 30, 4, 21, 100000];
array1.sort();
console.log(array1);
// expected output: Array [1, 100000, 21, 30, 4]
```
[mdn공식문서 sort()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
```javascript
arr.sort([compareFunction])
```
- **compareFunction(optional)**
정렬 순서를 정의하는 함수. 생략하면 배열은 각 요소의 문자열 변환에 따라 각 문자의 유니 코드 코드 포인트 값에 따라 정렬됩니다. ```compareFunction```이 제공되지 않으면 요소를 문자열로 변환하고 유니 코드 코드 포인트 순서로 문자열을 비교하여 정렬합니다.

<br>

### join()
```join()``` 메서드는 배열의 모든 요소를 연결해 하나의 문자열로 만듭니다.
배열의 모든 요소드을 연결한 하나의 문자열을 반환합니다. 만약 ```arr.length```가 0이라면(undefined 또는 null), 빈 문자열을 반환합니다.
```javascript
const elements = ['Fire', 'Air', 'Water'];

console.log(elements.join());
// expected output: "Fire,Air,Water"

console.log(elements.join(''));
// expected output: "FireAirWater"

console.log(elements.join('-'));
// expected output: "Fire-Air-Water"
```
[mdn공식문서 join()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/join)

```javascript
arr.join([separator])
```
- **separator(optional)**
배열의 각 요소를 구분할 문자열을 지정합니다. 이 구분자는 필요한 경우 문자열로 변환됩니다. 생략하면 배열의 요소들이 쉼표로 구분됩니다. separator가 빈 문자열이면 모든 요소들이 사이에 아무 문자도 없이 연결됩니다.
- 네 가지 다른 방법으로 배열 연결하기
다음 예제에서는 3개의 요소를 가진 배열 a를 만들고, 기본 구분자, 쉼표와 공백, 더하기 기호, 빈 문자열의 네 가지 구분자를 사용해 배열을 연결합니다.
```javascript
var a = ['바람', '비', '불'];
var myVar1 = a.join();      // myVar1에 '바람,비,불'을 대입
var myVar2 = a.join(', ');  // myVar2에 '바람, 비, 불'을 대입
var myVar3 = a.join(' + '); // myVar3에 '바람 + 비 + 불'을 대입
var myVar4 = a.join('');    // myVar4에 '바람비불'을 대입
```
<br>

### Map
- Map 객체는 키-값 쌍을 저장하며 각 쌍의 삽입 순서도 기억하는 콜렉션입니다. 
- Map 객체는 요소의 삽입 순서대로 원소를 순회합니다.
- Map과 Object의 차이(mdn공식문서 읽어보기)
[mdn공식문서 Map](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Map)
```
let newMap = new Map(); //새로운 Map 객체를 생성
```
- **Map 인스턴스**
모든 Map 인스턴스는 Map.prototype을 상속합니다.

- **속성**
```Map.prototype.constructor```
인스턴스의 프로토타입을 만드는 함수를 반환한다. 이것 Map 함수의 기본 값이다.
```Map.prototype.size```
Map 객체에 들어있는 key/value pair의 수를 반환한다.
- **메서드**
위의 코드에서 사용된 것들만 정리해 놓았다.
```javascript
Map.prototype.forEach(callbackFn[, thisArg])
// Map 객체 안에 존재하는 각각의 key/value pair에 집어넣은 순서대로 callbackFn을 부른다.
// 만약 thisArg 매개변수가 제공되면, 이것이 각 callback의 this 값으로 사용된다.
```
```javascript
Map.prototype.get(key)
//주어진 키(Key)에 해당되는 값(value)을 반환하고, 만약 없으면 undefined를 반환한다.
```
```javascript
Map.prototype.has(key)
// Map 객체 안에 주어진 key/value pair가 있는지 검사하고 Boolean 값을 반환한다.
```
```javascript
Map.prototype.keys()
// Map 객체 안의 모든 키(Key)들을 집어넣은 순서대로 가지고 있는 Iterator 객체를 반환한다.
```
```javascript
Map.prototype.set(key, value)
// Map 객체에 주어진 키(Key)에 값(Value)를 집어넣고, Map 객체를 반환한다.
```
```javascript
Map.prototype.values()
// Map 객체 안의 모든 값(Value)들을 집어넣은 순서대로 가지고 있는 Iterator 객체를 반환한다.
```

<br>

### Object.values()
```Object.values()```요소가 객체에서 찾을 수있는 열거 가능한 속성 값인 배열을 반환합니다. 속성의 순서는 객체의 속성 값을 수동으로 반복하여 주어진 순서와 동일합니다.
```javascript
const object1 = {
  a: 'somestring',
  b: 42,
  c: false
};
console.log(Object.values(object1));
// expected output: Array ["somestring", 42, false]
```
[mdn공식문서 Object.values()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values)
```javascript
Object.values( obj )
```
- obj : 열거 가능한 자체 속성 값을 가진 개체가 반환
- 반환 값 : 지정된 객체의 열거 가능한 속성 값을 포함하는 배열

<br>

### Prototype
자바스크립트 프로토타입에 대해 알아야 할 것 같아서 찾아보았다. 당장 한번에 이해하기엔 무리가 있을 것 같아서 공식문서와 잘 설명된 블로그를 천천히 읽어봐야겠다.
[mdn공식문서 object prototype](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/Object_prototypes)
JavaScript는 흔히 프로토타입 기반 언어(prototype-based language)라 불립니다.— 모든 객체들이 메소드와 속성들을 상속 받기 위한 템플릿으로써 프로토타입 객체(prototype object)를 가진다는 의미입니다. 프로토타입 객체도 또 다시 상위 프로토타입 객체로부터 메소드와 속성을 상속 받을 수도 있고 그 상위 프로토타입 객체도 마찬가지입니다. 이를 프로토타입 체인(prototype chain)이라 부르며 다른 객체에 정의된 메소드와 속성을 한 객체에서 사용할 수 있도록 하는 근간입니다.

[Prototype1](https://poiemaweb.com/js-prototype)
ECMAScript spec에서는 자바스크립트의 모든 객체는 [[Prototype]]이라는 인터널 슬롯(internal slot)를 가진다. [[Prototype]]의 값은 null 또는 객체이며 상속을 구현하는데 사용된다. [[Prototype]] 객체의 데이터 프로퍼티는 get 액세스를 위해 상속되어 자식 객체의 프로퍼티처럼 사용할 수 있다. 하지만 set 액세스는 허용되지 않는다. 라고 되어있다.
[[Prototype]]의 값은 Prototype(프로토타입) 객체이며 ```__proto__``` accessor property로 접근할 수 있다. ```__proto__``` 프로퍼티에 접근하면 내부적으로 ```Object.getPrototypeOf```가 호출되어 프로토타입 객체를 반환한다.
student 객체는 ```__proto__``` 프로퍼티로 자신의 부모 객체(프로토타입 객체)인 Object.prototype을 가리키고 있다. 객체를 생성할 때 프로토타입은 결정된다. 결정된 프로토타입 객체는 다른 임의의 객체로 변경할 수 있다. 이것은 부모 객체인 프로토타입을 동적으로 변경할 수 있다는 것을 의미한다. 이러한 특징을 활용하여 객체의 상속을 구현할 수 있다.

[Prototype2](https://medium.com/@pks2974/javascript-%EC%99%80-prototype-%ED%94%84%EB%A1%9C%ED%86%A0-%ED%83%80%EC%9E%85-515f759bff79)
