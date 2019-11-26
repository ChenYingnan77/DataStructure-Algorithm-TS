---
sidebar: auto
---

# 二进制

二进制常见的算法题目之一，二进制的一些相关运算如下：

| 操作名  | 结果      | 结果       | 结果      | 结构      |
| ------- | --------- | ---------- | --------- | --------- |
| 与(&)   | 0&0 == 0  | 0 & 1 == 0 | 1&0 == 0  | 1&1 == 1  |
| 或(\|)  | 0\|0 == 0 | 0\|1 == 1  | 1\|0 == 1 | 1\|1 == 1 |
| 异或(^) | 0^0 == 0  | 0^1 == 1   | 1^0 == 1  | 1^1==0    |

​	位的移动运算：

`<<`为左移运算符， `m<<n`表示把m左移动n位。左移n位时，最左边的n位会丢弃，最右侧会补上n个0

`>>`为右移运算符，`m>>n`表示把m右移动n位，右移动n位时，最右侧的n位会被丢弃，但是左侧会根据**正负**而产生不同的结果，当为正数时，左侧补上n个0，当为负数时，左侧补上n个1。

`>>>`为无符号右移运算符，此时右移不考虑正负因素，全部用0填充。

## 二进制中1的个数

> 此部分代码在 NumberOfOne.ts 中

我们首先使用一种常规方法，我们不右移传入的`num`，而是每次都左移动判断是否存在1的标识符`flag`，当flag为1时，首先和num的最右位判断，如果为1，count加1，之后左移动`flag`，`flag`变为10，便可判断num的右二位是否为1，依次类推 知道左移动到超出二进制最大限度为止，即对于32位的整数要循环32次。

```typescript
/**
 * 编写一个函数，输入是一个无符号整数，返回其二进制表达式中数字位数为 ‘1’ 的个数。
 * @param {number} num 
 * @return {number}
 */
function NumberOfOne(num: number) :number {
    // 记录1的个数
    let count = 0
    // 判断的标识
    let flag = 1
    while(flag) {
        // 当前位为1
        if (num & flag) {
            count++
        }
        // 标识符左移一位 判断次低位是否为1
        flag = flag << 1
    }
    return count
}
```

但是这种解法无论如何都要循环32次，效率低下，我们选择使用一种解法让他只循环1的个数次

我们知道，当一个二进制数不为0，我们就默认它内部含有1，当一个二进制的数减1时，会把它最右侧的1变为0，而这个最右侧的1如果右侧还有0则会统统变成1，我们可以让原数和减一之后的数做与运算，即可知道当前左侧还有几个1。那么一个二进制数有多少个1，就可以循环多少次。

```typescript
/**
 * 编写一个函数，输入是一个无符号整数，返回其二进制表达式中数字位数为 ‘1’ 的个数。
 * @param {number} num 
 * @return {number}
 */
function NumberOfOneBetter(num: number): number {
    let count = 0
    // 如果不为0就认为内部还有1
    while (num) {
        count++
        num = num & (num-1)
    }
    return count
}
```
