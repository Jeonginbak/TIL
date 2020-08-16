## 53. Maximum Subarray
> Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.
&nbsp;
**Example**
```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```
**Follow up**
If you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

<br>

## 풀이 과정
주어진 배열에서 가장 큰 합계를 가진 연속 된 배열을 찾아서 그 합계를 반환하는 문제 였다. 예시로 주어진 배열을 기준으로 문제를 풀이 하였다. 
첫번째 index부터 마지막 index까지 순차적으로 누적합계를 구하는데 한 번 더할 때 마다 그 값을 누적합을 모아두기 위한 새로운 array에 담았다. 그리고 첫번째 index를 삭제하고 다시 첫번째 index부터 마지막 index까지 더한 값을 또 array에 담았다. 이런식으로 제일 첫번째 index을 삭제해 가면서 단계적으로 처음부터 끝까지 더하면 예시에서 보여준 값을 찾을 수 있다고 판단하였다. 
누적된 합계를 구하기 위해서 reduce를 사용하였고 첫번째 인덱스를 삭제해가면서 계속 더해야 했기 때문에 for문과 array.shift()를 사용하였다. 그리고 누적합을 모아둔 배열에서 최대값을 찾아 반환하였다. 
이렇게 하면 예시로 주어진 배열과 같은 값은 얻을 수 있었지만 문제를 푸는데는 실패했다😭
submit을 해보니 문제가 되는 경우가 배열 길이가 1일 경우, 2일 경우와 배열의 요소의 값이 전부 음수일 경우 였다.
배열의 길이가 1일 경우에는 그 값이 곧 최대값이 되기 때문에 그대로 보여줘야 했는데array.shift()를 사용하다보니 삭제가 되서 빈 배열이 나왔다. 배열의 길이가 2일 경우에는 요소가 하나는 음수, 하나는 양수일 경우 최대 값은 당연히 양수가 되어야 하는데 누적합을 구해버리니 잘못된 결과가 나왔다. 이 부분들은 if문으로 해결해 볼 수 있었다. 배열의 길이가 1일 때는 그대로 그 값을 반환하면 되었고 배열의 길이가 2인데 그 합이 1보다 작을 경우에는 배열의 요소 중 최대 값을 찾아 반환하는 식으로 말이다. 
애초부터 전제를 잘못두고 풀었다고 느낀게 배열의 길이가 3인데 배열의 요소가 전부 음수일 경우였다. 이 경우에도 역시 배열의 요소중 최대값이 결과 여야 했다.
9시가 되어서 일단 답안을 단체 슬랙방에 제출을 해놓고 좀 더 도전해봤는데 위와 같은 결론에 도달 했다😢
```javascript
var maxSubArray = function(nums) {  
    let addArr = [];
        for(let i = 0; i < nums.length; i++){
          nums.reduce(function(acc, cur, curidx, arrNums){
          addArr.push(acc+cur)
          return acc+cur
          })
          nums.shift()
        } 

      return Math.max.apply(null, addArr)
};

maxSubArray([-2,1,-3,4,-1,2,1,-5,4]) 
//최대 값인 6은 나오지만 특정의 경우에는 값이 나오지 않았다ㅠㅠ
```
문제는 풀지 못했지만 이것 저것 다양한 시도를 해볼 수 있었다. 이게 알고리즘을 하는 이유이지 싶지만 풀수 있을 것 같아서 하루종일 붙잡고 있었다가 하루종일 알고리즘 한 문제만 풀었다.🤣
할 것도 많으니 시간 분배를 잘해야 겠다.

<br>

## 다른 풀이
동기 분들의 풀이 방법을 보니 제일 많이 쓴것은 ```Math.max```라는 메서드 였다. 구글링 해서 알아냈다고 하던데 풀이 과정에 따라 내가 필요한 것을 검색하다 보니 못찾았던건지 답안 찾기가 쉬워서 최대한 검색은 mdn에서만 했는데 역시 구글링도 실력이란 생각이 들었다.

### 1-1. Math.max()를 활용한 풀이
```javascript
var maxSubArray = function(nums) {
  let addNum = 0;
  let result = nums[0];
  for(let i of nums) {
    addNum = Math.max(i, addNum+i);
    // addNum + i랑 i랑 비교했을 때 큰 숫자를 남겨야함
    result = Math.max(result, addNum);
    // nums의 배열요소 하나하나를 배교해서 만약 요소 하나가 제일 큰 경우 그 요소를 결과값으로 저장, 아니면 addNum을 결과값로 저장
  }
  return result;
};
```

### 1-2. Math.max()를 활용한 풀이
오늘의 알고리즘 풀이 방법이다. Math.max()를 사용한 방법은 코드가 깔끔하고 짧았다.
전개 구문이라는 것을 알게 되었다.
```javascript
var maxSubArray = function(nums) {
   for (let i = 1; i < nums.length; i++) {
       nums[i] = Math.max(nums[i], nums[i] + nums[i - 1]);
   };
    return Math.max(...nums);
};
```

### 1-3. Math.max()를 활용한 풀이
```javascript
var maxSubArray = function(nums) {
    let largest = nums[0];
    for (let i = 1; i < nums.length; i++) {
        if (nums[i - 1] > 0) {
            nums[i] += nums[i - 1];
        }
        largest = Math.max(nums[i], largest);
    }
    return largest;
};
```

### 2. forEach()활용한 풀이

```javascript
function maxSubArray(nums: number[]): number {
    let sum = 0 // 현재 최대값
    let maxSum = nums[0]; // 전체 최대값

    if (nums.length === 1) return maxSum;

    nums.forEach((num, index) => {
        if (num > sum + num) { // 현재 숫자가 최대 + 현재보다 더 크면 sum 새로 시작
            sum = num;
        } else {
            sum += num;
        }
        if(sum >= maxSum) { // 현재 최대가 가장 크면 maxSum 업데이트
            maxSum = sum;
        }
        console.log(num, sum, maxSum);
    });
    return maxSum;
};
```
<br>

## 활용되었던 javascript 정리

### reduce()
reduce()는 빈 요소를 제외하고 배열 내에 존재하는 각 요소에 대해 callback 함수를 한 번씩 실행하는데, 콜백 함수는 다음의 네 인수를 받습니다. 
accumulator, currentValue, currentIndex, array 
콜백의 최초 호출 때 accumulator와 currentValue는 다음 두 가지 값 중 하나를 가질 수 있습니다. 만약 reduce() 함수 호출에서 initialValue를 제공한 경우, accumulator는 initialValue와 같고 currentValue는 배열의 첫 번째 값과 같습니다. initialValue를 제공하지 않았다면, accumulator는 배열의 첫 번째 값과 같고 currentValue는 두 번째와 같습니다.
```javascript
const array1 = [1, 2, 3, 4];
const reducer = (accumulator, currentValue) => accumulator + currentValue;

// 1 + 2 + 3 + 4
console.log(array1.reduce(reducer));
// expected output: 10

// 5 + 1 + 2 + 3 + 4
console.log(array1.reduce(reducer, 5));
// expected output: 15
```

[mdn 공식문서 reduce()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Array/Reduce)

```javascript
arr.reduce(callback[, initialValue])
```
- **callback** : 배열의 각 요소에 대해 실행할 함수. 다음 네 가지 인수를 받습니다.
- **accumulator** : 누산기accmulator는 콜백의 반환값을 누적합니다. 콜백의 이전 반환값 또는, 콜백의 첫 번째 호출이면서 initialValue를 제공한 경우에는 initialValue의 값입니다.
- **currentValue** : 처리할 현재 요소.
- **currentIndex**(Optional) : 처리할 현재 요소의 인덱스. initialValue를 제공한 경우 0, 아니면 1부터 시작합니다.
- **array**(Optional) : reduce()를 호출한 배열.
- **initialValue**(Optional) : callback의 최초 호출에서 첫 번째 인수에 제공하는 값. 초기값을 제공하지 않으면 배열의 첫 번째 요소를 사용합니다. 빈 배열에서 초기값 없이 reduce()를 호출하면 오류가 발생합니다.
- **반환 값** : 누적 계산의 결과 값.

<br>

### Math.max()
입력된 숫자 중 가장 큰 숫자를 반환합니다. 만약 인수 중 하나라도 숫자로 변환하지 못한다면 NaN로 반환합니다. max ()는 Math의 정적 메서드이기 때문에 만든 Math 개체의 메서드가 아닌 항상 Math.max ()로 사용해야합니다. (Math는 생성자가 아닙니다). 만약 아무 요소도 주어지지 않았다면 -Infinity로 반환합니다. 만약 한 개 이상의 요소가 숫자로 변환되지 않는다면 NaN로 반환됩니다.
```javascript
Math.max([값1[, 값2[, ...]]])
```
[mdn 공식문서 Math.max()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Math/max)

<br>

### for...of
for...of 명령문은 반복가능한 객체 (Array, Map, Set, String, TypedArray, arguments 객체 등을 포함)에 대해서 반복하고 각 개별 속성값에 대해 실행되는 문이 있는 사용자 정의 반복 후크를 호출하는 루프를 생성합니다.
```javascript
const array1 = ['a', 'b', 'c'];

for (const element of array1) {
  console.log(element);
}

// expected output: "a"
// expected output: "b"
// expected output: "c"
```
[mdn 공식문서 for...of](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/for...of)
```javascript
for (variable of iterable) {
  statement
}
```
- **variable** : 각 반복에 서로 다른 속성값이 variable에 할당됩니다.
- **iterable** : 반복되는 열거가능(enumerable)한 속성이 있는 객체.

<br>

### 전개 구문
전개 구문을 사용하면 배열이나 문자열과 같이 반복 가능한 문자를 0개 이상의 인수 (함수로 호출할 경우) 또는 요소 (배열 리터럴의 경우)로 확장하여, 0개 이상의 키-값의 쌍으로 객체로 확장시킬 수 있습니다.
```javascript
function sum(x, y, z) {
  return x + y + z;
}

const numbers = [1, 2, 3];

console.log(sum(...numbers));
// expected output: 6

console.log(sum.apply(null, numbers));
// expected output: 6
```
[mdn 공식문서 전개 구문](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Spread_syntax)
