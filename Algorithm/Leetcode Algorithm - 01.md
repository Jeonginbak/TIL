> wecode 7기 동기들과 30days 알고리즘 풀기를 하고 있다. 매일 한 문제씩 풀며 easy의 경우에는 하루에 2문제씩 매일 밤 9시에 문제를 풀었든 못풀었든 공유하기로 했다. 

# 1days
## 33. Search in Rotated Sorted Array
> Suppose an array sorted in ascending order is rotated at some pivot unknown to you beforehand.
(i.e., [0,1,2,4,5,6,7] might become [4,5,6,7,0,1,2]).
You are given a target value to search. If found in the array return its index, otherwise return -1.
You may assume no duplicate exists in the array.
Your algorithm's runtime complexity must be in the order of O(log n).
&nbsp;
**Example 1**
```
Input: nums = [4,5,6,7,0,1,2], target = 0
Output: 4
```
&nbsp;
**Example 2**
```
Input: nums = [4,5,6,7,0,1,2], target = 3
Output: -1
```

<br>

## 풀이 과정
nums라는 배열과 하나의 숫자가 주어진다. 이 숫자가 배열에 있으면 배열의 index 값을 반환하고 없으면 -1을 반환한다. 처음에는 ```for```문을 활용하여 배열을 돌면서 숫자가 있는지 확인하려고 하였으나 생각해보니 ```indexOf()```를 사용하여 풀면 숫자가 배열에 있으면 해당하는 index 값을 반환 할 것이고 없으면 -1을 반환하기 때문에 짧게 코딩할 수 있었다.
요즘 코드카데미와 wecode에서 프리코스때 할 수 있는 javascript를 통해서 indexOf()관련 문제를 풀어서 빨리 생각해 낼 수 있었다.
```javascript
const search = (nums, target) => {
  let result = nums.indexOf(target)
  return result
}
```
주어진 함수는 ```var search = function(nums, targer) {}``` 이었으나 ES6방식에 맞춰서 고쳐서 코딩해보았다. ```let result```로 변수를 선언하여 값을 넣어주지 않고 바로 ```return```에서 ```indexOf()```를 사용하여도 된다.
```javascript
const search = (nums, target) => { 
  return nums.indexOf(target)
}
```
<br>

## 다른 풀이
### 1. binary search tree를 활용한 풀이
다른 동기가 푼 알고리즘의 런타임 복잡도는 O(log n)순서 여야 한다. 라는 문제 조건에 맞춰서 풀이한 것이다.

```javascript
var search = function (nums, target) {
   if (nums.length === 0) {
    return -1;
  }  
  let root;
  let start = 0;
  let end = nums.length - 1;
  let result = -1;  
  while (start <= end) {
    root = nums.indexOf(Math.min.apply(null, nums.slice(start, end + 1)));    
    if (nums[root] === target) {
      result = root;
      break;
    }
    else if ( nums[start] <= target && target <= nums[root - 1] ) {
      end = root - 1;
    } 
    else {
      start = root + 1;
    }
  }
  return result;
};
```
Math.min.apply(null, nums.slice(start, end + 1)로 중앙 값(배열 내 최솟값) 0의 index를 root로 잡아 값을 찾음.

#### Big-O 표기법
[알고리즘의 시간 복잡도와 Big-O 쉽게 이해하기](https://blog.chulgil.me/algorithm/)
Big-O 표기법은 불필요한 연산을 제거하여 알고리즘 분석을 쉽게 할 목적으로 사용한다.
- 시간복잡도
알고리즘의 성능을 설명. 알고리즘을 수행하기 위해 프로세스가 수형해야하는 연산을 수치화 한 것.
O(n log n) : 문제를 해결하기 위한 단계의 수가 N*(log2N) 번만큼의 수행시간을 가진다.(선형로그형)
- O(log n) O(n log n)
주로 입력 크기에 따라 처리 시간이 증가하는 정렬알고리즘에서 많이 사용된다.

#### 이진탐색
이진 탐색이란 데이터가 정렬돼 있는 배열에서 특정한 값을 찾아내는 알고리즘이다. 배열의 중간에 있는 임의의 값을 선택하여 찾고자 하는 값 X와 비교한다. X가 중간 값보다 작으면 중간 값을 기준으로 좌측의 데이터들을 대상으로, X가 중간값보다 크면 배열의 우측을 대상으로 다시 탐색한다. 동일한 방법으로 다시 중간의 값을 임의로 선택하고 비교한다. 해당 값을 찾을 때까지 이 과정을 반복한다.

### 2. typescript 적용
typescript를 적용하여 푼 동기도 있었다. 내가 생각한 풀이 방법과 같으나 typescript가 적용되었다. 
```typescript
function search(nums: number[], target: number): number {
 return nums.indexOf(target) // nums 원소 중 target과 일치하는 원소 index return, 일치 원소 없을 시 -1 return
};
```
<br>

## 활용되었던 javascript 정리

### while
while문은 조건문이 참일 때 실행되는 반복문이다. 조건은 문장안이 실행되기 전에 참, 거짓을 판단한다.
```javascript
// n이 3보다 작을 때까지 반복한다.
var n = 0;
var x = 0;

while (n < 3) {
  n++;
  x += n;
}
```
반복을 살펴보면, n을 x에 계속 더하게 된다. 그러므로 x와 n 변수는 다음의 값을 갖는다.
첫번째 반복; n=1 과 x=1
두번째 반복; n=2 과 x=3
세번째 반복; n=3 과 x=6
세번째 반복후, n<3 이라는 초건은 더 이상 참이아니가 되므로 반복은 종료된다.

[mdn 공식문서 while](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/while)
```javascript
while (condition)
  statement
```
- **condition**
반복이 시작되기 전에 조건문은 참,거짓을 판단받게 된다. 만약 조건문이 참이라면, while문 안의 문장들이 실행된다. 거짓이라면, 문장은 그냥 while 반복문 후로 넘어간다.
- **statement**
조건문이 참일 때만 while문 속의 문장들이 실행된다. 반복문 속에 여러개의 문장을 사용하고 싶다면 중괄호 { } 를 통해 문장들을 하나로 묶어야 한다.

<br>

### indexOf()
indexOf() 메서드는 배열에서 지정된 요소를 찾을 수 있는 첫 번째 인덱스를 반환하고 존재하지 않으면 -1을 반환합니다.
```javascript
arr.indexOf(searchElement[, fromIndex]) // array뿐만 아니라 string도 가능가능하다.
```
```javascript
const beasts = ['ant', 'bison', 'camel', 'duck', 'bison'];
console.log(beasts.indexOf('bison'));
// expected output: 1
// start from index 2
console.log(beasts.indexOf('bison', 2));
// expected output: 4
console.log(beasts.indexOf('giraffe'));
// expected output: -1
```
[mdn공식문서 indexOf()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)

<br>

### Math
```Math```는 수학적인 상수와 함수를 위한 속성과 메서드를 가진 내장 객체입니다. 함수 객체가 아닙니다. 다른 전역 객체와 달리 Math는 생성자가 아닙니다. Math의 모든 속성과 메서드는 정적입니다. 파이 상수는 ```Math.PI```로 참조할 수 있고, 사인 함수는 매개변수 x에 대해 ```Math.sin(x)```와 같이 호출할 수 있습니다. 상수는 JavaScript에서 가능한 최대 실수 정밀도로 정의되어 있습니다.
```javascript
Math.max([x[, y[, …]]])
// 0개 이상의 인수에서 제일 큰 수를 반환합니다.
```
```javascript
Math.min([x[, y[, …]]])
// 0개 이상의 인수에서 제일 작은 수를 반환합니다.
```
[mdn 공식문서 Math](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math)

<br>

### apply()
```apply()``` 메서드는 주어진 this 값과 배열 (또는 유사 배열 객체) 로 제공되는 arguments 로 함수를 호출합니다.
```javascript
const numbers = [5, 6, 2, 3, 7];

const max = Math.max.apply(null, numbers);

console.log(max);
// expected output: 7

const min = Math.min.apply(null, numbers);

console.log(min);
// expected output: 2
```
[mdn공식문서 apply()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

<br>

### slice()
```slice()``` 메서드는 어떤 배열의 begin부터 end까지(end 미포함)에 대한 얕은 복사본을 새로운 배열 객체로 반환합니다. 원본 배열은 바뀌지 않습니다.
```javascript
const animals = ['ant', 'bison', 'camel', 'duck', 'elephant'];

console.log(animals.slice(2));
// expected output: Array ["camel", "duck", "elephant"]

console.log(animals.slice(2, 4));
// expected output: Array ["camel", "duck"]

console.log(animals.slice(1, 5));
// expected output: Array ["bison", "camel", "duck", "elephant"]
```
```javascript
arr.slice([begin[, end]])
```
- **begin**(Optional)
0을 시작으로 하는 추출 시작점에 대한 인덱스를 의미합니다.
음수 인덱스는 배열의 끝에서부터의 길이를 나타냅니다. slice(-2) 는 배열에서 마지막 두 개의 엘리먼트를 추출합니다.
begin이 undefined인 경우에는, 0번 인덱스부터 slice 합니다.
begin이 배열의 길이보다 큰 경우에는, 빈 배열을 반환합니다.
- **end**(Optional)
추출을 종료 할 0 기준 인덱스입니다. slice 는 end 인덱스를 제외하고 추출합니다.
예를 들어, slice(1,4)는 두번째 요소부터 네번째 요소까지 (1, 2 및 3을 인덱스로 하는 요소) 추출합니다.
음수 인덱스는 배열의 끝에서부터의 길이를 나타냅니다. 예를들어 slice(2,-1) 는 세번째부터 끝에서 두번째 요소까지 추출합니다.
end가 생략되면 slice()는 배열의 끝까지(arr.length) 추출합니다.
     만약 end 값이 배열의 길이보다 크다면, silce()는 배열의 끝까지(arr.length) 추출합니다.s