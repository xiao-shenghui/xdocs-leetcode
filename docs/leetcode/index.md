# LeetCode
> 一个适合前端工程师，从 0 学习算法的文档。    
> 参考至 [leetcode-javascript](https://github.com/sl1673495/leetcode-javascript), 对齐进行排序，整理和学习。  
> 以下内容按`类型优先`，`题序其次`的原则排序。  

## 动态规划
> 动态规划的核心思想: 拆分子问题，记住过往，减少重复计算。  
> `最长回文子串-5`，`括号生成-22`，`最小路径和-64`，`爬楼梯-70`，`解码方法-91`,  
> `三角形最小路径和-120`，`买卖股票的最佳时机-121`，`单词拆分-139`，`最大正方形-221`，   
> `最长上升子序列-300`，`摆动序列-376`，`无重叠区间-435`，`一和零-474`，`目标和-494`，`最长重复子数组-718`，  
> `最长的斐波那契子序列的长度-873`, `下降路径最小和-931`，`最长等差数列-1027`，`最长公共子序列-1143`， 

### 最长回文子串-5
> 用到 [`中心扩散法`](https://blog.csdn.net/sjp11/article/details/124366179) 
- **难度**： 中等
- **题目**： 
给你一个`字符串s`，找到 `s` 中`最长的`回文子串。  
如果字符串的反序与原始字符串相同，则该字符串称为回文字符串。
- **示例 1**：
```html
输入：s = "babad"
输出："bab"
解释："aba" 同样是符合题意的答案。
```
- **示例 2**：
```html
输入：s = "cbbd"
输出："bb"
```
- **提示**：
	- 1 <= s.length <= 1000
	- s 仅由数字和英文字母组成

- **解题思路**
```js
var longestPalindrome = function(s) {
    // 1.回文字符的要求： >= 1个字符，连续的字符
    // 2.判断回文字符是否在字符串内
    // 3.判断最长的回文子串
    
    // 解题思路：
    // 暴力法: 
    // 由长度为n = length-1开始，取连续的n个reverse()之后，判断是否includes()
    // 例如"babad", n = 4, 取"baba",失败，取"abad"失败，到末尾了。
    // 然后 n = 3，取"bab",成功。 出结果。

    // 中心扩散法： 利用回文的两端相等特性, 以及对称性
    // 找一个起始点i，利用：i--, j = ++i往两边同时扩散，
    // 如果s[i] == s[j], 那么它就是回文。
    // 这里要分偶数和奇数回文，
    // 偶数起始点要有两个，i和i+1。 j = i+1+1

    // 此时回文结果为 str.substr(s[i],len) (len就是i和j的距离)
    // 直到i从0边界，扩散到了末尾边界。
};
```
- **解题**
```js
var longestPalindrome = function(s) {
	let len = 0;
    let start = 0;
    for(let i = 1; i<s.length-1; i++){
        // 奇数回文
        check(i,i)
        // 偶数回文
        check(i,i+1)
    }

    // 回文方法
    function check(i,j){
    	// s[i] == s[j]两端相等
        while (i >= 0 && j < s.length-1 && s[i] == s[j]) {
        	// 只要扩散以后的间距，大于上一次间距，就表示出现了更长的回文。
            if (j - i + 1 > len) {
                start = i;
                len = j - i + 1;
            }
            // 对称性：两端同步扩展，一直扩展到边界为止
            i--;
            j++;
        }
    }
    return s.substr(start, len)
}
```
### 括号生成-22
> 用到 [`回溯法`](https://blog.csdn.net/qq_52314655/article/details/122506533) 
- **难度**： 中等
- **题目**： 
数字`n`代表生成`括号`的`对数`，请你设计一个函数，用于能够生成所有**可能的**并且**有效的**括号组合。
- **示例 1**：
```html
输入：n = 3
输出：["((()))","(()())","(())()","()(())","()()()"]
```
- **示例 2**：
```html
输入：n = 1
输出：["()"]
```
- **提示**：
    - `1 <= n <= 8`

- **解题思路**
```js
/**
 * @param {number} n 参数 n: number
 * @return {string[]} 返回 字符串数组
 */
var generateParenthesis = function(n) {
    // 只有2个可选，左括号或者右括号.
    // 并且最终 左括号数量 == 右括号数量 == n
    // 然后是判断合法性即可。 例如: )()(, ())( 是不合法的。
    // 判断合法性：
    // 1. 左边第一个必须是左括号(, 
    // 2. 右边第一个必须是右括号), 
    // 3. 并且左右括号数量一致, 用一个变量n来记录, 
    // 遇到左括号++, 遇到右括号--,最终必须为0
    // 那么说明就是合法的.

    // 暴力递归法
    // 核心: 由一个空串'', 
    // 根据左右括号数量是否小于n, 来往空串追加左括号和右括号, 递归追加
    // 再判断合法性, 
    // 合法就push进结果, 不合法就return.

    // 使用递归的方式来生成所有可能的组合，然后再判断每个组合是否是有效的括号组合。

    // 回溯法：对暴力的改进
    // 一层层递归，尝试搜素答案，找到答案：返回结果，尝试其他的分支。
    // 找不到答案：返回上一层，尝试其他分支。
    // 剪枝: 递归的过程中产生的分支，只要且必须满足 right < left, 因为是left先增加的。

    // 核心：由一个空串'', 根据左括号数量是否小于n, 来往空串追加左括号，
    // 根据右括号是否小于左括号，来往结果中追加右括号。
    // 回溯法通过递归的方式生成所有可能的括号组合，
    // 但在生成过程中会进行限制条件和剪枝操作，
    // 以保证生成的括号组合都是有效的。
    // 优点: 可以避免生成无效的括号组合，因此效率较高。
};
```
- **解题**
- 暴力递归法
```js
// 暴力法
var generateParenthesis = function(n) {
    const result = [];
    // 判断是否有效
    // combination 表示当前组合
    function isValid(combination) {
    let count = 0;
    for (let i = 0; i < combination.length; i++) {
      if (combination[i] === '(') {
        count++;
      } else {
        count--;
      }
      if (count < 0) {
        return false;
      }
    }
    return count === 0;
    }

    function generateAll(combination, left, right, n) {
    // 最终状态，所有括号用完，得到合法组合
    if (left === n && right === n) {
      if (isValid(combination)) {
        result.push(combination);
      }
      return;
    }
    // 分支1：左边括号小于n
    if (left < n) {
      generateAll(combination + '(', left + 1, right, n);
    }
    // 分支2：右边括号小于n
    if (right < n) {
      generateAll(combination + ')', left, right + 1, n);
    }
    }

    generateAll('', 0, 0, n);

    return result;
}
```

- 回溯法
```js
// 回溯+剪枝法
function generateParenthesis(n) {
  const result = [];
  
  function backtrack(combination, left, right, n) {
    // 所有括号用完，得到合法组合
    if (left === n && right === n) {
      result.push(combination);
      return;
    }
    // 分支1：
    if (left < n) {
      backtrack(combination + '(', left + 1, right, n);
    }
    // 分支2： 
    // 右边小于左边，就开始追加。
    // 且先追加的(
    // 所以不需要判断合法性了，等于结合了暴力法的isValid

    // 这里就是剪枝的思想
    if (right < left) {
      backtrack(combination + ')', left, right + 1, n);
    }
  }
  
  backtrack('', 0, 0, n);
  
  return result;
}
```
