# Scala 应试速查模板 — Array / Map / 命令式编程

> 针对 CS2 Imperative Programming。重点是你卷面上反复写错的地方:数组创建、Map 操作、`break`、边界条件。

---

## 1. Array — 创建

```scala
val a = Array(1, 2, 3)              // 字面量(❌ 不能写 [1,2,3])
val a = new Array[Int](n)           // 长度 n,全 0
val a = new Array[Boolean](n)       // 长度 n,全 false
val a = Array.fill(n)(0)            // 长度 n,填 0
val a = Array.fill(n)(false)        // 长度 n,填 false
val a = Array.fill(n)(-1)           // 长度 n,填 -1(lastIndex 题用得到)
val a = Array.tabulate(n)(i => i*i) // b(i) = i*i
val a = Array.ofDim[Int](n)         // 等价于 new Array[Int](n)
```

⚠️ 你写过的 `Array[n]` / `Array[Boolean]` **不是合法语法**。要 `new Array[Boolean](n)`。

## 2. Array — 访问与常用方法

```scala
a(i)                // 取元素(圆括号,❌ 不是 a[i])
a(i) = x            // 改元素
a.length            // 长度
a.indices           // 等价于 0 until a.length
a.slice(i, j)       // 子数组 [i, j)
a.take(k)           // 前 k 个
a.drop(k)           // 去掉前 k 个
a ++ b              // 拼接(k-rotated 题用得到:c.slice(k,n) ++ c.slice(0,k))
a.max / a.min       // 最大 / 最小
a.sum               // 求和
a.reverse           // 反转
a.contains(x)       // 是否含 x
```

## 3. mutable.Map(Poly 题核心)

```scala
import scala.collection.mutable

val m = mutable.Map.empty[Int, Double]                       // 空 map
val m = mutable.Map.empty[Int, Double].withDefaultValue(0.0) // 缺省值 0.0
val m = mutable.Map(0 -> 4.0, 1 -> -1.0, 3 -> 2.5)           // 直接初始化

m(e)                  // 取值(缺省时若设了 withDefaultValue 返回缺省,否则抛异常)
m.getOrElse(e, 0.0)   // 安全取值,缺省 0.0(题目给的 coefficient 写法)
m(e) = c              // 插入/更新 ✅(❌ 不是 m[(c,e)] —— 键是指数,值是系数)
m.update(e, c)        // 同上
m.contains(e)         // 是否含键
m.remove(e)           // 删键
m -= e                // 删键(简写)
m.size                // 键数
m.keys / m.values     // 所有键 / 所有值
m.toArray             // 转 Array[(Int, Double)] = (键, 值)
```

遍历 Map:

```scala
for ((e, c) <- m) {        // e=指数, c=系数
  // ...
}
```

✅ **Poly.add 正确写法**:
```scala
def add(p2: Poly): Unit =
  for ((e, c) <- p2.coefficientsAsArray) coeffs(e) = coefficient(e) + c
```

✅ **Poly.multiply 正确写法**(新建 map,算完再赋值):
```scala
def multiply(p2: Poly): Unit = {
  val res = mutable.Map.empty[Int, Double].withDefaultValue(0.0)
  for ((d, cd) <- coeffs; (d2, cd2) <- p2.coefficientsAsArray)
    res(d + d2) = res(d + d2) + cd * cd2
  coeffs = res
}
```

## 4. 元组 Tuple

```scala
val t = (3, 2.5)        // (Int, Double)
t._1                    // 第一个 = 3
t._2                    // 第二个 = 2.5
val (i, j) = (0, n)     // 解构赋值
var (i, j) = (0, n)     // 可变解构(binarySearch 用)
Array[(Int, Double)]    // 元组数组的类型写法
```

## 5. 循环

```scala
while (cond) { ... }

for (i <- 0 until n) { ... }      // 0,1,...,n-1
for (i <- 0 to n) { ... }         // 0,1,...,n  (含 n)
for (i <- n-1 to 0 by -1) { ... } // 倒序 n-1,...,0(leaders 题:从右往左)
for (x <- a) { ... }              // 遍历元素
```

⚠️ **写 while 必检查:循环变量有没有自增/自减!**
你的 segmentEquals 就是漏了 `j += 1` 变成死循环。

## 6. break(Scala 没有原生 break)

**方法 A — flag 法(最稳,考试首选)**:
```scala
var found = false
var i = 0
while (i < n && !found) {
  if (cond) found = true
  else i += 1
}
```

**方法 B — 函数内直接 return**:
```scala
def findRightmost(a: Array[Int], v: Int): Int = {
  var i = a.length - 1
  while (i >= 0) {
    if (a(i) == v) return i   // 直接返回 = 提前退出
    i -= 1
  }
  -1
}
```

**方法 C — Breaks(知道即可,写起来啰嗦)**:
```scala
import scala.util.control.Breaks._
breakable { while (...) { if (cond) break() } }
```

## 7. 前条件 / 不变式断言

```scala
require(a.length > 0)     // 前条件,违反抛 IllegalArgumentException
require(c != 0.0)         // DTI 检查(MonomialPoly 用)
assert(e >= 0)            // 内部不变式,违反抛 AssertionError
```

口诀:**`require` 检查调用者给的输入,`assert` 检查自己维护的不变量。**

## 8. 常量边界值(初始化别再写错)

```scala
Int.MinValue   // 求"最大值"时的初始 curmax —— ❌ 不要用 0!
Int.MaxValue   // 求"最小值"时的初始 curmin
```

✅ **leaders 正确初始化**:`var curmax = Int.MinValue`
(用 0 会让负数数组的末元素被误判,这次就栽在这。)

## 9. 函数签名

```scala
def f(a: Array[Int]): Int = { ... }
def g(a: Array[Int], k: Int): Boolean = { ... }
def h(s: Array[Char], i: Int, m: Int): Int = { ... }
```

返回类型是 `Boolean` 就必须返回 `true/false`,**不能 `return i`**(occursIn 题的坑)。

---

## 10. 高频错误对照表(你这次的真实失分点)

| ❌ 你写的 | ✅ 正确 |
|---|---|
| `Array[n]` | `new Array[Boolean](n)` 或 `Array.fill(n)(false)` |
| `lp([1,1,1])` | `lp(Array(1,1,1))` |
| `m[(c,e)] = ...` | `m(e) = c`(键=指数,值=系数) |
| `var curmax = 0`(求 max) | `var curmax = Int.MinValue` |
| `while(j<n){ if... }` 漏自增 | 循环体内补 `j += 1` |
| 返回类型 Boolean 却 `return i` | `return true` / `return false` |
| `0 until (m-n)`(漏末窗口) | `0 to (m-n)` |
| 切片后下标没加偏移 | `binarySearch(a.slice(s,e), v) + s` |

---

## 11. 两个可直接套的模板片段

**从右往左扫 + 求后缀最大(leaders / 后缀类问题)**:
```scala
def leaders(a: Array[Int]): Array[Boolean] = {
  val n = a.length
  val b = new Array[Boolean](n)
  var curmax = Int.MinValue
  var i = n - 1
  while (i >= 0) {
    if (a(i) > curmax) { b(i) = true; curmax = a(i) }
    else b(i) = false
    i -= 1
  }
  b
}
```

**标准二分(返回最左出现位置,题目原型)**:
```scala
def binarySearch(a: Array[Int], v: Int): Int = {
  var (i, j) = (0, a.length)
  while (i < j) {
    val m = i + (j - i) / 2          // ✅ 防溢出,不要写 (i+j)/2
    if (a(m) < v) i = m + 1 else j = m
  }
  if (i < a.length && a(i) == v) i else -1
}
```
