## 一. Java 数据类型

### 基本数据类型

整型（byte、short、int、long）、浮点型（float、double）、字符型（char）和布尔型（boolean）

#### 1.1 整数类型

整型是Java中最基本的数据类型之一。它可以用来存储整数值，包括正数、负数和0。Java中的整型有四种类型：byte、short、int和long。

- `byte`：占用1个字节，范围从-128到127。
- `short`：占用2个字节，范围从-32,768到32,767。
- `int`：占用4个字节，范围从-2,147,483,648到2,147,483,647。
- `long`：占用8个字节，范围从-9,223,372,036,854,775,808到9,223,372,036,854,775,807。

示例：

```
byte byteVar = 100;
short shortVar = 1000;
int intVar = 100000;
long longVar = 10000000000L; // 注意：long 类型数值后需要加 'L' 或 'l' 表示。
```

#### 1.2 浮点类型

浮点型是Java中用来存储小数的数据类型。Java中的浮点型有两种类型：float和double。

- `float`：占用4个字节，可以存储大约6~7位有效数字。
- `double`：占用8个字节，可以存储大约15位有效数字。

示例：

```
float floatValue = 3.14f;
double doubleValue = 3.14;
```

#### 1.3 字符类型

* `char`是Java中用来存储字符的数据类型。在Java中，字符型使用单引号`' '`表示，只能存储一个字符。

示例：

```
char ch = 'A';
```

#### 1.4 布尔类型

* `boolean`是Java中用来存储逻辑值的数据类型，只能存储`true`或者`false`两种状态。

示例：

```
boolean b = false;
```

## 二. Kotlin 数据类型

### 基本数据类型

与 Java 基本类型相对应，Kotlin 也有 8 种基本数据类型。

- 整数类型：Byte、Short、Int、Long，Int 是默认类型
- 浮点类型：Float、Double，Double是默认类型
- 字符类型：Char
- 布尔类型：Boolean

Kotlin 中去掉了原始类型，只有包装类型，编译器在编译代码的时候，会自动优化性 能，把对应的包装类型拆箱为原始类型。

Kotlin 系统类型分为可空类型和不可空类型。 Kotlin 中引入了可空类型，把有可能为 null 的值单独用可空类型来表示。这样就在可空引用与不可空引用之间划分出一条明确的、 显式的“界线”。

#### 2.1 整数类型

与 Java类似， Kotlin也提供了4种整型。

- `Byte`：1个字节（8位），取值范围 -128 ~ 127。
- `Short`：2个字节（16位），取值范围 -32768（-2 ^ 15） ~ 32767（2 ^ 15 - 1）。
- `Int`：4个字节（32位），取值范围 -2147483648（-2 ^ 31）~ 2147483647（2 ^ 31 - 1）。
- `Long`：8个字节（64位），取值范围 （-2 ^ 63） ~ （2 ^ 63 - 1)。

示例：

```
val byteValue: Byte = 100
val shortValue: Short = 1000
val intValue: Int = 100000
val longValue: Long = 10000000000L // 注意：long 类型数值后需要加 'L' 或 'l' 表示。
```

当程序直接给出一个较大的整数时，该整数默认可能就是 Long 型，如果将这个整数赋值给 Int、 Short 或 Byte 型，编译器将会报错。

Kotlin 的整型与 Java 不同， Kotlin 的整型不是基本类型，而是引用类型(大致相当于 Java 的包装类)， Byte、 Short、 Int、 Long 都继承了 Number 类型，因此它们都可调用方法、访问属性。

通过访问不同整数类型的 MIN_VALUE 和 MAX _VALUE 属性来获取对应类型的最大值和最小值。

Kotlin 是 null 安全的语言，因此 Byte、 Short、 Int、 Long 型变量都不能接受 null值，如果要存储 null值，则应该使用 Byte?、 Short、 Int?、 Long? 类型。

此外，整数类型添加 “?” 后缀与不加后缀还有一个**`区别`**：普通类型的整型变量将会映射成 Java 的基本类型；带 “?” 后缀的整型变量将会映射成基本类型的包装类。举例来说，Int 类型的变量将会映射成 Java 的 int 基本类型，但 Int? 类型的变量则会自动映射成 Java 的 Integer类型。

#### 2.2 浮点类型

浮点型数值可以包含小数部分，浮点型比整型的取值范围更大，可以存储比 Long 型更大或更小的数。

Kotlin 的浮点型有两种。

1. `Float`：表示 32 位的浮点型，当精度要求不高时可以使用此种类型。
2. `Double`：表示 64 位的双精度浮点型，当程序要求存储很大或者高精度的浮点数时使用这种类型。

示例：

```
val floatValue: Float = 3.14f
val doubleValue: Double = 3.14
```

Kotlin 的浮点数有两种表示形式。

1. 十进制数形式：这种形式就是简单的浮点数，例如 5.12、 512.0、 0.512 等。 浮点数必须包含一个小数点，否则会被当成整数类型处理。
2. 科学计数形式:例如 5.12e2 (即 5.12 x 10 ^ 2) 、 5.12E2 (也是 5.12 x 10 ^ 2)等。

只有浮点型的数值才可以使用科学计数形式表示。例如 51200 是一个 Int 型的值，但 512E2 则是浮点型的值。

如果声明一个常量或变量时没有指定数据类型，只是简单地指定了其初始值为浮点数，那 么 Kotlin 会自动判断该变量的类型为 Double。

#### 2.3 字符类型

字符型`Char`通常用于表示单个的字符，字符型值必须使用单引号（'）括起来。 Kotlin 语言使用 16 位的 Unicode 字符集作为编码方式，而 Unicode 被设计成支持世界上所有书面语言的字符，包括中文字符，因此 Kotlin 程序支持各种语言的字符。

字符型值有如下 3 种表示形式 。

1. 直接通过单个字符来指定字符型值，例如 'A'、 '9' 等。
2. 通过转义字符表示特殊字符型值，例如 '\n'、 '\t' 等。
3. 直接使用 Unicode值来表示字符型值，格式是'\uXXXX’，其中 XXXX 代表一个十六进 制的整数。

示例：

```
val letter: Char = 'A'
```

#### 2.4 布尔类型

布尔型只有一个 `Boolean `类型，用于表示逻辑上的“真”或“假”。与 Java 类似， Kotlin 的 Boolean类型的值只能是 `true `或` false`，不能用0或者非0来代表。其他数据类型的值也不能转换成 Boolean 类型。

示例：

```
val b : Boolean = false;
```

## 三. Dart 数据类型

Dart 语言常用的基本数据类型包括 Number、String、Boolean、List、Map

### 基本类型

#### 3.1 Number 类型

`Number `类型包括int和double都是最最基础的数字类型，int为整数，double为浮点数，double表示的是标准的64位浮点数，可使用`double.maxFinite`获取最大值。

* `int`整型，取值范围 -2 ^ 53 ~ 2 ^ 53 - 1
* `double` 浮点类型，64位长度的浮点型数据，即双精度浮点型。

示例：

```
int integerValue = 10; //int
double doubleValue = 3.14;//double
```

int 和 double 都是Number 类型的子类。

#### 3.2 布尔类型

Dart中布尔类型采用的是`bool`关键字来表示，与其他语言一样。

示例：

```
bool b = false;
print('bool b:$b');
```

#### 3.3 String 类型

Dart中只有`String`字符串类型，没有实际的char字符类型，但是可以使用`String.chodUnitAt(index)`获取到指定下标的UniCode编码值，然后可以通过`String.fromCharCode(int)`方法将UniCode值转换为String类型。与其他语言的 String 类型一样。

示例：

```
String message = 'Hello, Bob!';
```

### 集合类型

在集合类型中Dart和Java还是比较相似的，都是提供了List、Set和Map这三种集合类型。

## 四. ArkTs 数据类型

在 TypeScript 中，类型层次结构主要包括原始数据类型、对象类型、函数类型和高级类型等。

### 原始数据类型

- `number`：表示数值类型，包括整数和浮点数。
- `string`：表示字符串类型。
- `boolean`：表示布尔类型，值为 `true` 或 `false`。
- `null`：表示空值。
- `undefined`：表示未定义的值。
- `symbol`：表示唯一的、不可变的值，通常用于对象属性的键。
- `bigint`：表示任意精度的整数。

#### 1. number 类型

`number` 类型可以包含整数和浮点数，没有区分 `int` 和 `float` 类型。它表示所有数字类型，包括十进制、二进制、八进制和十六进制等。

- `NaN`（Not a Number）表示非数字，当进行非数学运算时可能会出现。
- `Infinity` 表示无穷大，例如除以 0 会返回 Infinity。

```
let decimal: number = 6; // 十进制
let hex: number = 0xf00d; // 十六进制
let binary: number = 0b1010; // 二进制
let octal: number = 0o744; // 八进制

let floatNumber: number = 3.14; // 浮点数

let notANumber: number = NaN;
let infinityNumber: number = Infinity;

```

#### 2. string 类型

`string` 类型用于表示字符串类型的数据。字符串是一组字符序列，可以包含字母、数字、符号和空格等字符。

```
let str: string = "Hello, Bob!";
```

##### 多行字符串

使用模板字符串（使用反引号 `）可以方便地创建多行字符串。

```
let multiLineString: string = `
  This is a 
  multi-line
  string in TypeScript.
`;
```

#### 3. boolean 类型

表示布尔类型，值为 `true` 或 `false`。

```
let isTrue: boolean = true;
let isFalse: boolean = false;
```

#### 4. null 空

`null` 表示一个特殊的值，用于表示变量的值为空。它是一种数据类型，只有一个值，即 `null`。

```
let nullValue: null = null;
```

##### null 类型的特点：

- `null` 是一个特殊的 JavaScript 值，表示一个空值或不存在的对象。
- 当一个变量被赋值为 `null`，表示该变量的值为空。
- 与 `undefined` 类似，`null` 也是 TypeScript 的一个原始数据类型。
- `null` 通常用于清空或标识一个变量未初始化的状态。

##### null 类型的使用场景：

- 初始化变量为 null，等待赋予实际值。
- 可以用于清空对象的引用，使其不再引用任何对象。

需要注意的是，使用 `null` 时要小心处理，避免因为未对变量进行判空检查而引发空指针异常。

```
let nullableValue: string | null = null; // 声明一个可为空的变量
if (nullableValue !== null) {
  // 进行 null 值的检查
  // 当 nullableValue 不为 null 时执行
} else {
  // 当 nullableValue 为 null 时执行
}
```

`null` 用于表示空值或不存在的对象，在 TypeScript 中可以帮助我们更清晰地处理变量值为空的情况。

#### 5. 未定义的值undefined 

`undefined` 表示一个变量尚未赋值或不存在的值。它是 TypeScript 的一个原始数据类型，用于表示缺少值的状态。

```
let undefinedValue: undefined = undefined;
```

##### undefined 类型的特点：

- `undefined` 是 JavaScript 的原始值，表示变量未定义或值未被赋值。
- 当声明的变量未被初始化时，默认值为 `undefined`。
- 在某些情况下，变量值可能会被显式赋值为 `undefined`。

##### undefined 类型的使用场景：

- 变量声明但未初始化时，其默认值为 `undefined`。
- 当一个函数没有显式返回值时，默认返回 `undefined`。
- 与 `null` 类似，`undefined` 通常用于检查变量是否已定义或赋值。

```
let x: number; // 未初始化的变量默认值为 undefined
if (typeof x === "undefined") {
  // 执行变量 x 未定义的情况下的逻辑
}
```

需要注意的是，`undefined` 可能会导致一些潜在的问题，因此在使用变量之前最好进行非空检查或判断，避免出现空指针异常或意外的错误。

#### 6. 唯一的、不可变的值symbol

`symbol` 是一种原始数据类型，表示唯一的、不可变的值。它是 ES6 中新增的数据类型，用于创建唯一的标识符。

##### 创建 Symbol：

使用 `Symbol()` 函数可以创建一个全局唯一的 Symbol 值。

```
let sym1: symbol = Symbol();
let sym2: symbol = Symbol("description"); // 可选的描述参数
```

##### Symbol 的特点：

- **唯一性**：每个通过 `Symbol()` 创建的 Symbol 值都是唯一的，即使描述相同也是不同的。
- **不可改变性**：Symbol 值是不可改变且唯一的，不能被修改。

##### 使用 Symbol：

Symbol 主要用于对象属性的键（key）。

```
const sym: symbol = Symbol();

let obj = {
  [sym]: "value",
};

console.log(obj[sym]); // 访问 Symbol 键的值
```

`Symbol `通常用于对象属性的键，作为对象属性的唯一标识符。它能够解决属性名冲突的问题，确保属性名的唯一性，常用于在对象中使用独特的键来存储特定的数据。

#### 7.任意精度的整数 bigint

`bigint` 是一种用于表示任意精度整数的新的原始数据类型。它可以表示比 `number` 类型更大范围的整数，不受固定范围的限制。

##### 创建 BigInt：

要创建一个 `bigint` 类型的值，可以在整数后面添加 `n` 后缀，或者使用 `BigInt()` 函数。

```
typescriptCopy code
let bigIntValue: bigint = 123n;
let anotherBigInt: bigint = BigInt("9007199254740992"); // 从字符串创建
```

##### BigInt 的特点：

- **表示大整数**：`bigint` 类型可以表示非常大的整数，远超过 JavaScript 中 `number` 类型能表示的范围。
- **精度高**：BigInt 可以精确表示和计算大整数，而不会出现精度丢失。

##### BigInt 的操作：

BigInt 支持大整数之间的数学运算，包括加法、减法、乘法和除法等操作。

```
typescriptCopy code
let a: bigint = 123n;
let b: bigint = 456n;

let sum: bigint = a + b;
let difference: bigint = a - b;
let product: bigint = a * b;
let quotient: bigint = a / b;
```

##### 注意事项：

- `bigint` 类型的值不能与 `number` 类型的值进行直接运算，需要先将其中一个转换为 `bigint` 类型。
- `bigint` 类型的值与 `number` 类型的值不能混合使用。

BigInt 提供了一种处理大整数的方式，在需要处理超出 `number` 类型范围的整数时，可以使用 `bigint` 类型来进行精确计算和表示。



[HarmonyOS开发：ArkTs常见数据类型（一）](https://juejin.cn/post/7303368502177775654)

[HarmonyOS开发：ArkTs常见数据类型（二）](https://juejin.cn/post/7304635710694670387)
