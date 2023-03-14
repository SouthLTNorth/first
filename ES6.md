# let 命令
> let命令 声明的变量只在所在代码块内有效, 且先声明后使用。`for (let i = 0; i < 10; i++) {}`

# const 命令
> const命令 声明的变量只在所在代码块内有效, 且声明同时初始化。

***

# 数组

## 赋值
> 数组的元素是按次序排列的。
```
	let [a, b, c] = [1, 2, 3];//a:1 b:2 c:3
	let [a, [[b], c]] = [1, [[2], 3]];//a:1 b:2 c:3
	let [a, , c] = [1, 2, 3];//a:1 c:3
	let [a, ...b] = [1, 2, 3, 4];//a:1 b:[2, 3, 4]
	let [a, b, ...c] = [1];//a:1 b:undefined c:[]
```

## 默认值
> 只有当一个数组成员严格等于undefined，默认值才会生效。
```
	let [x = 1] = [undefined];//x:1
	let [x = 1] = [null];//x:null
```

***

# 对象

## 赋值
> 对象的属性没有次序，变量必须与属性同名，才能取到正确的值。
```
	let { bar, foo } = { foo: 'aaa', bar: 'bbb' };//foo:"aaa" bar:"bbb"
	let { baz } = { foo: 'aaa', bar: 'bbb' };//baz:undefined
```

## 默认值
> 只有当一个对象成员严格等于undefined，默认值才会生效。
```
	var {x = 3} = {x: undefined};//x:3
	var {x = 3} = {x: null};//x:null
```

***

# Map

## 定义
> Map 结构: Iterator 接口的对象, for...of循环
```
	var map = new Map();
	map.set(key, value);
	for (let [key, value] of map) {
		console.log(key);
		console.log(value);
	}

```

***

# 字符串

## 字符的 Unicode 表示法
> 只限于码点在\u0000~\uFFFF之间的字符。超出这个范围的字符，必须用两个双字节的形式表示, 可用`for...of`循环。
```
	'\z' === 'z'  // true
	'\172' === 'z' // true
	'\x7A' === 'z' // true
	'\u007A' === 'z' // true
	'\u{7A}' === 'z' // true
```

## 模板字符串

### 解析
> '字符' === '字符的转义形式' `'中' === '\u4e2d'`
> JavaScript 规定有5个字符，不能在字符串里面直接使用，只能使用转义形式。
```
	U+005C：反斜杠 \u005c
	U+000D：回车 \u000d
	U+000A：换行符 \u000a
	U+2028：行分隔符 \u2028
	U+2029：段分隔符 \u2029
```
> 含字符转义形式的解析 `eval("'\u2028'") or eval("'\u2029'")`

### 定义
> 用反引号包括, ${} 引入变量或函数。
> `Hello ${name}!` or `Hello ${fn()}!`;

### 编译
```
	let template = `
		<ul>
		  <% for(let i=0; i < arr.length; i++) { %>
			<li><%= arr[i] %></li>
		  <% } %>
		</ul>
	`;
```

## String.fromCharCode() 和 String.fromCodePoint()
> String.fromCharCode();用于从 Unicode 码点返回对应字符，但是这个方法不能识别码点大于0xFFFF的字符。
> String.fromCodePoint();可以识别大于0xFFFF的字符。

## codePointAt()
> str.charAt();一般用来读取 2 个字节的字符。
> str.charCodeAt();只能分别返回前两个字节和后两个字节的值。
> str.codePointAt();能够正确处理 4 个字节储存的字符，返回一个字符的码点(十进制)。

## String.raw()
> String.raw`Hi\n${2+3}!`;它会将所有变量替换，而且对斜杠进行转义。

## includes(), startsWith(), endsWith()
> 可以用来确定一个字符串是否包含在另一个字符串中。
> includes();返回布尔值，表示是否找到了参数字符串。
> startsWith();返回布尔值，表示参数字符串是否在原字符串的头部。
> endsWith();返回布尔值，表示参数字符串是否在原字符串的尾部。

> 第二个参数，表示开始搜索的位置。endsWith它针对前n个字符，其他两个方法针对从第n个位置直到字符串结束。

## repeat()
> 返回一个新字符串，表示将原字符串重复n次。`'x'.repeat(3);// "xxx"`

## padStart() 和 padEnd()
> 字符串补全长度的功能，padStart()用于头部补全，padEnd()用于尾部补全。
> 两个参数，第一个参数是字符串补全生效的最大长度，第二个参数是用来补全的字符串。

## trimStart() 和 trimEnd()
> 返回新字符串，trimStart()、trimLeft()消除字符串头部的空格，trimEnd()、trimRight()消除尾部的空格。

## replace() 和 replaceAll()
> replace()只能替换第一个匹配。`str.replace(/b/g, '_');str.replace('b', '_');`
> replaceAll()可以一次性替换所有匹配。`str.replaceAll(/b/g, '_');str.replaceAll('b', '_');`
> replaceAll()的第二个参数: 替换的文本 或 特殊字符串 或 函数。
```
	$&：匹配的字符串。
	$` ：匹配结果前面的文本。
	$'：匹配结果后面的文本。
	$n：匹配成功的第n组内容，n是从1开始的自然数。这个参数生效的前提是，第一个参数必须是正则表达式。str.replaceAll(/(ab)(bc)/g, '$2$1');
	$$：指代美元符号$。
	str.replaceAll('b', () => '_');
```

## at()
> 返回参数指定位置的字符，支持负索引（即倒数的位置）。`str.at(1);str.at(-1);`

***

# 正则表达式

## RegExp 构造函数
`var regex = new RegExp(a, b);`
> ES5: 参数a是一个字符串，参数b表示正则表达式的修饰符（flag）。
> ES5: 参数a是一个正则表示式，无参数b。`var regex = new RegExp(/a/g);`
> ES6: 参数a是一个字符串或正则表示式，参数b表示正则表达式的修饰符（flag）。

## 字符串的正则方法
> 字符串对象共有 4 个方法，可以使用正则表达式：match()、replace()、search()和split()。

## u 修饰符
> u修饰符，含义为“Unicode 模式”，用来正确处理大于\uFFFF的 Unicode 字符。也就是说，会正确处理四个字节的 UTF-16 编码。
```
	/^\uD83D/u.test('\uD83D\uDC2A') // false
	/^\uD83D/.test('\uD83D\uDC2A') // true

```
> RegExp.prototype.unicode 属性，表示是否设置了u修饰符。
```
	const r1 = /hello/;
	const r2 = /hello/u;
	r1.unicode // false
	r2.unicode // true
```

## y 修饰符
> 全局匹配，匹配必须从剩余的第一个位置开始。
> 是否设置了y修饰符 `/正则/y.sticky`

## RegExp.prototype.flags 和 RegExp.prototype.source 属性
> 返回正则表达式的正文。`/正则/ig.source`
> 返回正则表达式的修饰符。`/正则/ig.flags`

## s 修饰符：dotAll 模式
> 可以匹配任意单个字符。

## 后行断言 和 先行断言
> 先行断言: 
1. x只有在y前面才匹配: /x(?=y)/
2. 只匹配百分号之前的数字: /\d+(?=%)/

> 先行否定断言: 
1. x只有不在y前面才匹配: /x(?!y)/
2. 只匹配不在百分号之前的数字: /\d+(?!%)/

> 后行断言:
1. x只有在y后面才匹配: /(?<=y)x/
2. 只匹配美元符号之后的数字: /(?<=\$)\d+/

> 后行否定断言
1. x只有不在y后面才匹配: /(?<!y)x/
2. 只匹配不在美元符号后面的数字:/(?<!\$)\d+/

## 遍历器转为数组
> 使用...运算符和Array.from()方法就可以了。`[...string.matchAll(regex)]`或`Array.from(string.matchAll(regex))`

# 数值

## 二进制和八进制表示法
> 二进制: 用前缀0b（或0B）表示。
> 八进制: 用前缀0o（或0O）表示。
> 转十进制: Number(二进制/八进制)

## Number.isFinite(), Number.isNaN()
> Number.isFinite(): 检查一个数值是否为有限的, 参数为数值。(非全局方法)
> Number.isNaN(): 检查一个值是否为NaN。(非全局方法)

> isFinite(): 检查一个数值是否为有限的, 参数为数值。(全局方法)
> isNaN(): 检查一个值是否为NaN。(全局方法)

## Number.parseInt(), Number.parseFloat()
> Number.parseInt(): 将数值转为整数, 参数为数值。(非全局方法)
> Number.parseFloat(): 将数值转为小数, 参数为数值。(非全局方法)

> parseInt(): 将数值转为整数, 参数为数值。(全局方法)
> parseFloat(): 将数值转为小数, 参数为数值。(全局方法)

## Number.isInteger()
> 判断一个数值是否为整数, 参数为数值。

## Number.EPSILON
> Number.EPSILON实际上是 JavaScript 能够表示的最小精度。误差如果小于这个值，就可以认为已经没有意义了，即不存在误差了。

## 安全整数和 Number.isSafeInteger()
> Number.isSafeInteger(): 判断一个整数是否落在这个范围之内。

## Math 对象的扩展
> Math.trunc(): 去除一个数的小数部分，返回整数部分。
```
	// 兼容方法
	Math.trunc = Math.trunc || function(x) {
		return x < 0 ? Math.ceil(x) : Math.floor(x);
	};
```
> Math.sign(): 判断一个数到底是正数、负数、还是零。对于非数值，会先将其转换为数值。
```
	// 兼容方法
	Math.sign = Math.sign || function(x) {
		x = +x; // convert to a number
		if (x === 0 || isNaN(x)) {
			return x;
		}
		return x > 0 ? 1 : -1;
	};
```
> Math.cbrt(): 计算一个数的立方根。
```
	// 兼容方法
	Math.cbrt = Math.cbrt || function(x) {
		var y = Math.pow(Math.abs(x), 1/3);
		return x < 0 ? -y : y;
	};
```
> Math.hypot(): 返回所有参数的平方和的平方根。

## 对数方法
> Math.expm1(): 返回 ex - 1，即Math.exp(x) - 1。
```
	// 兼容方法
	Math.expm1 = Math.expm1 || function(x) {
		return Math.exp(x) - 1;
	};
```
> Math.log1p(): 返回1 + x的自然对数，即Math.log(1 + x)。如果x小于-1，返回NaN。
```
	// 兼容方法
	Math.log10 = Math.log10 || function(x) {
		return Math.log(x) / Math.LN10;
	};
```
> Math.log10(): 返回以 10 为底的x的对数。如果x小于 0，则返回 NaN。
```
	// 兼容方法
	Math.log1p = Math.log1p || function(x) {
		return Math.log(1 + x);
	};
```
> Math.log2(): 返回以 2 为底的x的对数。如果x小于 0，则返回 NaN。
```
	// 兼容方法
	Math.log2 = Math.log2 || function(x) {
		return Math.log(x) / Math.LN2;
	};

```

## Boolean()、Number()和String()
> 转为布尔值、数值和字符串类型。

---

# 函数

## 函数参数的默认值
```
	function a(x, y = 1) {
		console.log(y);
	}
```

## 严格模式
> 'use strict';

## 箭头函数
> 没有this指代对象。

## Function.prototype.toString()
> 返回函数本身（去掉换行）。

## catch 命令的参数省略
> try {} catch (err) {} 可以写成 try {} catch {}

## apply
> 最大值: Math.max.apply(null, [14, 3, 77]);//77
> Math.max(...[14, 3, 77]) == Math.max(14, 3, 77);

## Array.of()
用于将一组值，转换为数组。

## find()，findIndex()，findLast()，findLastIndex()
> 找到符合条件的组合: arr.find(function(){})

## includes()
> Array.prototype.includes: 返回一个布尔值，表示某个数组是否包含给定的值
> 与字符串的includes方法类似。

## Map 结构的has方法，是用来查找键名的，比如Map.prototype.has(key)、WeakMap.prototype.has(key)、Reflect.has(target, propertyKey)。

## Set 结构的has方法，是用来查找值的，比如Set.prototype.has(value)、WeakSet.prototype.has(value)。

## flat和flatMap
```
	// 相当于 [[[2]], [[4]], [[6]], [[8]]].flat()
	[1, 2, 3, 4].flatMap(x => [[x * 2]]);
	// [[2], [4], [6], [8]]
	
	[1, [2, [3]]].flat(Infinity);
```

## at
```
	const arr = [5, 12, 8, 130, 44];
	arr.at(2) // 8
	arr.at(-2) // 130
```

## toReversed()，toSorted()，toSpliced()，with()
> toReversed()对应reverse()，用来颠倒数组成员的位置。
> toSorted()对应sort()，用来对数组成员排序。
> toSpliced()对应splice()，用来在指定位置，删除指定数量的成员，并插入新成员。
> with(index, value)对应splice(index, 1, value)，用来将指定位置的成员替换为新的值。

## group()，groupToMap()
```
	const array = [1, 2, 3, 4, 5];
	array.group((num, index, array) => {
		return num % 2 === 0 ? 'even': 'odd';
	});
	// { odd: [1, 3, 5], even: [2, 4] }
	
	[6.1, 4.2, 6.3].groupBy(Math.floor)
	// { '4': [4.2], '6': [6.1, 6.3] }
```
```
	const array = [1, 2, 3, 4, 5];
	const odd  = { odd: true };
	const even = { even: true };
	array.groupToMap((num, index, array) => {
		return num % 2 === 0 ? even: odd;
	});
	//  Map { {odd: true}: [1, 3, 5], {even: true}: [2, 4] }
```

## Array.prototype.sort() 的排序稳定性
```
// 稳定的
const arr = ['peach','straw','apple','spork'];
const stableSorting = (s1, s2) => {
  if (s1[0] < s2[0]) return -1;
  return 1;
};
arr.sort(stableSorting)
// ["apple", "peach", "straw", "spork"]
```
```
// 不稳定的
const arr = ['peach','straw','apple','spork'];
const unstableSorting = (s1, s2) => {
  if (s1[0] <= s2[0]) return -1;
  return 1;
};
arr.sort(unstableSorting)
// ["apple", "peach", "spork", "straw"]
```







# Generator 构造函数
> [0,1,1,2,3,5,8,13,21...]
