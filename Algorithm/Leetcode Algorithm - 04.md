## 55. Jump Game
> Given an array of non-negative integers, you are initially positioned at the first index of the array.
Each element in the array represents your maximum jump length at that position.
Determine if you are able to reach the last index.
&nbsp;
**Example 1**
```
Input: nums = [2,3,1,1,4]
Output: true
Explanation: Jump 1 step from index 0 to 1, then 3 steps to the last index.
```
&nbsp;
**Example 2**
```
Input: nums = [3,2,1,0,4]
Output: false
Explanation: You will always arrive at index 3 no matter what. Its maximum jump length is 0, which makes it impossible to reach the last index.
```
&nbsp;
**Constraints**
1 <= nums.length <= 3 * 10^4
0 <= nums[i][j] <= 10^5

<br>

## 풀이 과정
양수로 이루어진 배열에서 각 배열의 요소는 오른쪽으로 이동할 수 있는 최대 길이이다. 주어진 배열이 배열의 마지막 요소에 도달 할 수 있는지 여부를 판단하는 하여 true나 false를 반환해야 하는 문제 였다.
어제와 마찬가지로 예시로 주어진 문제는 풀렸지만 특정 경우에는 잘못된 결과가 나왔다.😭 
요소의 값이 배열의 길이를 넘어도 괜찮지만 요소의 값이 배열의 길이를 넘지 못하면 false가 나와야 하는데 문제의 핵심을 파악하지 않고 무조건 문제만 풀었던 것 같다. 
잘 풀지 못해도 코드 뜯어보고 하면서 점점 나아지지 않을까 생각은 하는데 계속해서 실패하니까 내가 정말 잘하고 있는건지 이대로 실력이 늘기는 하는 건지 우울한 생각이 들었다.
다음 문제도 어렵기는 마찬가지라 심란한 마음이 든다.😔
```javascript
var canJump = function(nums) {
    let move = nums[0];
    let len = nums.length-1;
    for (let i = 0; i < nums.length-1 ; ++i) {
        if (move === nums[i]) {
            move = nums[i+move]
        } else {
            move;
        }
    }
    if (move === nums[len]) {
      return true 
      } else {
      return false
      }
  }; // 오늘도 실패 ㅜㅜ
```
move라는 변수에 배열의 첫번째 요소를 할당하고 for문을 돌면서 그 값과 같은지 비교해서 같으면 index 값에 move를 더하여 move의 값을 업데이트 한다. 최종적으로 move의 값이 마지막 요소의 값과 같은지를 체크해서 true 또는 false를 반환한다. 
이 풀이의 문제는 마지막요소가 0인데 배열의 요소 중에 0이 있는 경우 또는 요소의 값이 배열의 길이를 넘을 경우 제대로 된 값을 반환하지 못했다. 
오늘은 오후 7시 부터 제출시간 9시까지 딱 2시간 가량만 문제 풀이에 시간을 할애 했다 그래도 너무 피곤했다😩 블로그 정리를 매일 익일에 하게 된다. 문제가 풀렸으면 피로가 싹풀렸을 텐데... 언젠가는 꼭 성공했음 좋겠다!

<br>

## 다른 풀이
```Math.max``` 메서드가 자주 사용되는 것 같다. 이 메서드를 잘 활용할 수 있도록 해야겠다. 
다들 사용한 메서드나 함수는 비슷했다.
### 1. 오늘의 알고리즘 풀이
풀이를 잘 써주셔서 문제를 이해하는데 도움이 많이 됬다.
```javascript
var canJump = function(nums) {
    let standard = nums[0];
    
    for (let i = 1; i < nums.length; i++) {
      //이미 0번째를 standard로 검사 했으니까 1부터 시작
        if (standard === 0) {
          return false; 
        }
      //standard가 0이면 무조건 false, 요소가 0이면 더 이상 점프 할 수 없다!
        standard--;
      //다음 숫자로 넘어갈 수록 점프할 수 있는 구간이 하나씩 줄어들기 때문에 -- 해줘야함
        standard = Math.max(standard, nums[i]);
      //standard와 그 다음 숫자를 비교해서 큰 숫자로 옮겨 가야 마지막까지 도달할 수 있음
    }
    return true;
  // 위의 for문으로 마지막까지 도달할 수 없는 모든 경우의 수를 검사 했으므로 나머지는 무조건 true
};
```
<br>

### 2. 풀이 2
```typescript
function canJump(nums: number[]): boolean {
    let leap = 0; // 점프 가능한 최대 거리

    for (let i=0; i<nums.length; i++) {
        if (i <= leap) { // 도달 가능 할 경우 만
            leap = Math.max(leap, i + nums[i]); // leap 업데이트
            if (leap >= nums.length - 1) return true; // 최대 점프거리 >= 끝
        }
    }
    return false;
};
```
<br>

### 3. 풀이 3
```javascript
var canJump = function(nums) {
    let max = 0; // 갈 수 있는 최대값.

    for (let i = 0; i < nums.length; i++) {
        if (i > max) return false; // i가 이 최대값 보다 크다면 i는 도달하지 못 하는 곳이다. false를 반환.

        if (i + nums[i] >= nums.length - 1) return true; // 현재 index + index의 값을 더했을 때 nums의 길이를 넘어가면 true!

        max = Math.max(max, i + nums[i]); // 최대 어디까지 갈 수 있는지 max 값을 갱신.
    }
};
```
<br>

### 4. 풀이 4
```javascript
var canJump = function(nums) {
   let lastIndex = nums.length-1;
    let result = false;
    for (let i = nums.length-1; i >= 0; i--) {
        if (i + nums[i] >= lastIndex) {
            lastIndex = i;
            result = true;
        }
        else {
            result = false;
        }
    }  
    return result;
};
```

<br>

## 활용되었던 javascript 정리
오늘 활용되었던 javascript는 오늘까지 이어져온 알고리즘에서 꾸준히 사용되는 것들이다.
### if
### for
### Math.max
