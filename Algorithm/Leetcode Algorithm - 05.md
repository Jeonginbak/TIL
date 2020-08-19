## 64. Minimum Path Sum
> Given a m x n grid filled with non-negative numbers, find a path from top left to bottom right which minimizes the sum of all numbers along its path.
Note: You can only move either down or right at any point in time.
&nbsp;
**Example**
```
Input:
[
  [1,3,1],
  [1,5,1],
  [4,2,1]
]
Output: 7
Explanation: Because the path 1→3→1→1→1 minimizes the sum.
```

<br>

## 풀이
주어진 배열에서 왼쪽위에서 부터 오른쪽 아래로 가는 경로에 따른 숫자의 합이 제일 작은 값을 구하는 문제다. 
```javascript
var minPathSum = function(grid) {
  for (let i = 0; i < grid.length; i++) {
    for (let j = 0; j < grid[i].length; j++) {
      if(i === 0 && j === 0) {
        continue;
      }
      else if(i === 0) {
        grid[i][j] += grid[i][j-1];
      } 
      else if(j === 0) {
        grid[i][j] += grid[i-1][j];
      }
      else {
        grid[i][j] += Math.min(grid[i][j-1], grid[i-1][j]);      
      }
    }
  }
  return grid[grid.length-1][grid[0].length-1];
};
```
<br>

### 1. for문 i값은 grid arr 배열의 index 값
```javascript
for (let i = 0; i < grid.length; i++) {}
/* 
grid = [[1,3,1], // i = 0
        [1,5,1], // i = 1
        [4,2,1]] // i = 2 
*/
```
<br>

### 2. for문 j값은 gird arr 요소의 배열 index 값
```javascript
for (let j = 0; j < grid[i].length; j++) {}
/* 
grid = [[1,3,1], 
  // j = 0 1 2
        [1,5,1], 
  // j = 0 1 2
        [4,2,1]]
  // j = 0 1 2 
*/
```
<br>

### 3. i값이 0이고 j값이 0일 때 건너뛰고 계속 진행
```javascript
if(i === 0 && j === 0) {
  continue;
} // 이때 해당 index의 값은 1
```
<br>

### 4. i값이 0일 때 현재 index의 값과 이전 index의 값을 더해준다.
		
```javascript
else if(i === 0) {
     grid[i][j] += grid[i][j-1];
}
// grid[0][1] + gird[0][1-1] // 3 + 1 = 4 // [1,3,1] -> [1,4,1]
// grid[0][2] + grid[0][2-1] // 1 + 4 = 5 // [1,4,1] -> [1,4,5]
```
<br>

### 5. j값이 0일 때 현재 index의 값과 이전 index의 값을 더해준다.
```javascript
else if(j === 0) {
     grid[i][j] += grid[i-1][j];
// grid[1][0] + grid[1-1][0] // 1 + 1 = 2 // [1,5,1] -> [2,5,1]
// grid[2][0] + grid[2-1][0] // 4 + 2 = 6 // [4,2,1] -> [6,2,1]
```
&nbsp;
현재까지 배열 상황, 위의 경우를 다 거치게 되면 다음 위치는 ```index[1][1]```이다.
```javascript
/* 
grid = [[1,4,5],
        [2,5,1],
        [6,2,1]]
*/
```
<br>

### 6. 위의 두가지 경우가 아닐 때에는 현재 위치한 index 기준으로 왼쪽 index 값과 위쪽 index 값 중 작은 값과 현재 index 값을 더한다.
```javascript
else {
      grid[i][j] += Math.min(grid[i][j-1], grid[i-1][j]);  
      }
// 현재위치 grid[1][1]
// grid[1][1-1], grid[1-1][1] -> 2 , 4 // 5 + 2 = 7 // [2,5,1] -> [2,7,1] 
// 현재위치 grid[1][2]
// grid[1][2-1], grid[1-1][2] -> 7, 5 // 1 + 5 = 6 // [2,7,1] -> [2,7,6]
// 현재위치 grid[2][1]
// grid[2][1-1], grid[2-1][1] -> 6, 7 // 2 + 6 = 8 // [6,2,1] -> [6,8,1]
// 현재위치 grid[2][2]
// grid[2][2-1], grid[2-1][2] -> 8, 6 // 1 + 6 = 7 // [6,8,1] -> [6,8,7]
```
&nbsp;
배열 상황
```javascript
/* 
grid = [[1,4,5],
        [2,7,6],
        [6,8,7]]
*/
```
<br>

### 7. ```index[2][2]```의 값은 작은 값끼리 더해진 값
```javascript
return grid[grid.length-1][grid[0].length-1];
// grid.length // 3, grid[0].length // 3
// grid[2][2] output 7
```
