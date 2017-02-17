Dart 语言简易教程(一)： http://www.jianshu.com/p/8a62b1a2fd75
Dart 语言简易教程(二)： http://www.jianshu.com/p/b2153a32dd8b
Dart 语言简易教程(三)： http://www.jianshu.com/p/6d2495a0d3d7

# Dart 语言简易教程(四)
## 操作符
Dart 支持各种类型的操作符，并且其中的一些操作符还能进行重载。
支持的完整操作如下：

|Description	| Operator |
|--- | --- |
|unary postfix	| expr++    expr--    ()    []    .    ?.|
|unary prefix	| -expr    !expr    ~expr    ++expr    --expr   |
|multiplicative	| *    /    %  ~/ |
|additive	| +    - |
|shift	| <<    >> |
|bitwise AND	| & |
|mbitwise XOR	| ^ |
|bitwise OR	| \| |
|relational and type test	| >=    >    <=    <    as    is    is! |
|equality	| ==    !=   |
|logical AND	| && |
|logical OR	| \|| |
|if null	| ?? |
|conditional	| expr1 ? expr2 : expr3 |
|cascade	| .. |
|assignment	| =    *=    /=    ~/=    %=    +=    -=    <<=    >>=    &=    ^=    \|=    ??= |

在上表中，操作符的优先级依次降低。就是排在最上面的一行优先级最高，最后的一行优先级越低。

### 算术操作符
Dart 支持的基本算术操作符如下表所示：

Operator |	Meaning
--- | ---
+	| Add
–	| Subtract
\-expr	| Unary minus, also known as negation (reverse the sign of the expression)
\*	| Multiply
/	| Divide
~/	| Divide, returning an integer result
%	| Get the remainder of an integer division (modulo)

实例：
```Dart
assert(2 + 3 == 5);
assert(2 - 3 == -1);
assert(2 * 3 == 6);
assert(5 / 2 == 2.5);   // Result is a double
assert(5 ~/ 2 == 2);    // Result is an integer
assert(5 % 2 == 1);     // Remainder

print('5/2 = ${5~/2} r ${5%2}'); // 5/2 = 2 r 1
```

Dart 同时也支持词前及词后的自增自减操作。

Operator	| Meaning
--- | ---
++var	| var = var + 1 (expression value is var + 1)
var++	| var = var + 1 (expression value is var)
--var	| var = var – 1 (expression value is var – 1)
var--	| var = var – 1 (expression value is var)

实例：
```Dart
var a, b;

a = 0;
b = ++a;        // Increment a before b gets its value.
assert(a == b); // 1 == 1

a = 0;
b = a++;        // Increment a AFTER b gets its value.
assert(a != b); // 1 != 0

a = 0;
b = --a;        // Decrement a before b gets its value.
assert(a == b); // -1 == -1

a = 0;
b = a--;        // Decrement a AFTER b gets its value.
assert(a != b); // -1 != 0
```

### 关系操作符
Dart 支持的关系操作符列表：
Operator	| Meaning
--- | ---
==	| Equal;
!=	| Not equal
>	| Greater than
<	| Less than
>=	| Greater than or equal to
<=	| Less than or equal to

> 假如要比对`x ` 与`y `是否相等，`== `操作符的工作流程如下：
> - 如果`x ` 或者 `y ` 都是`null ` 类型，比较的结果是`true `。
> - 如果`x ` 或者 `y ` 只有一个是`null ` 类型，比较的结果是`false `。
> - 返回调用`x.==(y) `的结果作为`== ` 操作的结果。

### 类型比较操作符
Dart 支持在运行时比较对象的类型，支持的操作如下：


Operator	| Meaning
--- | ---
as	| Typecast
is	| True if the object has the specified type
is!	| False if the object has the specified type

`is `操作，用来比较前操作数是否是后操作数的对象。
`as `操作，用来将前操作数指定为后操作数的类型。

### 指定操作符
`= `操作符，将后操作数的值赋给前操作数。
`??= `操作符，如果前操作数是`null `类型，则将后操作数赋值给前操作数；如果前操作数不等于`null `,则保持前操作数的值发生变化。

#### 组合指定操作符
Dart 支持将算术操作符与`= `组合起来实现更复杂的功能。
- | - | - | - | - | -
--- | --- | --- | --- | --- | ---
= |	–= |	/= |	%= |	>>= |	^=
+= |	*= |	~/= |  <<= |	&=	| =

下表演示了这是如何工作的：
 | Compound assignment |	Equivalent expression
--- | --- | ---
For an operator op:	| a op= b	| a = a op b
Example:	| a += b	| a = a + b

实例：
```Dart
var a = 2;           // Assign using =
a *= 3;              // Assign and multiply: a = a * 3
assert(a == 6);
```

### 逻辑操作符
Dart 支持的逻辑操作符如下表：
Operator |	Meaning
--- | ---
!expr	| inverts the following expression (changes false to true, and vice versa)
||	| logical OR
&&	| logical AND

逻辑操作符实例：
```Dart
if (!done && (col == 0 || col == 3)) {
  // ...Do something...
}
```

### 位操作符及位移操作符
Dart 针对整数(int 类型)支持位操作符及位移操作符。

Operator |	Meaning
--- | ---
&	|	AND
|	|	OR
^	|	XOR
~expr	|	Unary bitwise complement (0s become 1s; 1s become 0s)
<<	|	Shift left
>>	|	Shift right

位操作符及位移操作符实例：
```Dart
final value = 0x22;
final bitmask = 0x0f;

assert((value & bitmask)  == 0x02);  // AND
assert((value & ~bitmask) == 0x20);  // AND NOT
assert((value | bitmask)  == 0x2f);  // OR
assert((value ^ bitmask)  == 0x2d);  // XOR
assert((value << 4)       == 0x220); // Shift left
assert((value >> 4)       == 0x02);  // Shift right
```

## 条件表达式
Dart 支持条件表达式，同时为了减少代码，也提供了简化的操作符。
Dart中有两种方式简化类似其它语言的if-else 功能。
- 使用` ? : ` 表达式。
使用如下的表达式：
```Dart
condition ? expr1 : expr2
```
`condition `的值为true, 则返回结果为` expr1 `；反之则返回` expr2 `。

- 使用` ?? ` 表达式。
使用如下的表达式：
```Dart
expr1 ?? expr2
```
如果` expr1 `的值为non-null，则返回结果为` expr1 `；反之则返回` expr2 `。

下面的例子演示了：将Dart 的`? : `操作符转化为'if-else' 的版本：
```Dart
// Slightly longer version uses ?: operator.
String toString() => msg == null ? super.toString() : msg;

// Very long version uses if-else statement.
String toString() {
  if (msg == null) {
    return super.toString();
  } else {
    return msg;
  }
}
```

### 级联操作符(..)
通过级联操作符(..)，可以连续的操作同一对象，达到减少中间变量，减少代码的目的。
如下面的例子：
```Dart
querySelector('#button') // Get an object.
  ..text = 'Confirm'   // Use its members.
  ..classes.add('important')
  ..onClick.listen((e) => window.alert('Confirmed!'));
```
下面的代码与上面的例子实现功能完全相同：
```Dart
var button = querySelector('#button');
button.text = 'Confirm';
button.classes.add('important');
button.onClick.listen((e) => window.alert('Confirmed!'));
```

另一段例子：
```Dart
final addressBook = (new AddressBookBuilder()
      ..name = 'jenny'
      ..email = 'jenny@example.com'
      ..phone = (new PhoneNumberBuilder()
            ..number = '415-555-0100'
            ..label = 'home')
          .build())
    .build();
```

### 其它的一些操作符

Operator	| Name	| Meaning
--- | --- | ---
()	| Function application	| Represents a function call
[]	| List access	| Refers to the value at the specified index in the list
.	| Member access	| Refers to a property of an expression; example: foo.bar selects property bar from expression foo
?.	| Conditional member access	| Like ., but the leftmost operand can be null; example: foo?.bar selects property bar from expression foo unless foo is null (in which case the value of foo?.bar is null)
