# == 的求值
---
## 本文初衷
项目中，一般建议避免使用`==`,使用`===`，作为替代。
原因Douglas Crockford 在他的《JavaScript: The Good Parts》 中已经说得很清楚了，在这里不赘述了。
写代码的时候可以避免`==`,但是在读别人代码的时候还是需要推算一下。
本文结合 [ECMAScript](https://tc39.github.io/ecma262/#sec-abstract-equality-comparison) 标准，讲讲`==`的求值过程。

## 定义
1、对象 ： Object
2、非对象：Undefined Null Boolean Number String Symbol

## 标准过程

假设对比的两个值为 x 和 y，最终的返回结果一定是 true 或者 false。
1. 如果x和y,的类型相同，则返回 `x===y`
2. 如果x 是 null, y是 undefined ， 返回 true，
3. 如果x 是 undefined, y 是null，返回true；
4. 如果 x 是数字，y是字符串，则将y转为数字，然后返回 `x===y`
5. 如果x 是字符串，y是数字，则将x转为数字，然后返回 `x===y`
6. 如果x 是布尔，将x转为数字，然后返回`x==y`
7. 如果y 是布尔，将y转为数字，然后返回`x==y`
8. 如果x是字符串、数组、symbol三者之一，y是对象；将y转成简单类型(ToPrimitive(y))，然后返回x==y
9. 如果x是对象，y是字符串、数组、symbol三者之一；将x转成简单类型，然后返回x==y
10. 返回false

## 规律
+ 有对象类型和非对象类型比较，通过ToPrimitive(该对象，hint=number)，先将对象转成非对象，再比较
+ 非对象，通过ToNumber，转成数字，再比较

`ToNumber(非对象x)`和`非对象x-0`的结果一致。绝大多数请求ToNumber的结果大家都知道，主要难点在于ToPrimitive。

## ToPrimitive ( input [ , PreferredType ] )

先看看方法的签名
```
ToPrimitive ( input [ , PreferredType ] )
```
input 是需要转化的对象，
PreferredType 的值 要么是'string'，要么是'number'，默认是'number'。

假设要把对象A转成一个费对象。

如果 PreferredType 是 'number'：
依次调用 A[Symbol.toPrimitive],A[valueOf],A[toString] 一旦调用成功，且得到一个非对象的值，则返回。
如果PreferredType 是 'string'：在调用的顺序是A[Symbol.toPrimitive],A[toString],A[valueOf] 。

** A[Symbol.toPrimitive] 默认是undefined，除非你设置为一个函数
** 一般日期对象转成非对象的时候PreferredType=string，其他情况下PreferredType=number。



### 总结

### 疑点
+ 严格等于
+ 转为简单类型
+ 转为数字




## 疑点解释

## 示例说明
[]==false

string  « "toString", "valueOf" ».
else « "valueOf", "toString" ».

题目：是否存在a==1,a==2,a==3 都为true


7.2.12SameValueNonNumber ( x, y )
The internal comparison abstract operation SameValueNonNumber(x, y), where neither x nor y are Number values, produces true or false. Such a comparison is performed as follows:

Assert: Type(x) is not Number.
Assert: Type(x) is the same as Type(y).
If Type(x) is Undefined, return true.
If Type(x) is Null, return true.
If Type(x) is String, then
If x and y are exactly the same sequence of code units (same length and same code units at corresponding indices), return true; otherwise, return false.
If Type(x) is Boolean, then
If x and y are both true or both false, return true; otherwise, return false.
If Type(x) is Symbol, then
If x and y are both the same Symbol value, return true; otherwise, return false.
If x and y are the same Object value, return true. Otherwise, return false.



7.2.15Strict Equality Comparison
The comparison x === y, where x and y are values, produces true or false. Such a comparison is performed as follows:

If Type(x) is different from Type(y), return false.
If Type(x) is Number, then
If x is NaN, return false.
If y is NaN, return false.
If x is the same Number value as y, return true.
If x is +0 and y is -0, return true.
If x is -0 and y is +0, return true.
Return false.
Return SameValueNonNumber(x, y).
NOTE
This algorithm differs from the SameValue Algorithm in its treatment of signed zeroes and NaNs.

7.2.14Abstract Equality Comparison
The comparison x == y, where x and y are values, produces true or false. Such a comparison is performed as follows:

If Type(x) is the same as Type(y), then
Return the result of performing Strict Equality Comparison x === y.
If x is null and y is undefined, return true.
If x is undefined and y is null, return true.
If Type(x) is Number and Type(y) is String, return the result of the comparison x == ! ToNumber(y).
If Type(x) is String and Type(y) is Number, return the result of the comparison ! ToNumber(x) == y.
If Type(x) is Boolean, return the result of the comparison ! ToNumber(x) == y.
If Type(y) is Boolean, return the result of the comparison x == ! ToNumber(y).
If Type(x) is either String, Number, or Symbol and Type(y) is Object, return the result of the comparison x == ToPrimitive(y).
If Type(x) is Object and Type(y) is either String, Number, or Symbol, return the result of the comparison ToPrimitive(x) == y.
Return false.
