# 游戏关卡 Simplex 语法参考

## 适用范围与阅读顺序

本文面向《Turing Complete》Windows 2.1.334 的关卡脚本开发。本文直接提供编写关卡所需的语法和接口用法，不要求访问程序实现。

先阅读[Simplex 语言规范](simplex-language-spec.md)，再按任务选择输入生成、判题、时序状态、UI 或外设章节。示例均注明依赖的字段或上下文；标为片段的代码不能单独作为完整关卡使用。

**接口约束：** 调用宿主函数前必须确认程序提供该接口；实现回调前必须确认宿主调用该签名。未列出的参数、返回类型和生命周期不能由名称推断。

## 1. 三层概念

| 层次 | 内容 | 由谁提供 |
| --- | --- | --- |
| 语言语法 | def、var、Struct、switch、前缀转换、插值 | 原版编译器 |
| 标准前置库或内建能力 | Arr、Str、array、len、random 等 | 原版编译环境；随目标环境提供 |
| 关卡宿主接口 | Input、Output、TestResult、UI 和 RAM 接口 | 游戏生成的运行环境 |

Simplex 是关卡脚本与仿真的实现语言；玩家在 Overture、Symphony 中编写的汇编是另一种语言。`test.si` 通常不是能直接运行的独立程序。

`meta.txt` 负责关卡配置、描述与解锁；`ui.txt` 定义显示布局；`test.si` 生成输入、判定输出并更新界面。不要把这些资源格式全部当作 Simplex 源码。

## 2. 最常用的语法速查

| 需求 | 写法 | 注意点 |
| --- | --- | --- |
| 可变状态 | `var count = 0` | 初始化时推断类型 |
| 不重绑的局部值 | `let expected = 3` | 不等于所有内部数据深度不可变 |
| 编译期表 | `const #EXPECTED = [U1 0, 0, 0, 1]` | 引用保留 # |
| 指定整数位宽 | `U8 value` | 复杂表达式写 `U8 (a + b)` |
| 函数 | `def name(cycle: Int) Input { ... }` | 返回类型在括号后 |
| 构造输入 | `Input {input: U2 cycle}` | 字段与板上端口一致 |
| 条件 | `if condition { ... } else { ... }` | else 与前一个右括号同行 |
| 循环 | `while i < count { ... }` | 无 for 循环 |
| 数组 | `[U1 0, 1]`、`array(<U32>, 16)` | 从 0 开始索引；避免 [] |
| 长度 | `items.len()` | 点调用形式 |
| 插值 | `` `value={value}` `` | 反引号，非普通引号 |
| 引用修改 | `helper($state)` | 形参也写 `$state: Type` |

使用空格缩进，不用 Tab；语句用换行分隔，不用分号。二元减法写 `a - 2`，避免 `a-2` 被拆成负数 token。

## 3. 组合逻辑：输入与真值表

以下基于原版 AND 关卡，补上显式 `return pass` 方便阅读。要求当前起始板生成 `Input.input: U2`，并提供 `Output.output` 和 `output_is_z`。不是任意关卡都能直接粘贴。

```text
const #EXPECTED = [U1 0, 0, 0, 1]

def get_input(cycle: Int) Input {
    return Input {input: U2 cycle}
}

def check_output(cycle: Int, input: Input, output: Output) TestResult {
    if output.output_is_z {
        return fail
    }

    if output.output != #EXPECTED[cycle] {
        return fail
    }

    if cycle == #EXPECTED.len() - 1 {
        return win
    }

    return pass
}
```

这里把两位组合打包到一个输入字段，不应擅自改成 `input.a` 和 `input.b`。字段名来自实际板结构，不来自显示标题。

| 返回值 | 关卡含义 |
| --- | --- |
| pass | 当前检查通过，继续测试 |
| fail | 当前测试失败 |
| win | 整个关卡完成 |

原版许多 check_output 在普通周期没有显式 return。沿用原作的事实不能推广为所有返回类型的默认值规则；新示例建议明确返回 pass。`exit(0)` 是独立实验退出进程的方式，不是关卡通关方式。

真值表索引必须落在范围内；上述模板只对应四种输入的测试流程，不能不检查宿主周期范围就扩展运行。

## 4. 字节运算：先扩宽，再计算

原版 对应示例关卡 的核心写法：

```text
let value = Int (U64 input.a + U64 input.b + U64 input.carry_in)
```

`U64 input.a` 是前缀类型转换，不是函数调用。先把操作数扩宽，避免依赖窄位宽中间结果保留进位。

该关卡的判定将低八位与进位分别检查：

```text
var sum = Int input.a + Int input.b + Int input.carry_in
var car = Int (U64 (sum > 0xff))

if Int output.output != (Int sum & 0xff) {
    return fail
}

if Int output.carry_out != car {
    return fail
}
```

这是依赖当前字段的片段。不能把该转换例子概括成任意 Int 与 Bool 转换的完整规则。

**优先级与 C 不同：** `+ - | ^ &` 同级，`* / % << >>` 同级，各级左结合。掩码、移位和求和混合时主动加括号，例如 `((value >> 8) & 0xff)`。

条件表达式的假分支有特殊读取规则，复杂分支写 `flag ? (a + 1) : (b - 1)`。

## 5. Z 状态与错误提示

原版脚本常先检查输出是否为 Z：

```text
if output.output_is_z {
    set_error((31337_70376380014778, `Output should never be Z state`))
    return fail
}
```

Z 表示没有驱动值，不能当普通数值 0 参与比较。`output_is_z` 是当前宿主结构体中的字段，不是所有变量的通用属性；其他输出对应什么标志，应核对生成定义。

上面的 `(数字ID, 文本)` 是游戏本地化文本用法，原版关卡中存在此用法，不应据此推导通用元组语法。不要为新文案复用不相干的原版 ID；沿用已有 ID 时须保持原意。

普通调试信息可使用宿主调用形式 `set_error(message)`；其可接受的具体类型以程序提供的接口说明和检查结果为准。

## 6. 时序逻辑：期望状态与当前输出

原版 Counter 使用两份状态：

```text
var last_mem = 0
var new_mem = 0
```

在 get_input 中先保存上一状态，再算下一状态：

```text
last_mem = new_mem

if overwrite == 0 {
    new_mem = (last_mem + 1) % 256
} else {
    new_mem = num
}
```

check_output 比较的是 `last_mem`。这表明该关的观察时机要求输出对应更新前状态；不能照直觉改成比较 new_mem。

全局状态适合参考模型和交互进度，但重试、重新载入、切换测试时如何初始化取决于宿主执行链。不要仅凭“顶层 var”推断所有生命周期。确定性用例应覆盖首周期、覆盖、连续计数和回绕；随机测试作为补充。

## 7. UI 更新

原版 Adding Bytes 有以下调用：

```text
ui_set_text("table[0][1]", `{input.a}`)
ui_set_text("table[3][1]", `{value}`)
```

字符串路径指向 ui.txt 中的元素或表格位置。脚本变量名与 UI 元素 ID 是两套名称。

`[F]`、`[T]`、`[Z]`、`[right]` 等是显示文本标记，既不是 Simplex 关键字，也不会自动完成电路判定。UI 显示期望值和当前值时，应分别使用参考模型与实际输出。

其他 UI 接口包括 ui_set_hidden、ui_set_position、ui_set_color；具体参数仅使用本参考明确给出的形式，或以程序提供的接口帮助为准；缺少说明时不要猜测。

## 8. switch 与指令参考模型

Simplex 的 switch 没有外层大括号：

```text
switch operation
    case 0 {
        value = 1
    }
    case 1 {
        value = 2
    }
    default {
        value = 0
    }
```

case/default 位于后续行。该形式适合操作码解码、输入事件分派和控制信号生成；不要照搬 C 的冒号和外层花括号。

原版 Symphony IO Devices 使用 counter、registers、ram、ssd 等脚本变量构建参考模型，例如：

```text
var counter = U32 0
var registers = array(<U32>, 16)
```

其 `retire_instruction(...)` 是该脚本定义的辅助函数，不是通用回调。参考模型计算期望状态，玩家电路执行后再由检查逻辑比较，两者不能混为一体。

## 9. 其他真实回调形式

| 原版位置 | 签名 | 作用 |
| --- | --- | --- |
| Tower of Hanoi | `def on_manual_input(input: Int) None` | 手动输入事件 |
| Tower of Hanoi | `def get_input_switched() Int` | 该关输入读取形式 |
| Tower of Hanoi | `def check_output_switched(output: Int) TestResult` | 该关输出检查形式 |
| Symphony IO Devices | `def on_ui_update(input: Input, output: Output) None` | UI 更新 |

这些是对应关卡中的已见签名，不表示任意关卡都调用全部回调。Tower 中 redraw、level_input 是作者自定义帮助函数，也不应误列为宿主固定 API。

## 10. RAM 与外设接口

RAM 访问示例：

```text
let pixel = get_ram_value("Framebuffer", address, <U8>)
```

`<U8>` 是类型值；Framebuffer 是元件标签，不是操作系统地址。读取地址范围、端口时序和连接对象必须由当前关卡定义。

可检索的宿主接口名还包括 get_cycle、get_test、get_component_count、set_ram_value、get_load_port_input、get_screen_settings、get_screen_connection。此处是名称索引，不构造未经核对的参数表。

不要用底层 Ptr/load/store 的宿主内存操作替代普通关卡 RAM 接口。

## 11. 常见问题

| 问题 | 应检查什么 |
| --- | --- |
| var 类型注解报错 | 写 `var n = U8 0`，不要写 `var n: U8 = 0` |
| else 解析失败 | 保持 `} else {` 同行 |
| switch 无法解析 | 去掉 switch 外层花括号，case 放后续行 |
| 算术结果意外 | 核对混合优先级、转换范围和运算前位宽 |
| 第一周期错误 | 检查初始状态以及比较更新前还是更新后状态 |
| 字段不存在 | 核对编辑器中的输入输出端口配置，不照搬别关字段 |
| 数值为零仍失败 | 检查 Z 状态，Z 不等于 0 |
| UI 不更新 | 核对 ui.txt 元素 ID、路径和回调时机 |
| import 断言 | 当前原版实测未实现可用导入；可在构建阶段合并源码 |
| 字段 += 报类型错误 | 实测存在此问题，可改成 `obj.field = obj.field + 1` |
| 外部工具拒绝脚本 | 分别检查语言支持与宿主声明是否完整 |

## 12. 代码生成与审查清单

1. 确认目标版本、关卡类型及实际 Input/Output 字段。
2. 使用空格和换行；保持控制块的左括号与条件同行。
3. 位运算写明位宽，混合运算加括号，宽结果先扩宽操作数。
4. 区分当前状态与下一状态，确认宿主观察输出的时机。
5. 检查 Z 状态、失败路径、普通步骤通过和最终通关条件。
6. 确保数组索引、周期和 RAM 地址在预期范围内。
7. 核对 UI ID、元件标签和回调签名，不补造宿主 API。
8. 在目标游戏环境验证编译与行为；脚本检查通过后仍需运行关卡验收。


相关参考：[关卡编辑器错误类型与含义表](level-editor-error-reference.md)。
