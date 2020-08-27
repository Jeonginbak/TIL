## 122. Best Time to Buy and Sell Stock II
> Say you have an array prices for which the ith element is the price of a given stock on day i.
Design an algorithm to find the maximum profit. You may complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times).
&nbsp;
**Note** 
You may not engage in multiple transactions at the same time (i.e., you must sell the stock before you buy again).
&nbsp;
**Example 1**
```
Input: [7,1,5,3,6,4]
Output: 7
Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), profit = 5-1 = 4.
             Then buy on day 4 (price = 3) and sell on day 5 (price = 6), profit = 6-3 = 3.
```
&nbsp;
**Example 2**
```
Input: [1,2,3,4,5]
Output: 4
Explanation: Buy on day 1 (price = 1) and sell on day 5 (price = 5), profit = 5-1 = 4.
             Note that you cannot buy on day 1, buy on day 2 and sell them later, as you are
             engaging multiple transactions at the same time. You must sell before buying again.
```
&nbsp;
**Example 3**
```
Input: [7,6,4,3,1]
Output: 0
Explanation: In this case, no transaction is done, i.e. max profit = 0.
```
&nbsp;
**Constraints**
1 <= prices.length <= 3 * 10 ^ 4
0 <= prices[i] <= 10 ^ 4

<br>

## 풀이
요소의 값이 주식의 가격이고 요소의 순서가 날짜인 (배열의 첫번째 요소부터 1일) 배열에서 최대의 이익을 구하는 문제. (조건: 구매를 하고서 다음 구매를 하려면 주식을 판매하고서 구매해야함.)
```javascript
var maxProfit = function(prices) {
  let profit = 0;
  for (let i = 1; i < prices.length; i++) {
    if (prices[i] > prices[i-1]) profit += prices[i]-prices[i-1];
  }
  return profit;
};
```
<br>

### 1. profit이라는 변수를 선언하고 0을 할당한다.
```javascript
let profit = 0;
```
<br>

### 2. for문으로 주어진 배열의 ```index[1]``` 부터 순차적으로 반복한다.
```javascript
for (let i = 1; i < prices.length; i++) {}
```
<br>

### 3. if문으로 현재 index와 이전 index를 비교하여 현재 index의 값이 클 경우 현재 index에 이전 index의 값을 빼고 profit변수의 값과 더하여 profit에 저장하고 profit 값을 반환한다.
```javascript
}
if (prices[i] > prices[i-1]) profit += prices[i]-prices[i-1];
}
return profit;

// Example 1 : [7,1,5,3,6,4]
// 1 > 7 pass
// 5 > 1 -> profit = profit + (prices[2]-prices[2-1]) -> 4 = 0 + (5-1)
// 3 > 5 pass
// 6 > 3 -> 7 = 4 + (6-3)
// 4 > 6 pass
// return 7

// Example 2 : [1,2,3,4,5]
// 2 > 1 -> 1 = 0 + (2-1)
// 3 > 2 -> 2 = 1 + (3-2)
// 4 > 3 -> 3 = 2 + (4-3)
// 5 > 4 -> 4 = 3 + (5-4)
// return 4

// Example 2 : [7,6,4,3,1]
// 6 > 7 pass
// 4 > 6 pass
// 3 > 4 pass
// 1 > 3 pass
// return 0
```
<br>

## 136. Single Number
> Given a non-empty array of integers, every element appears twice except for one. Find that single one.
&nbsp;
**Note**
Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?
&nbsp;
**Example 1**
```
Input: [2,2,1]
Output: 1
```
&nbsp;
**Example 2**
```
Input: [4,1,2,1,2]
Output: 4
```

<br>

## 풀이
주어진 배열에서 중복되지 않는 값을 찾아서 반환하는 문제이다.
```javascript
var singleNumber = function(nums) {
  let number = 0;
  for (let i = 0; i < nums.length; i++) {
    number ^= nums[i];
  }
  return number;
}
```
<br>

### 1. number라는 변수를 선언하여 0을 할당한다.
```javascript
let number = 0;
```
<br>

### 2. for문으로 배열을 순차적으로 반복한다.
```javascript
for (let i = 0; i < nums.length; i++) {}
```
<br>

### 3. XOR연산자로 다른 값을 찾아내여 number에 저장하여 값을 반환한다.
```javascript
}
number ^= nums[i];
}
 return number;
}
// Example 1 : [2,2,1]
// 2 ^ 0 // 010 + 000 = 010 // 2
// 2 ^ 2 // 010 + 010 = 000 // 0
// 0 ^ 1 // 000 + 001 = 001 // 1
// return 1

// Example 2 : [4,1,2,1,2]
// 4 ^ 0 // 100 + 000 = 100 // 4
// 4 ^ 1 // 100 + 001 = 101 // 5
// 5 ^ 2 // 101 + 010 = 111 // 7
// 7 ^ 1 // 111 + 001 = 110 // 6
// 6 ^ 2 // 110 + 010 = 100 // 4
```

<br>

## 다른 풀이
### 1. 풀이 1
```javascript
var singleNumber = function(nums) {
    let existedNum = new Set();
    let answer;

    nums.forEach((num, index) => {
        // 미래에 중복 수 나오면 set에 등록
        if (nums.indexOf(num, index + 1) !== -1) {
            existedNum.add(num);
        } else if (!existedNum.has(num)) {
          // 더 이상 중복 수 없고 set에도 없으면 정답
            answer = num;
        }
    })
    return answer;
};
```
<br>

### 2. 풀이 2
```javascript
var singleNumber = function(nums) {
    for (let i in nums) {
        let iRemoved = nums.splice(i,1)[0];
// i번째 element만 빼둠
        if (!nums.includes(iRemoved)) {
// i번째 element가 빠진 nums가 빠진애를 포함하지 않으면
            return iRemoved;
// 걔가 범인
        }
        nums.splice(i,0,iRemoved)
// 아니면 미안하니까 다시 넣어줌
    }
};
```
<br>

## 활용되었던 javascript 정리
### XOR 연산자 (비트 논리적 배타합)
XOR연산자는 대응되는 두 비트가 서로 다르면 1을 반환(0과1, 1과0)하고, 서로 같으면 0을 반환(0과 0, 1과 1)한다.
[mdn 공식문서 XOR연산자](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Bitwise_Operators)

a | b | a^b
--|--|--
0 | 0 | 0
0 | 1 | 1
1 | 0 | 1
1 | 1 | 0
