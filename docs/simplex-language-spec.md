# Simplex 语言规范

## 适用范围

目标环境为《Turing Complete》Windows 2.1.334 使用的 Simplex。本文提供编写关卡脚本及 agent 生成、审查代码所需的规则，使用本文不需要访问程序实现或研究资料。本文不是官方标准，不保证其他版本行为相同。

## 规则等级

- **已确认**：有原版编译执行结果或关卡用例支持，结论限于已覆盖的类型与输入。
- **语法识别**：前端能识别相应构造，不代表功能完整可用。
- **待确认**：证据不足，不得作为代码生成或兼容实现的默认假设。

正文给出已整理的语法规则；附有限制或“待确认”的条目优先于通用语法摘要。EBNF 描述结构，换行、命名和类型约束由各章节补充。

## 开发者与 agent 使用约定

1. 区分语言、标准前置库和游戏宿主接口。Input、Output、TestResult 和 UI 接口由宿主提供，不是语言关键字。
2. 生成代码时优先采用已确认形式；遇到待确认语义，保留显式转换与括号，并用目标编译器验证。
3. 不从其他语言类推优先级、默认返回值、数组复制、引用生命周期或整数扩宽规则。
4. 关卡接口和完整示例见[游戏关卡 Simplex 语法参考](simplex-game-level-reference.md)。

## 1. 词法结构

### 1.1 空白与换行

- 空格会被跳过。
- `\r` 会被跳过。
- `\n` 是有效 token，换行在 Simplex 中具有语法意义。
- **Tab 不是合法空白。** 实测源文件含 Tab 会触发
  `tokenizer.nim ... tok_unknown`。
- 缩进本身不是分程序结构的语法，代码块由 `{}` 界定；但 `switch` 的
  `case/default` 必须出现在后续行。

### 1.2 注释

支持：

```text
// 单行注释
/* 块注释 */
```

块注释支持嵌套：

```text
/* outer
   /* inner */
*/
```

注释可以出现在 token 之间。换行不会因为 `//` 以外的方式被吞掉。

### 1.3 标识符

通常形式：

```text
identifier := [A-Za-z_][A-Za-z0-9_]*
```

编译器还会根据位置区分：

- 变量名、函数名和字段名应使用小写开头。
- 类型名应为 PascalCase；反例如 `type point` 会报
  `Invalid type name: point`。
- 函数名首字母大写会报 `Invalid function name`。
- 常量通常写成 `#NAME`。

以下名字属于保留的声明关键字，不能直接作为普通标识符：

```text
let var const type dot unary binary call_conv extern
def index_read index_write
```

类型相关的特殊名字：

```text
None Struct Arr Enum
```

### 1.4 关键字与内建运算符

从 tokenizer 的运行时关键字哈希表恢复出以下 41 项：

```text
#if     else      extern    dot       unary
<<      def       let       >>        >=
import  true      false     <=        asm
continue elif     index_read          <
&       return    if        type      break
binary  const     ==        +         >
index_write       /         __reg_alloc_use
*       ^         var       %         ||
call_conv         switch    &&        while
|
```

注意：

- 静态条件关键字是 `#if`，不是 `static_if`。
- `case` 和 `default` 是上下文关键字，没有占用全局保留字。
- **没有 `for`。** `for` 会被当成普通标识符，原版编译器实测报
  `Variable for not in scope`。
- 没有 `class`、`interface`、`module` 等关键字。
- `__reg_alloc_use` 是内部保留项，关卡脚本没有已验证用法。

### 1.5 数值

已确认支持：

```text
0
123
1_000_000
0x10
0xff_ff
0b1010
-3
```

- 十进制支持下划线分隔。
- 支持 `0x` 十六进制。
- 支持 `0b` 二进制。
- 支持负整数 token。
- `077` 可编译，但不能据此确认它是八进制；`0o77` 实测不通过。
- 当前语言没有可直接使用的浮点字面量；`1.5` 会触发 tokenizer 错误。

与 `-` 有关的词法注意点：

```text
a - 2
```

安全；

```text
a-2
```

实测会被拆成 `a` 和负数字面量 `-2`，导致语句解析失败。建议二元减法两侧写空格。

### 1.6 字符串

三种引号形式：

```text
"ordinary string"
'ordinary or escaped string'
`template with {expression} interpolation`
```

普通字符串和单引号字符串均被编译器接受。单引号还用于外部符号名称：

```text
kernel32.'ExitProcess'(code)
```

反引号字符串支持插值：

```text
let text = `count={counter}`
```

tokenizer 中有专门处理的转义：

```text
\0 \a \t \r \v \f \n \e
```

具体未列出的转义行为仍需逐个验证。

### 1.7 标点

语法标点：

```text
( ) [ ] { } , : . ? $ #
```

分号不是合法语句分隔符。实测：

```text
var x=1; var y=2
```

会使编译器异常退出。语句之间使用换行。

## 2. 程序与语句

顶层和块内均可以出现语句。空行允许。

```ebnf
program     = { newline | statement } ;
block       = "{" { newline | statement } "}" ;
```

语句类别：

```ebnf
statement   = declaration
            | function_decl
            | type_decl
            | if_statement
            | while_statement
            | switch_statement
            | static_if
            | asm_statement
            | return_statement
            | break_statement
            | continue_statement
            | import_statement
            | block
            | assignment
            | expression_statement ;
```

### 2.1 变量、绑定和常量

```ebnf
declaration = "var" identifier "=" expression
            | "let" identifier "=" expression
            | "const" [ "#" ] identifier "=" expression ;
```

推荐使用标准常量写法：

```text
const #LIMIT = 3
let count = #LIMIT + 1
```

规则：

- `var` 可重新赋值。
- `let` 不能重新赋值。
- `const` 是编译期常量。
- `var` 和 `let` **不写类型注解**；`var x: Int = 1` 会报
  `var declarations use '='`。
- `var x`、`let x` 不允许省略初始化。
- `const` 可以出现在函数内部。
- 引用常量时通常也带 `#`，例如 `#SECOND`。

### 2.2 `if` / `elif` / `else`

```ebnf
if_statement = "if" expression block
               [ "elif" expression block ]*
               [ "else" block ] ;
```

示例：

```text
if x > 0 {
    x -= 1
} elif x == 0 {
    x = 1
} else {
    x = 2
}
```

格式限制：

- `{` 必须与 `if condition` 在同一行。
- `else` 或 `elif` 必须与前面的 `}` 在同一行：
  `} else {`、`} elif condition {`。
- 把 `else` 放到下一行会导致编译异常。

### 2.3 `while`

```ebnf
while_statement = "while" expression block ;
```

示例：

```text
while x < 10 {
    x += 1
}
```

`{` 必须与 `while condition` 在同一行。

### 2.4 `switch`

```ebnf
switch_statement = "switch" expression newline
                   case_clause { case_clause } ;

case_clause      = "case" expression block
                 | "default" block ;
```

示例：

```text
switch operation
    case 0 {
        branch = 1
    }
    case 3 {
        branch = 2
    }
    default {
        branch = 0
    }
```

规则：

- `switch` 本身不使用外层 `{}`。
- `case` 和 `default` 位于后续行。
- 缩进不是语法要求。
- `default` 单独写，不要写成 `case default`。

### 2.5 `return`、`break`、`continue`

```ebnf
return_statement   = "return" [ expression ] ;
break_statement    = "break" ;
continue_statement = "continue" ;
```

`return` 可单独使用，表示返回 `None` 或不带值。

### 2.6 赋值

```ebnf
assignment = assignable assignment_operator expression ;
```

赋值运算符：

```text
=  +=  -=  *=  /=  %=  &=  |=  ^=  <<=  >>=
```

普通赋值单独写成 `=`。

不支持：

- 多重赋值 `a, b = b, a`。
- `++`、`--`。
- 把赋值当普通表达式使用。

赋值目标可以是：

```text
variable
object.field
array[index]
```

## 3. 函数与方法

### 3.1 函数类别

```ebnf
function_decl =
    "def" identifier parameter_list type block
  | "dot" identifier parameter_list type block
  | "binary" binary_operator parameter_list type block
  | "unary" unary_operator parameter_list type block
  | "index_read" parameter_list type block
  | "index_write" parameter_list type block ;
```

所有函数都必须写返回类型；无返回值写 `None`。

普通函数：

```text
def add(a: Int, b: Int) Int {
    return a + b
}
```

函数体内可以定义嵌套函数：

```text
def outer(x: Int) Int {
    def inner(y: Int) Int {
        return y + 1
    }
    return inner(x)
}
```

### 3.2 参数

```ebnf
parameter_list = "(" [ parameter { "," parameter } [ "," ] ] ")" ;
parameter      = [ "$" ] identifier ":" type ;
```

尾逗号允许：

```text
def f(a: Int, b: Int,) Int {
    return a + b
}
```

不支持参数默认值：

```text
def f(a: Int = 1) Int
```

实测该写法不会被理解为默认参数。

### 3.3 `$` 引用参数

参数：

```text
def increment($value: Int) None {
    value += 1
}
```

调用：

```text
var count = 0
increment($count)
```

`$` 表示把调用者的可修改存储位置传给函数。方法调用也可写：

```text
$counter.tick()
```

### 3.4 方法

`dot` 定义通过 `.` 调用的方法：

```text
dot squared_length(p: Point) Int {
    return p.x * p.x + p.y * p.y
}

let value = point.squared_length()
```

点左侧的值作为第一个参数传入。

### 3.5 运算符重载

二元：

```text
binary +(a: Point, b: Point) Point {
    return Point {x: a.x + b.x, y: a.y + b.y}
}
```

复合赋值也可以用 `binary` 重载，此时第一个参数通常是 `$`：

```text
binary += ($a: Str, b: Str) None {
    a = a + b
}
```

一元：

```text
unary -(a: Fixed) Fixed {
    return Fixed (-Int a)
}
```

### 3.6 下标读写重载

```text
index_read (input: Str, index: Int) Char {
    return (Arr[Char] input)[index]
}

index_write (input: Str, index: Int, value: Char) None {
    (Arr[Char] input)[index] = value
}
```

`index_read` 和 `index_write` 后面没有函数名。

## 4. 类型声明与类型表达式

### 4.1 类型别名

```text
type Time U64
type File Int
type Ptr U64
type Str Arr[Char]
```

### 4.2 结构体

```text
type Point Struct {
    x: Int,
    y: Int,
}
```

字段形式：

```ebnf
struct_field = identifier ":" type ;
```

- 多个字段之间必须有逗号。
- 最后一个字段后的逗号可选。
- 仅靠换行分隔字段不合法。
- 类型名使用 PascalCase。

结构体字面量：

```text
var point = Point {x: 3, y: 4}
```

### 4.3 枚举

```text
type State Enum[idle, busy]
```

也接受换行分隔：

```text
type State Enum[
    idle,
    busy
]
```

枚举成员是裸标识符。没有确认支持 `member = integer` 形式的显式枚举值。

### 4.4 数组类型

标准形式：

```text
type Vector Arr[U64]
type Matrix Arr[Vector]
```

`Arr[T]` 是数组类型构造。

实测 `type A [U8]` 不合法；数组简写只出现在表达式中的转换：

```text
[U8] value
```

### 4.5 内置类型和类型变量

常见类型：

```text
Int
Bool
U1 U2 U8 U16 U32 U64 ...
S8 S16 S32 S64 ...
Arr[T]
None
```

泛型类型变量：

```text
@T
@Any
@Type
@Size
```

泛型不是写在函数名后声明，而是直接出现在参数和返回类型中：

```text
def identity(value: @T) @T {
    return value
}
```

调用时通常依靠实参推断。类型本身也可以作为值传入：

```text
array(<U32>, 16)
raw_cast(<U64>, value)
```

不支持传统的显式函数泛型调用写法 `id<Int>(3)`；实测会把 `id` 当普通标识符并失败。

## 5. 表达式

### 5.1 运算优先级

从低到高：

| 优先级 | 运算符 | 结合性 |
|---|---|---|
| 最低 | `? :` 条件表达式 | 建议使用括号 |
| 5 | `\|\|` | 左结合 |
| 6 | `&&` | 左结合 |
| 7 | `<` `>` `<=` `>=` `==` `!=` | 左结合 |
| 8 | `+` `-` `\|` `^` `&` | **同一级，左结合** |
| 9 | `*` `/` `%` `<<` `>>` | **同一级，左结合** |
| 最高 | 一元、后缀、类型转换 | — |

这组优先级与 C/C++不完全相同。例如：

```text
1 + 2 * 3
```

结果为 `7`；

```text
1 << 1 + 1
```

按 `(1 << 1) + 1` 得到 `3`；

```text
2 | 4 & 1
```

因为 `|` 和 `&`同级且左结合，实际为 `(2 | 4) & 1`，结果为 `0`。

空元组无符号是关键字 `None`，不是 Python 式空元组。

### 5.2 一元表达式

```text
!value
~value
-value
+value
$value
```

其中：

- `!` 是逻辑非。
- `~` 是按位非。
- `-` 是数值取负；一元 `+` 仅见于语法识别记录，可用性待确认，不列为推荐写法。
- `$` 表示可变引用/借用。

### 5.3 条件表达式

```text
condition ? true_value : false_value
```

示例：

```text
let ready = counter == 3 ? true : false
```

原版对照记录表明，假分支按前缀表达式读取：

```text
flag ? n + 1 : n - 1
```

等价于 `(flag ? n + 1 : n) - 1`。需要完整分支运算时，必须显式写成：

```text
flag ? (n + 1) : (n - 1)
```

### 5.4 调用、成员和下标

```text
function(args)
value[index]
value.field
value.method(args)
library.'external_symbol'(args)
```

普通点调用与外部符号调用都使用 `.`，但外部符号用单引号包围。

### 5.5 类型转换

已确认形式：

```text
Type expression
<Type> expression
[ElementType] expression
Arr[ElementType] expression
```

示例：

```text
U8 300
Int byte_value
<Cstring> result
[U8] text
Arr[U8] value
```

类型值本身可写成：

```text
<U32>
<@Type>
```

### 5.6 数组字面量

```text
[U32 1, 2, 3]
[1, 2, 3]
```

第一个元素可以带统一元素类型。空数组 `[]` 实测会使编译器异常退出。

### 5.7 结构体字面量

```text
Point {
    x: 3,
    y: 4,
}
```

也可以单行：

```text
Point {x: 3, y: 4}
```

### 5.8 模板字符串

```text
`count={counter}`
```

`{...}` 中是普通 Simplex 表达式。

## 6. 编译期条件

语法：

```text
#if expression {
    ...
} else {
    ...
}
```

示例：

```text
#if false {
    let never_compiled = UnknownType 1
}
```

该代码可以通过，因为假分支在语义分析之前就被剔除。`#if` 不是运行时 `if`。

## 7. 外部函数与调用约定

### 7.1 `call_conv`

调用约定声明示例：

```text
call_conv x86_64 windows_x64 (
    arguments: [rcx, rdx, r8, r9],
    returns: [rax],
    save_restore: [rax, rcx, rdx, r8, r9, r10, r11],
    stack_alignment: 16,
    shadow_space: 40,
)
```

```ebnf
call_conv_decl = "call_conv" target identifier
                 "(" call_conv_fields ")" ;
```

已见字段：

```text
arguments
returns
save_restore
stack_alignment
shadow_space
```

### 7.2 `extern`

```text
extern windows_x64 kernel32
```

它把库名加入外部符号作用域，之后可使用：

```text
kernel32.'ExitProcess'(code)
kernel32.'_write'(text, length)
```

语法：

```ebnf
extern_decl = "extern" identifier identifier ;
external_call = identifier "." string "(" arguments ")" ;
```

## 8. 内联汇编

### 8.1 基本形式

```text
asm x86_64 {
    ...
}

asm aarch64 {
    ...
}
```

也接受：

```text
asm x86_64 windows {
    ...
}
```

### 8.2 汇编变量别名

```text
define remainder rax
define idx r10
```

### 8.3 标签

```text
loop:
    ...
```

### 8.4 Simplex 变量作为汇编操作数

```text
mov $result, rax
```

`$result` 让编译器把 Simplex 局部变量分配到操作数。

### 8.5 常规指令

```text
mov rcx, 0
idiv rcx
rdtsc
mov rcx, 32
shl rdx, cl
or rax, rdx
```

汇编语法依赖目标平台；完整寄存器/指令集合属于后端接口，不建议在没有目标说明时写成通用文档。

当前 x86-64 目标内置的寄存器名表包括：

```text
rax eax ax al
rcx ecx cx cl
rdx edx dx dl
rbx ebx bx bl
rsp esp sp spl
rbp ebp bp bpl
rsi esi si sil
rdi edi di dil
r8...r15 及对应的 d/w/b 形式
```

当前 x86-64 助记符表包含常见指令和别名，例如：

```text
mov movzx lzcnt bsf bsr rdtsc ret push pop lea
setl setle setg setge seta setae setb setbe sete setne
cmove cmovne cmova cmovae cmovg cmovge cmovz
jz jnz je jne jmp test cmp sub imul add and or xor not neg
shr sar shl rol ror idiv div cqo syscall call
rep movsb rep stosb
```

## 9. `import`

语法表面形式：

```text
import "helper.si"
```

但在 2.1.334 的独立编译实验中，`import` 到达
`front_end.nim(3980, 7)` 的未实现断言并以异常结束。

因此应区分：

- **语法被识别**；
- **当前版本没有可用的模块加载实现**。

不要把它写成已经可用的多文件导入功能。

## 10. 已验证不支持或不可依赖的形式

| 写法 | 实际结果 |
|---|---|
| `for i in 0..4 {}` | `for` 是普通标识符，不支持的循环 |
| `static_if` | 应为 `#if` |
| 分号隔开语句 | tokenizer 异常 |
| Tab 缩进 | tokenizer 异常 |
| 参数默认值 | 不支持 |
| `a, b = b, a` | 不支持 |
| `a++`、`++a` | 不支持 |
| 空数组 `[]` | 实测异常退出 |
| `Enum[a = 1]` | 实测异常退出 |
| `type A [U8]` | 不支持 |
| `class`、`interface` | 没有对应语法 |
| `id<Int>(3)` | 不支持这种显式泛型调用 |
| `var x: Int = 1` | 不支持，变量用 `=` 推断 |
| 浮点字面量 `1.5` | tokenizer 异常 |

## 11. 一份紧凑的 EBNF 摘要

```ebnf
program = { newline | statement } ;

declaration =
    "var" identifier "=" expression
  | "let" identifier "=" expression
  | "const" [ "#" ] identifier "=" expression ;

type_decl =
    "type" type_identifier type
  | "type" type_identifier "Struct" "{" fields "}"
  | "type" type_identifier "Enum" "[" enum_items "]" ;

function_decl =
    "def" identifier params type block
  | "dot" identifier params type block
  | "binary" operator params type block
  | "unary" operator params type block
  | "index_read" params type block
  | "index_write" params type block ;

params = "(" [ param { "," param } [ "," ] ] ")" ;
param  = [ "$" ] identifier ":" type ;

if_statement =
    "if" expression block
    { "elif" expression block }
    [ "else" block ] ;

while_statement = "while" expression block ;

switch_statement =
    "switch" expression newline
    ( "case" expression block | "default" block ) { ... } ;

static_if = "#if" expression block [ "else" block ] ;

expression = conditional ;

conditional =
    logical_or [ "?" expression ":" prefix_expression ] ;

logical_or  = logical_and { "||" logical_and } ;
logical_and = comparison { "&&" comparison } ;
comparison  = add_bitwise { comparison_op add_bitwise } ;
add_bitwise = mul_shift { add_bitwise_op mul_shift } ;
mul_shift   = unary { mul_shift_op unary } ;

unary = ( "!" | "~" | "-" | "+" | "$" ) unary
      | postfix ;

postfix =
    primary
    { "(" arguments ")"
    | "[" expression "]"
    | "." identifier
    | "." string
    } ;
```

## 12. 已有对照支持的标量语义

以下规则由已有原版对照记录支持，适用于表中指定的 Int / Bool 场景，不推广为所有位宽类型的完整规则。

| 项目 | 已确认的 Int / Bool 行为 |
| --- | --- |
| Int | 有符号 64 位机器字 |
| Int 加减乘 | 按机器字回绕 |
| Int 除法 | 向零截断 |
| Int 余数 | 与被除数同号 |
| Int 右移 | 逻辑右移；不能以 C 的有符号算术右移推断 |
| Int 移位量 | 使用低六位 |
| 逻辑运算 | 对规范 Bool 使用；条件应显式比较，不把整数隐式当条件 |
| 除零、最小 Int 除以 -1 | 未定义兼容结果，不可依赖 |
| Int 转 Bool | 原版存在依上下文变化的非规范 Bool 行为，不能简单归纳为取最低位 |

推荐字面量采用小写 `0x` / `0b`。负的进制数写 `-(0xF)`、`-(0b101)`；原版对 `-0xF` 和大写前缀存在特殊解析行为，不按常规语义承诺。

位宽转换在真实关卡中广泛使用，但完整的扩宽、符号扩展、截断及混合类型推断仍待验证。需要宽结果时先扩宽操作数，再运算；不要假设窄类型运算的中间结果自动扩宽。


## 13. 仍需作为“语义规则”单独验证的部分

以下已经超出纯粹语法，但仍会影响写规范：

- 隐式整数扩宽、截断和符号转换条件。
- 同名函数重载的精确匹配顺序。
- `@T`、`@Any`、`@Type`、`@Size` 的实例化约束。
- `let` 对结构体、数组和引用的可变性边界。
- `$` 引用参数的生命周期和别名规则。
- 常量表达式允许使用哪些内建函数。
- 汇编操作数的完整合法组合。
- `call_conv` 各字段是否都可省略。

这些应该写在文档的“类型系统”和“语义”章节，而不是混入词法或语句产生式。


## 14. 宿主环境与兼容实现

原版关卡使用数组、结构体、字符串、全局状态、位宽整数和宿主回调。实现兼容工具时，应分别验证语言接受性、运行语义和宿主接口；任一独立工具的功能限制都不能用于定义原版语言。

原版关卡存在返回 TestResult 的函数执行到末尾而没有显式 return 的写法。这不构成所有返回类型的默认值规则；新代码应显式表达返回结果，具体关卡协议见配套参考。

## 15. 标准库与类型语义补充



已有原版执行记录确认：`U8 300` 为 44；支持按 Int/Str 参数类型区分的同名函数重载；基础泛型 identity、dot 方法、引用参数和结构体运算符重载有编译执行证据。

前置库定义 `type Str Arr[Char]`，以及 Time、Seed、File、Ptr 等类型；它们不应全部列作词法关键字。

基础整数数组支持 len、clone、sort，但未验证嵌套对象深拷贝；字段复合赋值存在已记录的问题，完整赋值可作为规避方式。array、str、bin、hex、random 等能力的存在不等于它们所有类型组合、边界和分布均已验证。

64 位 asr、rol、ror 有执行通过的样例；部分窄位宽样例不符合预期，不能推广。U128 声明被接受不构成完整 128 位算术支持的证据。Float/F32/F64 在目标测试环境中是未知类型。

原版语法摘要与实验结论出现差异时，应优先采用有具体输入输出的原版实验，并保留差异说明。本规范中的一元 + 仅保留识别线索，不列为已验证可用操作。


相关参考：[关卡编辑器错误类型与含义表](level-editor-error-reference.md)。
