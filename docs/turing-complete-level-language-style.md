# Turing Complete 关卡说明语言风格规则

## 适用范围与依据

依据《Turing Complete》原版关卡中的关卡描述，归纳其叙述口吻、任务表达、教学组织和术语使用规则。关卡描述中的导师叙事与技术任务说明是主要分析对象；提示、界面文字和通关反馈仅作为辅助对照。

本文使用原版关卡描述的英文文本进行分析。下文中文例句为写作示例，不是官方译文。原作观察和建议规范分开说明；建议字数不是原作统计结果。

附录按关卡名称收录描述文本，供直接阅读和对照；排列不代表游戏解锁顺序。

## 一、整体声音

**外星导师主持的计算机实验：故事里一本正经地调侃人类，任务里准确、直接地解释机器行为。**

- 导师称玩家为“你”，以“我们”代表测试方；涉及共建机器时也会用共同完成任务的口吻。
- 叙事把人类放进地球物种的比较框架：大象的脑容量、海洋动物的手臂、海豚的鳍、狗的服从性，都能成为看似科学的评价依据。
- 幽默来自严肃语气与荒诞评价的落差，不依赖感叹号、网络梗或夸张卖萌。
- 技术正文多用 Create、Build、Output、Read、Add、Set、Implement 等动作起句。定义行为优先于渲染挑战。
- 长度服从教学需求。已有知识只写一句任务；首次出现的移位、总线、汇编、指令编码则允许多段解释、图表和代码。

## 二、说明的组织规则

| 层次 | 原作做法 | 中文写作规则 |
| --- | --- | --- |
| 故事引子 | 部分关卡先谈测试进展或其他物种 | 可选；独立成段，不把笑话嵌进判定条件 |
| 本关目标 | 直接说明要构建或计算什么 | 用“构建……”“读取……并输出……”等动作句 |
| 行为条件 | When / If / Otherwise；明确输入和输出 | 写清触发条件、对应动作和其他情况 |
| 新概念 | 先解释作用，再给术语、例子或布局 | 仅补充完成当前任务所需知识 |
| 操作提示 | 指定按钮、工具、组件和点击方式 | 界面名称与游戏实际名称一致 |
| 提示 | 使用 [tip]；可能提供解题方向或降低挫败感 | 与必需规则分开；不把验收条件藏进提示 |
| 通关后话语 | 解释等价关系、介绍解锁或推进故事 | 放在完成后，不重复整段题目 |

这不是每关必须套用的固定顺序。Byte Constant 只有“始终输出 164”的任务；Symphony RAM 则包含内存端口、编码、操作语义和依赖顺序。

## 三、句式与信息精度

1. **一个动作句承担一个主要要求。** 先写目标，再追加条件、例外和操作。复杂关卡按语义拆段，避免把所有内容塞进一句。
2. **输入输出可辨认。** 区分第一、第二输入，数据与控制位，地址与值，源与目的寄存器。避免只写“处理它”。
3. **条件可检查。** 例如选择器为 [F] 时输出 A，否则输出 B；不要写成“根据情况选择正确输出”。
4. **显式写影响结果的例外。** Divide 对除数为 0 的结果作出规定；Modulo 对相同情况给出不同规定。不能用常识替代本关约定。
5. **时序关卡写时间。** 交代每周期、保存、覆盖、读取先后等。不能把组合逻辑的即时条件句直接套在记忆电路上。
6. **位宽和数值解释属于规则。** 8 位、32 位、有符号、无符号、最高有效位等信息必须保留。
7. **概念通过例子落地。** 移位给位序列，汇编给指令，递归给伪代码。示例应解释正文，不应另立隐藏规则。
8. **承接前关，只讲增量。** 常见措辞是“上一关……”“现在加入……”“使用之前构建的组件……”。确保前置知识确实已可获得。

## 四、叙事与幽默的边界

原作允许导师居高临下、假装关心，以及温和的物种嘲讽。例如 AND Gate 将玩家表现与大象比较，Byte NOT 从脑容量转向手臂数量来解释智力，Conditional Jumps 用海豚不便编程推进故事。

建议新关卡延续这些原则：

- 调侃对象优先是外星人的评价体系、人类作为物种的处境和实验安排。
- 保持平静陈述，笑点后不解释“这是个玩笑”。
- 不要求每关出现笑话；简短技术关允许只有任务。
- 难关可以直说困难并给予鼓励。XOR 明确告诉玩家不必因未立即解出而气馁。
- 保留后期的认可和成长感。Tower of Hanoi 通关文本回望 NAND 起点，并确认人类有资格加入银河联邦。

## 五、不同类型关卡的写法

| 类型 | 语言重心 | 参考关卡 |
| --- | --- | --- |
| 基础逻辑 | 一句构建目标，配真值表或简短条件 | AND、OR、XOR、Always On |
| 位与字节运算 | 按位语义、位宽、进位、有符号解释 | Byte NAND、Adding Bytes、Signed Less |
| 记忆与时序 | 保存条件、覆盖条件、周期关系 | Saving Bytes、Counter |
| 组件与界面教学 | 解释行为，配具体工具操作或图片 | Bit Switch、Instruction Decoder |
| 处理器搭建 | 承接现有机器，说明新增模块及数据流 | Overture、Symphony 系列 |
| 指令实现 | 模式、编码、操作数、写回目标、例外 | Symphony Jumps、RAM、IO Devices |
| 编程与算法 | 输入协议、终止条件、输出协议，必要时给算法 | Conditional Jumps、Capitalize、Tower of Hanoi |
| 竞速小游戏 | 操作方式、目标和明确过关门槛 | Binary Racer |

## 六、术语和排版

- 标题通常是短名词或功能名，如 Equality、Counter、Instruction Decoder。建议采用简短、可检索的中文术语，保留必要英文缩写。
- bit 与 byte 分别使用“位”和“字节”；word 结合实际位宽使用“字”，不能一律译为“字节”。
- signed / unsigned 使用“有符号 / 无符号”；carry 使用“进位”；register 使用“寄存器”。具体名称应以项目术语表为准。
- 保留 [T]、[F]、[Z] 等状态标记，不把高阻态 Z 写成普通数值 0。短路错误 [E] 不应误述为正常第四状态。
- 组件名、引脚名、按钮名需与界面一致。指令助记符、寄存器名、文件名和代码保持原样。
- [tip]、[b]、[color]、[highlight]、[box]、[link]、[image] 等是功能标记；翻译或续写时保持结构合法。
- 位字段颜色承担对应关系，不能随意改色；位序列和代码需要保留换行与对齐。
- 文本中的本地化数字 ID 属于资源标识，不属于玩家可见文案。

## 七、可复用写作模板

### 简单逻辑任务

> 构建一个电路。当【条件】时，输出 [T]；否则输出 [F]。

### 带控制输入的任务

> 当【控制引脚】为 [T] 时，【动作 A】。否则，【动作 B】。

### 新概念教学

> 【概念】用于【当前用途】。

例如：【最小示例】。

本关需要你【具体任务】。

### 架构扩展

> 上一关我们实现了【已有能力】。现在加入【新能力】。

【编码或操作列表】

执行【指令】时，将【来源】写入【目标】。当【例外条件】时，【规定行为】。

### 可选故事开场（原创示例）

> 我们发现，人类在得到第二次机会后表现更好。测试预算暂时允许你验证这一点。

这些模板用于组织信息，不要求每关使用相同开头。建议简单任务控制在一至三句；复杂教学按概念拆段，不设强制总字数。

## 八、审稿检查

- 玩家能否仅根据正文说清输入、输出和成功条件？
- 所有分支、边界、位宽、符号和时序是否明确？
- 新概念是否有足够解释，旧概念是否避免反复讲解？
- 故事、任务、提示、通关反馈是否各自承担明确作用？
- 幽默是否保持导师的平静口吻，且不干扰规则？
- 界面名称、状态标记、代码和资源标签是否完整？
- 是否误把写作建议或未规定的行为当成原作事实？

## 附录：逐关文本与语言组织索引

下列索引用于核查《Turing Complete》原版关卡描述。保留英文文本；个别条目包含解锁提示、通关反馈等辅助文案，这些文本不作为关卡描述的主要依据。未提供描述的条目明确标注，不补写说明。

### Always On

```text
Create a circuit that is always [T].
```

```text
This level unlocks your first manual page. This one is important, make sure you pay attention to it.
```

### AND Gate

```text
Right now the elephant is dominating all the other test subjects from Earth. Adjusting for brain size, you are also doing well.
```

```text
Create the circuit that matches the table.
[tip]Most levels have a hint button (the lightbulb) you can click if you are stuck.[/tip]
```

### Bigger AND Gate

```text
Create an AND gate with 3 inputs.
```

### Double Detection

```text
This level has 4 inputs.
Output [T] when 2 or more of them are [T].

[tip]Don't overthink this level.[/tip]
```

### Assembly Programming

```text
Punchcard programming is tedious. Lets try adding 5 to the input again like in the previous level, but this time using assembly programming instead.
```

```text
The point of an assembly language is to give human readable nicknames to the instructions that our machines execute.

Conventionally, an instruction syntax will begin with an abbreviated word (called a "[color=#9966ff]mnemonic[/color]") that describes the instruction, followed by a list of [color=#66ccff]operands[/color] like so:
[highlight][color=#9966ff]mov[/color] [color=#66ccff]r1[/color], [color=#66ccff]r2[/color][/highlight]

Also by convention, the destination is on the left. So in the above example we move a value to r1 from r2. If you learn these conventions you will be able to read real life assembly languages.

Click "Edit" on the RAM component to open the assembly editor. From there, click the "spec.isa" tab in the upper left corner to get the full list of Overture instructions.
```

### Punchcard Programming

```text
Since both you and the dolphin built a computer, the competition for who gets to represent Earth at the Galactic Federation will go on until one of you drops out.

The next milestone is writing a program to solve the maze.
```

```text
Now that you have built your first computer it is time to learn how to program it.
```

```text
Click on the "Edit" button on the "Program" component to start punchcard programming your computer. Your computer should read an input, add 5 to it and output it again.
```

### Binary Racer

```text
This level is my favorite pastime. Converting decimal into binary under time pressure.
```

```text
Toggle the bits in the level panel so they add up to the decimal number in the question.

You must beat level 3 to pass.
```

### Code Breaker

```text
In this level you win when you output the secret code I am thinking of. No output will fail the level, so you could just try all numbers.

Also, I will set the input to 1 after a guess that was too high and 0 otherwise. You might be able to use this to find my code faster.
```

### Half Adder

```text
We measure the universe with numbers. So our machine must know how to count.
```

```text
Treat the inputs as [b]0[/b] or [b]1[/b] depending if they are [F] or [T]. Add them together in binary, such that the result is either [b]0[/b] or [b]1[/b].

As with normal addition, if the result cannot be described with one digit, set the carry to [b]1[/b].
```

### Bit Inverter

```text
We are experimenting with how backgrounds influence cognitive function in Earthlings.
```

```text
Create a conditional value inverter. When [color=#e49f44][b]Invert[/b][/color] is [T], output the opposite of [color=#e49f44][b]Value[/b][/color]. Otherwise just output [color=#e49f44][b]Value[/b][/color] as is.
```

```text
This level is exactly the same as the XOR gate level you solved before. Remember no matter what I tell you the goal of the level is, the real truth is in the truth table.
```

### Bit Switch

```text
If components output different values on the same wire, you get a short circuit error.

[center][image="short_circuit.png"][/center]

However, some components have grey output pins, these are not outputting at all when the component is not enabled. This is the case for the Bit Switch component.

[center][image="switch.png"][/center]

When the bottom pin is [F], the output is disabled. In this case, more than 1 output can connect to the same wire without causing error.

[center][image="bus_wire.png"][/center]

Practically speaking, it only makes sense to connect multiple outputs to the same wire if they are switches. Components need to "take turns" to output for it to be useful.
```

```text
Alternatively to thinking of the output as being disconnected, it can be thought of as outputting neither [F] or [T], but a third state we call "Z state" [Z].

Connecting two outputs to a wire will produce the following states:

Output 0 [F][F][F][T][T][T][Z][Z][Z]
Output 1 [F][T][Z][T][F][Z][F][T][Z]
   State [F][E][F][T][E][T][F][T][Z]

Note that short circuit [E] can not meaningfully be thought of as a fourth output state, as it breaks the circuit. It is included in the table for completeness anyway.
```

```text
Using 2 NOT gates and 2 Switches, build an XOR gate.
```

### Adding Bytes

```text
Add the two input bytes. Each output bit in the output should be a result of the addition of the corresponding bits from the inputs, and potentially a carry.
```

```text
If the result does not fit in 8 bits, turn the output carry [T] (you can think of it as the 9th bit)

Finally, there is an input carry as well. This is useful for chaining together byte adders to add larger numbers. You can think of this carry as adding either 0 [F] or 1 [T].
```

```text
Also, this level has a lot of tests. Right click on the run button to increase the testing speed.
```

### ASR

```text
The shift you built in the last level is known as [b]logical[/b] shift, logically shifting right by 1 is the same as dividing by 2 for unsigned numbers. For signed numbers, if the top bit of the value is [T], we fill the empty bits with [T] instead of [F]. This is called [b]arithmetic shift right[/b], often written as [b]asr[/b] in assembly.

[color=#e49f44]-106 asr 3[/color]
  [T] [F] [F] [T] [F] [T] [T] [F]
Results in:
 |[T] [T] [T]|[T] [F] [F] [T] [F]
^
And here we insert [T] instead of [F]
```

### Byte Constant

```text
Create a circuit that always outputs the number 164.
```

### Divide

```text
Divide the two inputs and output the result. Note, this level is optional, because it is very hard. If the second input (the divisor) is 0, the result should be 0.
```

### Equality

```text
Output [T] when both inputs are the same.
```

### Signed Less

```text
Create a circuit that outputs [T] when the first input is less than the second. Interpret the bytes as being [color=#dd6576]signed[/color].
```

### Unsigned Less

```text
Create a circuit that outputs [T] when the first input is less than the second. Interpret the bytes as being [color=#dd6576]unsigned[/color].
```

### LSR

```text
To shift a value 1 right just means moving all the bits in a byte to the right by 1 position.

This operation is known as [b]logical shift right[/b] and in assembly it is often abbreviated as [b]lsr[/b].

For example [b]180 lsr 1[/b]
[left]
           [T][F][T][T][F][T][F][F] Shifts this by 1:
           [F][T][F][T][T][F][T][F]
[/left]
And [b]180 lsr 3[/b]
[left]
           [T][F][T][T][F][T][F][F] Shifts this by 3:
           [F][F][F][T][F][T][T][F]
[/left]
```

```text
The task in this level is to shift the first input to the right by the value given in the second input.
```

```text
In addition to giving you a shift right component, you also get a shift left component.
```

### Modulo

```text
Output the modulo of the two inputs. If the second input (the divisor) is 0, the result should be the same as the first input (the dividend).
```

### Multiplexer

```text
Even though he did not pass our tests we decided to keep the dog. Unlike most Earthlings he is fluffy and can follow simple instructions.

We might want to team you two up, since you complement each other's shortcomings well.
```

```text
When the bit selector input is [F], output Byte A, otherwise output Byte B.
```

### Byte NAND

```text
Don't worry, if you can't build a computer you will either be released into the wild or be put in a zoo with lots of space.
```

```text
NAND each bit of the input bytes and output the result.

For example:
Input 0 [T][T][F][F][F][T][F][F]
Input 1 [F][F][F][F][T][T][F][F]
Output  [T][T][T][T][T][F][T][T]
```

### Byte NOT

```text
Our previous model of brain size being the main predictor of intelligence was naive.
```

```text
Tool making and usage is key in the early evolutionary stages of developing intelligence.
```

```text
Therefore the number of arms on a creature is clearly the main predictor of intelligence. Did you know there are sea creatures from your planet with four times as many arms as you and they are doing better in tests?
```

```text
"NOT" each bit of the byte input.
```

### Byte XOR

```text
Create a circuit that XORs two bytes bitwise.
```

### Capitalize

```text
The inputs in this level are sentences encoded as ASCII ([link="Computer concepts/ASCII encoding"]see manual[/link]). Each word is separated by a space which has the numeric value 32. Replace the first letter in each word with its uppercase counterpart.

The possible input characters are a to z lowercase and space.
```

### Circumference

```text
In this level you are given a radius and you calculate the circumference.

This is the equation: 2×π×r

r is the input.
And you can approximate π as 3.

Send the answer to output when it has been calculated.
```

### Conditional Jumps

```text
Good news for you, the dolphin already dropped out.

He had a hard time programming with his flippers.

We will keep the tests going though, to test how far humans can go.
```

```text
The goal of this level is to teach you about jumps and labels.

A jump is an instruction that overwrites the counter.

Overwriting the counter means we "jump" to an instruction somewhere else in our program, instead of simply proceeding to the next.
```

```text
If you use a fixed number as your jump destination, you will have to update it every time you add or remove code.

Instead you can use a feature called a [color=#66ccff]label[/color] to mark a location in the program that automatically updates when you change anything, like so:

[box][left]
[color=#66ccff]my_label[/color]:     [color=#aaaaaa]; This is a label[/color]
[color=#9966ff]imm[/color] [color=#66ccff]my_label[/color]  [color=#aaaaaa]; Load the offset of the label[/color]
[/left][/box]
```

```text
To finish this level, read the input until you see the number 37.

Then output a count of how many times you read the input.
```

### Count leading zeroes

```text
Create a circuit that starts from the most significant bit and counts how many in a row are [F].

The answer for this, for example, is 3, since it has 3 leading [F] bits:
[F][F][F] [T][F][T][F][T]
```

### Counter

```text
Counting is so fundamental even insects do it. From counting, species can evolve to compare and do arithmetic. And before you know it, you have apes building computers.
```

```text
Build a counter that will increase by 1 each cycle.

Additionally, there should be an option for overwriting the counter with a given value.

You are given two inputs, a bit and a byte.

The bit input should toggle between counting and overwriting with the value of the byte.
```

### Counting Signals

```text
Pain is temporary, glory is forever
```

```text
The output component of this level is a binary counter where the three pins correspond to 1, 2 and 4.
Use the binary counter to display the number of signals.

[tip]The solution for this level is not very neat and requires more components.[/tip]
```

### 1 Bit Decoder

```text
Create a component that can switch a signal between 2 pins.
```

### 2 Bit Decoder

```text
With 2 bits of input there are 4 combinations. Make a circuit that selects 1 output for each of the 4 combinations. No more or less than 1 bit should be [T] at a time.
```

### 3 Bit Decoder

```text
With 3 bits of input there are 8 combinations. Make a circuit that selects 1 output for each of the 8 combinations. Additionally, there is a disable input bit. When it is [T] all the outputs should be [F].
```

### Circular Dependency

```text
In our education system we traditionally teach by tricking students into doing the wrong thing and then teasing them.
```

```text
I am not sure it benefits the students, but teachers love it.
```

```text
Create a "circular dependency". This is a circuit where the input of a component relies on its own output. In a circular dependency situation, it is not possible to determine the output of a component, because you would first need to determine the input, which relies on the output and so on (hence the "circular").

This state is normally not allowed in other levels, but in this level the goal is to create it, so you understand what it is before going forward.
```

### Delayed Lines

```text
This level introduces the Delay Line component. It takes its input and outputs it 1 cycle later.
```

```text
Construct a circuit that outputs the same as the input, just delayed by 2 cycles.
```

### Double the Number

```text
This level introduces byte values, which are shown as numbers between 0 and 255. Bytes are made of 8 bits, each bit is being a binary digit of the number. You are given two new components, one of which takes a byte and splits it into bits, while the other does the reverse.

Using those components, figure out how to double the input number (up to 127).
```

### The Foundry

```text
Welcome to the foundry, the circuits you create in here will be usable as components in other levels.
The circuitry you create defines the behavior of the component and the layout defines its shape.
```

```text
Components can be added, renamed or deleted using the schematics menu.
```

```text
Since this level is a tool and not a challenge, you can go to the level map and continue on when you want.
```

### Full Adder

```text
In the previous level we added together 2 inputs, this time we add 3. Once again, add the inputs together in binary, such that the result is either [b]0[/b] or [b]1[/b].

As with normal addition, if the result cannot be described with one digit, set the carry to [b]1[/b].
```

### Hex Racer

```text
This level is my second favorite pastime. Converting hexadecimal to binary under time pressure.
```

```text
When dealing with many bits, binary can become hard to read. Hexadecimal (base 16) represents each combination of 4 bits as its own character:

0 [F][F][F][F]
1 [F][F][F][T]
2 [F][F][T][F]
3 [F][F][T][T]
4 [F][T][F][F]
5 [F][T][F][T]
6 [F][T][T][F]
7 [F][T][T][T]
8 [T][F][F][F]
9 [T][F][F][T]
A [T][F][T][F]
B [T][F][T][T]
C [T][T][F][F]
D [T][T][F][T]
E [T][T][T][F]
F [T][T][T][T]

As an example where hexadecimal is clearer, lets compare the representation of the largest 16 bit number:
[left]
              Binary:      1111111111111111
              Decimal:     65535
              Hexadecimal: FFFF
[/left]
It is not easy to tell if the binary is missing a digit or if the decimal number is correct, the hexadecimal on the other hand is immediately clear (once you get used to the representation at least).
```

```text
Toggle the bits in the level panel so they add up to the hexadecimal number in the question.

You must beat level 3 to pass.
```

### Humble Beginnings

```text
Congratulations, you have been abducted.

We are testing you on our spaceship.

Build a functional computer and your species may join the Galactic Federation.
```

```text
This first test checks your understanding of circuits. Here you have an input and an output with a wire in between. Turn on the input in the left hand menu to complete it.
```

### Logic Exam

```text
This level is here to make sure you master Boolean logic before you proceed. Being good at this will help you a lot in the future.
```

```text
You will be tested on a few different circuits here. When you hit the "Run" button, your circuit will be tested and you will get a green mark if it was correct or a red mark otherwise. The level is complete when 4 of the 5 marks in the history box below are green.
```

```text
Great job. Now that you have a firm grasp of Boolean logic, it is time to work towards building your first computer. But first you must learn about memory and binary arithmetic. The path to mastery is as long as it is satisfying.
```

### 无标题

此条目没有关卡说明文本。

### The Maze

```text
In this level you control a robot. Write an algorithm that guides him from the start to the door at the end of the maze.

These are the three commands you can output:
0 Turn left
1 Move forward
2 Turn right

You can also use the arrow keys to play around with the robot manually.
```

```text
Read the input to figure out what the robot sees in front of him. It is always one of these:
0 Nothing
1 Wall
```

```text
Great job. You finished the maze and are clearly a master at Overture assembly now. But Overture is too basic, it is good for learning, but is missing many features. Your next project will be to build towards Symphony, a more powerful computer which has RAM, a screen, a keyboard and lets you reuse code through functions. On the way there you will have to build some new components, learn about the stack and other things.
```

### Mod 4

```text
The length of our week is 4 days. Our weekdays are named Zero's day, One's day, Two's day and Three's day. I hear some Earthlings number their Two's day incorrectly?

It doesn't really surprise me.
```

```text
Anyway new year is coming up and we want you to calculate the weekday of everyone's birthday. We will give you the date and all you have to do is return the weekday within 8 cycles.
```

```text
In this level you are given a number as input and you should return that number modulo 4 as a result.

The challenge is that you only have 8 cycles to output the desired result.

Note: You don't need to change your hardware, you just have to figure out the trick to do modulo 4 quickly.
```

### Multiply

```text
Multiply two numbers. If the result doesn't fit in 8 bits, discard the bits that don't fit.
```

### NAND Gate

```text
All the animal species of Earth are going through the same test, to see if you have the intelligence to learn to build a computer.
```

```text
The first section is about "Boolean logic". Boolean logic describes the simplest forms of calculation computers can do. In fact these calculations are what every other calculation in the computer is made out of.
```

```text
This circuit contains a "NAND gate" that I want you to examine. I want you to document its behavior by filling in the "truth table" in the panel you see below. Only the two input rows of the truth table are filled out. Your task is to experiment with the inputs to the circuit, observe what the NAND gate does and fill the last row of the truth table.
```

```text
Correct! You have unlocked the NAND gate. Out of this fundamental building block, you will construct everything else in your computer.
```

### Negative Numbers

```text
To know the difference between things, you need subtraction. To get subtraction, first you need negative numbers.
```

```text
This level introduces "two's complement", the most common representation for negative numbers.

Here the highest digit is negated. For bytes this means the 8th digit changes its value from 128 to -128.

You must beat level 2 to pass.
```

### Nim

```text
In this level you need to beat the AI in a simple card game. These are the rules:

There are 12 cards on the table and each player takes turn and must pick up between 1 and 3 cards. You start and the player who picks up the last card (the joker) loses.

Reading input will give you the current number of cards. Sending 1, 2 or 3 to output will pick up that number of cards. The opponent reacts immediately, so you may read the input immediately after your output to get the result of his action.
```

```text
Also, schematics are shared across architecture levels, so you can solve this with either Overture or Symphony. Click the file icon in the upper left corner to change shematics.
```

### NOR Gate

```text
Most creatures on Earth are aggressive and cannot focus.

To succeed you must learn to think logically, while not destroying things.
```

```text
Create the circuit that matches the table.
```

### NOT Gate

```text
Now that you unlocked the NAND gate, it is time for you to build a circuit yourself.
```

```text
In the last level you made the truth table that described the circuit. In this level you build the circuit that matches truth table.
```

```text
Using NAND gates, build the NOT gate

Using: [b]NAND gate[/b]
Input 0: [F][T][F][T]
Input 1: [F][F][T][T]
Output:  [T][T][T][F]

Build: [b]NOT gate[/b]
Input:   [T][F]
Output:  [F][T]
```

### Odd Number of Signals

```text
Using a maximum of 3 components, output [T] only when an odd number of inputs are [T]
```

### Odd cycles

```text
Double click a component to select it and all attached wires.
```

```text
In a previous level you learned how we don't allow circular dependencies. Now you must learn the one exception.
```

```text
The delay line is allowed to depend on its own output. This is because its input does not influence the rest of the circuit until next cycle.

[image="delay_line.png"]
```

```text
Orange pins never affect the output in the same cycle. They therefore never cause circular dependencies.

  [image="delay_line.png"]
```

```text
Output [F] on the even cycles and [T] on the odd cycles.
```

### One-hot Encoding

```text
The input decides which output should be turned [T]. So if the input is 7, send [T] to the 7th output and [F] to all the others.

Do not use more than 3 components. Also, you may not use any of the decoders you built before.
```

### OR Gate

```text
Create the circuit that matches the table.
[tip]Double click on a component to move it together with connected wires.[/tip]
```

### Bigger OR Gate

```text
Create an OR gate with 3 inputs.
```

### Registers

```text
It is finally time to build your first computer. I will teach you how to build the [color=#e49f44]Overture[/color] architecture, an actual Turing complete machine, a true computer in every way.
```

```text
In this level you need to create a circuit which can move a value to a [color=#e49f44]destination[/color] from a [color=#ff6680]source[/color].

The instruction byte in this level determines the [color=#e49f44]destination[/color] and the [color=#ff6680]source[/color].

Bits 0, 1 and 2 give the [color=#ff6680]source[/color]. Bits 3, 4 and 5 give the [color=#e49f44]destination[/color].
```

```text
[color=#ff6680]Source[/color] and [color=#e49f44]destination[/color] can be 1 of 6 registers, we will name them REG 0 to REG 5 respectively.

Additionally, this level has a dedicated input component, which can be the [color=#ff6680]source[/color] and an output component, which can be the [color=#e49f44]destination[/color].
```

```text
These are the bit patterns for each [color=#ff6680]source[/color] and [color=#e49f44]destination[/color].

[color=#ff6680]Source[/color]:
[left]
                      [F] [F] [F]  REG 0
                      [F] [F] [T]  REG 1
                      [F] [T] [F]  REG 2
                      [F] [T] [T]  REG 3
                      [T] [F] [F]  REG 4
                      [T] [F] [T]  REG 5
                      [T] [T] [F]  INPUT
                      [T] [T] [T]  UNUSED
[/left]

[color=#e49f44]Destination[/color]:
[left]
                      [F] [F] [F]  REG 0
                      [F] [F] [T]  REG 1
                      [F] [T] [F]  REG 2
                      [F] [T] [T]  REG 3
                      [T] [F] [F]  REG 4
                      [T] [F] [T]  REG 5
                      [T] [T] [F]  OUTPUT
                      [T] [T] [T]  UNUSED
[/left]
```

### ALU

```text
Time to merge in the "Arithmetic Logic Unit" circuit you made previously with the register circuit. The ALU circuit was saved in the foundry and can now be added as a component. If you forgot which pin does what, check the circuit in the foundry.

Use the decoder you built to figure out if we should move between registers or do ALU calculations. Here are the 4 different modes again:
[left]
                      [F][F] Immediate
                      [F][T] ALU
                      [T][F] Move
                      [T][T] Condition
[/left]

For this level you only have to worry about the move and ALU mode. When in ALU mode, take [color=#e49f44]REG 1[/color] and [color=#e49f44]REG 2[/color] as inputs, and store the result in [color=#e49f44]REG 3[/color].

[tip]Don't forget the 3 bit decoder component has a disable bit.[/tip]
```

### Immediate Values

```text
We need a way to directly move numbers from our program into registers. For this we use the immediate mode (when the two highest bits are [F][F]).

When in immediate mode the whole byte is interpreted as a number that we store into REG 0. This means we can
store any value between 0 and 63.
```

### Program

```text
The instruction input component has been removed. It has been replaced by a RAM component containing the program. The RAM has a "load port" attached to it. When the enable pin on the load port is enabled, it will output a value from the RAM, based on the given address. Connect the counter so it loads a next instruction each cycle.
```

### Turing Complete

```text
Until this point all possible programs have been confined to running in order byte by byte.
```

```text
Before, only code could influence memory, now memory must influence code. With the addition of conditional logic, our computer can run any algorithm, and calculate anything calculable. Like all non-theoretical computers, this machine will only be limited by having finite memory.
```

```text
The final thing we need to add is a mechanism for changing the program counter through instructions when certain conditions are met.

When the two largest bits are [T][T], we are in condition mode. In condition mode, the value in REG 3 is compared against the conditions defined by the lowest 3 bits in the instruction. If the condition is true, we overwrite the counter to the value in REG 0.
Conditionally changing the counter means we can skip instructions based on conditions or have instructions run in a loop.

Conditions:[left]
                    [F][F][F] Never
                    [F][F][T] Always
                    [F][T][F] If [color=#e49f44]REG 3[/color] = 0
                    [F][T][T] If [color=#e49f44]REG 3[/color] ≠ 0
                    [T][F][F] If [color=#e49f44]REG 3[/color] < 0
                    [T][F][T] If [color=#e49f44]REG 3[/color] ≥ 0
                    [T][T][F] If [color=#e49f44]REG 3[/color] ≤ 0
                    [T][T][T] If [color=#e49f44]REG 3[/color] > 0
[/left]
These conditions correspond to the condition component that was saved in the foundry.
```

```text
You did it! I thought you were just this odd looking hairless ape creature, but you actually built a real computer! Amazing.
```

### Overture

```text
The only purpose of this optional level is to let you experiment with how much you can optimize Overture. Unlike previous levels, this level does not require you to complete exactly one instruction per cycle.
```

```text
By default, this level only checks that you write the correct values to output. You may opt in to having your register state checked as well, which is very useful for debugging, but this is optional. To have your register state be checked, send the number of instructions you finished each cycle to the component labeled "DoneCount".
```

```text
Note that if you change the RAM type to DRAM, you can set the pipeline depth of the RAM. If you for example set the depth to 2, loads will take two cycles, but the delay of the RAM will be divided by 3.
```

### Arithmetic Logic Unit (ALU) 1

```text
Create a device that can OR, NAND, NOR or AND two byte inputs. The third input will be the instruction. An instruction is just what we call the number that determines what to compute.

Instructions:

[color=#87a8c8]0 NAND
1 OR
2 AND
3 NOR[/color]
```

### Arithmetic Logic Unit (ALU) 2

```text
Add addition and subtraction to your circuit.

Instructions:

[color=#87a8c8]0 NAND
1 OR
2 AND
3 NOR
4 ADD
5 SUB[/color]
```

### Conditions

```text
This level has a value input and a 3-bit condition input.

The 3 bits select the condition as shown below. Check the value against the selected condition and output [T] if it is satisfied and [F] otherwise.

[left]
             Bit code:     Turn [T] when:
              [F][F][F]       Never
              [F][F][T]       Always
              [F][T][F]       If value = 0
              [F][T][T]       If value ≠ 0
              [T][F][F]       If value < 0
              [T][F][T]       If value ≥ 0
              [T][T][F]       If value ≤ 0
              [T][T][T]       If value > 0
[/left]
```

### Instruction Decoder

```text
The circuit you built in the "Registers" level can copy values between registers, while the "Arithmetic Logic Unit" can do different operations on 2 inputs. But you need to be able to do both in the same circuit. To do this, build a "decoder", which will decide which mode our computer is in, based on the 2 bits we haven't used yet.
```

```text
To distinguish between the instructions of 4 different kinds, the two highest bits will be used to determine which MODE we are in as follows:

[left]
                [F][F][Z][Z][Z][Z][Z][Z] Immediate
                [F][T][Z][Z][Z][Z][Z][Z] ALU
                [T][F][Z][Z][Z][Z][Z][Z] Move
                [T][T][Z][Z][Z][Z][Z][Z] Condition
[/left]
Here [Z] means any value.

Determine the MODE we are in from the input, then send [T] to the correct output.
```

### Hub Preview

此条目没有关卡说明文本。

### Hub Preview

此条目没有关卡说明文本。

### Little Box

```text
Can you fit 4 bytes of memory in this limited space?
```

```text
In this level you have to build a circuit that
can store or load from 4 different bytes of memory.

You are given 1 bit that determines if you are to load. Another bit determines if you are to store and it comes with a corresponding value.
Additionally, you have 2 address bits.

With 2 bits, there are 4 combinations, one for each byte of memory in this level.

[tip]Spacebar rotates components.[/tip]
```

### Random Number Generator

```text
Do you know how we deterministic robots can be creative and produce original thoughts? The answer is random number generators.
```

```text
In this level you only get one input, we call this the initial seed. The seed is put through the following steps to produce a pseudorandom number.

[b]32 bit xorshift RNG:[/b]
[box][left]
temp1  = seed  xor (seed  lsr 13)
temp2  = temp1 xor (temp1 lsl 17)
result = temp2 xor (temp2 lsr 5)
[/left][/box]

Finally output "result" and then use it as the seed of the next iteration.
```

### The Sandbox

此条目没有关卡说明文本。

### Saving Bytes

```text
Create a circuit that can store a byte.

When the "SAVE" input is [T], store the input byte.

At all times output the stored value to the output.
```

### Saving Gracefully

```text
The Delay Line only allows us to store a value for 1 cycle. But sometimes a component that can store a value for longer periods of time is useful.

We want you to build such a component.
```

```text
In this level you have 2 inputs.
When the first input is [T], update the stored value.
The second input is the new value to update the stored value with.
Always output what is currently stored.
```

### Objective Beauty

```text
Useful energy, known as exergy, is finite and if we consume it all, we will be at the heat death of the universe. This is why efficiency is important.

In our society, we find inefficiency ugly and efficiency beautiful.

In fact, we measure beauty in units of energy.
```

```text
This level introduces efficiency scoring, which is based on time and energy.
```

```text
These equations are equal, yet not the same:

((A + B) + C) + D
(A + B) + (C + D)

They produce the same result, but parallelism allows one to be faster than the other.
```

```text
Each of the basic gates (NAND, NOR, AND, OR) have a delay of one, meaning they take a unit of time to complete.

However, different parts of a circuit can happen simultaneously, so we only have to wait for the slowest path, called "the critical path".

Click the hourglass in the upper right to see the critical path.
```

```text
Each of the four basic gates consume one unit of energy in each unit of time.

So the total energy cost of your solution is the delay of the critical path multiplied with the number of fundamental gates used.
```

```text
The cost of the components you have unlocked are based on your score in the levels where you unlocked them.

If you want to improve the cost of a component, you can go back and optimize your solution in the level that unlocks it.

In fact, to beat this level, your XOR gate should cost 3 gates and have a delay of 2.
```

```text
To solve this level, add 1 to the input number and output the result.

If the result doesn't fit in 8 bits, discard the bit that doesn't fit.

Do this with no more than 40 gates and 10 delay.
```

### Second Cycle

```text
There is no shame in using pen and paper
```

```text
Output [T] on the second cycle only.
```

### Signed Negator

```text
Taking the input as signed (where the 8th bit is -128), make a component that takes a number and negates it.
For example 4 negated would become -4. -9 negated would become 9.
```

### Delicious order

```text
We are updating the galactic encyclopedia of food, inserting human foods. As we don't have an alphabet, entries in the encyclopedia are ordered by deliciousness.
```

```text
In this level your job is to sort 16 numbers. Read the input 16 times one by one, sort them and output them smallest to largest.
```

### Stack

```text
The challenge of this level is to implement a common programming structure called a "stack". You are supposed to do this in assembly. You can do two things with a stack:
[left]
  [color=#66ccff]PUSH[/color] - Adds a number to the stack
  [color=#66ccff]POP[/color]  - Removes the last added number from the stack
[/left]
You can think of the stack data structure like a physical stack of plates in a busy restaurant. The dishwasher will [color=#66ccff]push[/color] plates on top of the stack and the chef will [color=#66ccff]pop[/color] them off.

Read the input in this level to get the next operation.
A non-0 input is a push, in this case store the value so it can be popped later.
A 0 value is pop, remove and output the last number you stored.
```

```text
Since the stack is so useful for programming, a "push" and "pop" instruction will be added to the next Symphony level. Both of these are synonyms for two other instructions and they both use the "sp" register (short for stack pointer) to keep track of the stack.

"push" is a synonym for:
[box][left]
sub sp, sp, 4
store_32 [sp], %value
[/left][/box]
"pop" is a synonym for:
[box][left]
load_32 %destination, [sp]
add sp, sp, 4
[/left][/box]
You may notice that "push" subtracts from "sp" so that the stack grows downwards from the high addresses. This leaves the low addresses for you to use as you see fit.
```

### Functions

```text
Sometimes it is useful to reuse a section of code. We call these sections "functions". To reuse code, jump to the start of the function and jump back again at the end. We call jumping to the function "calling" the function, and jumping back from the function "returning".

But for this section of code to actually be reusable, the return jump will have to be able to return to different locations, depending on where we called the function from.

We could store the counter value to a register before jumping to the function and use this value when returning. However, if done this way, a function would not be able to call another function, since this would overwrite the original return address.

Say function A calls function B, who calls function C. When we want to return from C, we only need the return address for B and in B we only need the address for A. In general, regardless of which function we look at and how they call each other, the last return address we added is always the first one we will need again. This is exactly the behavior of the stack.
```

```text
In this level a "call" and "ret" instructions have been added to your list of instructions. Both of these are complex synonyms.

"call" is used to enter a function. It pushes the address of the next instruction on the stack (by reading the counter and adding an offset). It then jumps to the start of the function.

"ret" is used to return from a function. It pops the return address off the stack and jumps to it.
```

```text
As functions likely change the value of registers, we may want to push values on the stack to save them and pop them off again afterwards to restore them. It isn't important whether this is done inside or outside the called function, however it is good practice to establish a convention for this (called a "calling convention"), such that all functions are compatible with each other.
```

```text
This level is only here for you to observe how functions work, there is no test. Be sure you read the assembly and step through to the end so you understand how everything works. It will be necessary for future levels.
```

### Fast Symphony

```text
The objective of this level is simple, reduce the delay of Symphony to 175 or below. Go back through previous levels and improve scores if you have to.
```

### Wire Spaghetti

```text
Finally it is time for you to start building the [color=#e49f44]Symphony[/color] architecture!
```

```text
Most instructions take 2 arguments and produce 1 result (like ADD, OR etc).

[color=#e49f44]Symphony[/color] uses 4 bytes per instruction and can encode all three registers in each instruction, which are stored in the RAM component named "Main Memory". This means we can for example ADD any two registers and store the result in any register.
```

```text
These are the names of the 16 registers of [color=#e49f44]Symphony[/color]:

[center]
0  zr
1  r1
2  r2
3  r3
4  r4
5  r5
6  r6
7  r7
8  r8
9  r9
10 r10
11 r11
12 r12
13 r13
14 sp
15 flags
[center]

"zr" will always produce 0 when read, so any write to "zr" will be discarded, we can use this to simplify our hardware. "sp" and "flags" are normal registers that just happen to have different names. They will be reserved for special purposes later.

Instead of using individual registers, use the provided RAM component that is named "Register File".
```

```text
With all of that explained, implement the NAND instruction.

This is how the instructions are encoded:

[color=#bbbbbb]0[/color][color=#ff9800]MM[/color][color=#bbbbbb]0[/color][color=#ff6680]OOOO[/color] [color=#ffe666]DDDD[/color][color=#66ff99]AAAA[/color] [color=#bbbbbb]0000[/color][color=#66ccff]BBBB[/color] [color=#bbbbbb]00000000[/color]

[center]
[color=#ff9800]M[/color] = Mode             [color=#ffe666]D[/color] = Destination register
[color=#ff6680]O[/color] = Opcode           [color=#66ff99]A[/color] = Operand A
[color=#bbbbbb]0[/color] = Unused / zero    [color=#66ccff]B[/color] = Operand B
[/center]

Use the decoder to separate the destination and argument registers. Be careful: load and store ports accept the address in bytes, but each of the registers takes up 4 bytes. This means you need to provide the address of the register, not its index.
Also, in this level the [color=#ff9800]Mode[/color] is always 1 and the [color=#ff6680]Opcode[/color] is always 0. This means we NAND the first argument with the second and store the result to the result register. For now you can just use a 32 bit NAND gate in place of the ALU.

[center][image="symphony_diagram_1.png"][/center]
```

### IO

```text
In this level we add a way to input and output values to [color=#e49f44]Symphony[/color].

Again, this is how instructions are encoded:

[color=#bbbbbb]0[/color][color=#ff9800]MM[/color][color=#bbbbbb]0[/color][color=#ff6680]OOOO[/color] [color=#ffe666]DDDD[/color][color=#66ff99]AAAA[/color] [color=#bbbbbb]0000[/color][color=#66ccff]BBBB[/color] [color=#bbbbbb]00000000[/color]

[center]
[color=#ff9800]M[/color] = Mode             [color=#ffe666]D[/color] = Destination register
[color=#ff6680]O[/color] = Opcode           [color=#66ff99]A[/color] = Operand A
[color=#bbbbbb]0[/color] = Unused / zero    [color=#66ccff]B[/color] = Operand B
[/center]


Just like with [color=#e49f44]Overture[/color] there are 4 modes:

0 IO
1 ALU
2 JUMP
3 RAM

However, in this level you only have to worry about IO and ALU mode. When in ALU mode, execute the NAND instruction as in the previous level. When in IO mode, execute one of the following instructions, based on the opcode:

0 nop (Do nothing)
1 in  (Send input to the [color=#ffe666]destination register[/color])
2 out (Send [color=#66ccff]argument B[/color] to output)
```

### Integrating ALU

```text
Add the ALU you built in the previous level to the circuit. The operation of each instruction is encoded in the [color=#ff6680]Opcode[/color].

[color=#bbbbbb]0[/color][color=#ff9800]MM[/color][color=#bbbbbb]0[/color][color=#ff6680]OOOO[/color] [color=#ffe666]DDDD[/color][color=#66ff99]AAAA[/color] [color=#bbbbbb]0000[/color][color=#66ccff]BBBB[/color] [color=#bbbbbb]00000000[/color]

[center]
[color=#ff9800]M[/color] = Mode             [color=#ffe666]D[/color] = Destination register
[color=#ff6680]O[/color] = Opcode           [color=#66ff99]A[/color] = Operand A
[color=#bbbbbb]0[/color] = Unused / zero    [color=#66ccff]B[/color] = Operand B
[/center]
```

### Immediates

```text
Sometimes it is useful to load a value directly from the program instead of from registers. A value directly encoded in an instruction is called an immediate value. With [color=#e49f44]Symphony[/color], we can replace operand B with an immediate in any instruction. This is done by setting the fourth bit to 1 and encoding the value in the last two bytes. Here is an overview of our both kinds of instructions:

Imm bit [color=#9966ff]0[/color]: [color=#bbbbbb]0[/color][color=#ff9800]MM[/color][color=#9966ff]0[/color][color=#ff6680]OOOO[/color] [color=#ffe666]DDDD[/color][color=#66ff99]AAAA[/color] [color=#bbbbbb]0000[/color][color=#66ccff]BBBB[/color] [color=#bbbbbb]00000000[/color]
Imm bit [color=#9966ff]1[/color]: [color=#bbbbbb]0[/color][color=#ff9800]MM[/color][color=#9966ff]1[/color][color=#ff6680]OOOO[/color] [color=#ffe666]DDDD[/color][color=#66ff99]AAAA[/color] [color=#66ccff]IIIIIIII IIIIIIII[/color]

[center]
[color=#ff9800]M[/color] = Mode                [color=#ffe666]D[/color] = Destination register
[color=#ff6680]O[/color] = Opcode              [color=#66ff99]A[/color] = Register A
[color=#9966ff]0[/color]/[color=#9966ff]1[/color] = Immediate mode    [color=#66ccff]B[/color] = Register B
[color=#bbbbbb]0[/color] = Unused / zero       [color=#66ccff]I[/color] = Immediate value
[/center]
```

```text
Update the architecture to look like this diagram:

[image="symphony_diagram_2.png"]
```

### Jumps

```text
These are the [color=#ff9800]Modes[/color] of [color=#e49f44]Symphony[/color]:

[center]
[color=#ff9800]0[/color] IO
[color=#ff9800]1[/color] ALU
[color=#ff9800]2[/color] JUMP
[color=#ff9800]3[/color] RAM
[/center]

In this level we implement the JUMP mode. As always the instruction encoding is as follows:

Imm bit [color=#9966ff]0[/color]: [color=#bbbbbb]0[/color][color=#ff9800]MM[/color][color=#9966ff]0[/color][color=#ff6680]OOOO[/color] [color=#ffe666]DDDD[/color][color=#66ff99]AAAA[/color] [color=#bbbbbb]0000[/color][color=#66ccff]BBBB[/color] [color=#bbbbbb]00000000[/color]
Imm bit [color=#9966ff]1[/color]: [color=#bbbbbb]0[/color][color=#ff9800]MM[/color][color=#9966ff]1[/color][color=#ff6680]OOOO[/color] [color=#ffe666]DDDD[/color][color=#66ff99]AAAA[/color] [color=#66ccff]IIIIIIII IIIIIIII[/color]

[center]
[color=#ff9800]M[/color] = Mode                [color=#ffe666]D[/color] = Destination register
[color=#ff6680]O[/color] = Opcode              [color=#66ff99]A[/color] = Register A
[color=#9966ff]0[/color]/[color=#9966ff]1[/color] = Immediate mode    [color=#66ccff]B[/color] = Register B
[color=#bbbbbb]0[/color] = Unused / zero       [color=#66ccff]I[/color] = Immediate value
[/center]

In JUMP mode, the value in the register specified by [color=#66ff99]register A[/color] are the flags and should be compared with the condition, which is the [color=#ff6680]opcode[/color]. To compare them, use the "cond" custom component you built earlier.

If the flags match the condition, overwrite the counter value with [color=#66ccff]operand B[/color].
```

### RAM

```text
In this level we add loading from and storing to the same memory that is used for the program. We want to be able to read and write 32-bit words, 16-bit words and 8-bit bytes, so add 3 extra load ports and 3 store ports to the RAM component.

Insert the newly provided port components in between the 'Instructions' load port & the 'Main Memory' RAM component.
```

```text
Once again, these are the [color=#ff9800]Modes[/color] of [color=#e49f44]Symphony[/color]:

[center]
[color=#ff9800]0[/color] IO
[color=#ff9800]1[/color] ALU
[color=#ff9800]2[/color] JUMP
[color=#ff9800]3[/color] RAM
[/center]

And the encoding looks like this:

When immediate bit is [color=#9966ff]0[/color]:
Imm bit [color=#9966ff]0[/color]: [color=#bbbbbb]0[/color][color=#ff9800]MM[/color][color=#9966ff]0[/color][color=#ff6680]OOOO[/color] [color=#ffe666]DDDD[/color][color=#66ff99]AAAA[/color] [color=#bbbbbb]0000[/color][color=#66ccff]BBBB[/color] [color=#bbbbbb]00000000[/color]
Imm bit [color=#9966ff]1[/color]: [color=#bbbbbb]0[/color][color=#ff9800]MM[/color][color=#9966ff]1[/color][color=#ff6680]OOOO[/color] [color=#ffe666]DDDD[/color][color=#66ff99]AAAA[/color] [color=#66ccff]IIIIIIII IIIIIIII[/color]

[center]
[color=#ff9800]M[/color] = Mode                [color=#ffe666]D[/color] = Destination register
[color=#ff6680]O[/color] = Opcode              [color=#66ff99]A[/color] = Register A
[color=#9966ff]0[/color]/[color=#9966ff]1[/color] = Immediate mode    [color=#66ccff]B[/color] = Register B
[color=#bbbbbb]0[/color] = Unused / zero       [color=#66ccff]I[/color] = Immediate value
[/center]
```

```text
In this level we implement the RAM mode. For now, there are six [color=#ff6680]opcodes[/color] in this mode:
[center]
[color=#ff6680]0[/color] load_8
[color=#ff6680]1[/color] load_16
[color=#ff6680]2[/color] load_32
[color=#ff6680]3[/color] [color=#bbbbbb]reserved[/color]
[color=#ff6680]4[/color] store_8
[color=#ff6680]5[/color] store_16
[color=#ff6680]6[/color] store_32
[color=#ff6680]7[/color] [color=#bbbbbb]reserved[/color]
[/center]

The 'load' instructions should load the value from program memory at address [color=#66ccff]operand B[/color] and store the result in the [color=#ffe666]destination[/color] register.

The 'store' instructions should store the value in the [color=#66ff99]operand A[/color] register to program memory at address [color=#66ccff]operand B[/color].

The numerical suffix determines the width in bits of how many bits should be written to or read from memory, so a 'load_8' instruction should load 8 bits of data & a 'store_16' should store 16 bits of data starting at that address.
```

```text
Note that ports act from top to bottom. A load port will read after a store port if it is placed lower, but before if it is placed higher. This matters when the ports load and store to the same address. In general all ports wait for store ports above them and therefore have a dependency on them. In this level the load ports must be above the store ports or you will get a circular dependency.
```

### Persistent Memory

```text
In this level we add an another RAM component, but for storing data between restarts.

Then add two new memory instructions 'pload', 'pstore', which should function like the 32 bit variants of the load and store instructions from the previous level, but accessing the 'Persistent Memory' RAM component instead of the program memory.

These are all of the [color=#ff6680]opcode[/color] encodings when in memory mode:
[center]
[color=#ff6680]0[/color] load_8
[color=#ff6680]1[/color] load_16
[color=#ff6680]2[/color] load_32
[color=#ff6680]3[/color] pload
[color=#ff6680]4[/color] store_8
[color=#ff6680]5[/color] store_16
[color=#ff6680]6[/color] store_32
[color=#ff6680]7[/color] pstore
[/center]
```

### IO Devices

```text
For this level you need to place three new IO components: the Screen, Time and Keyboard components.

Also link the screen to the RAM component named "Main Memory".
```

```text
So far IO mode has had only 3 instructions, but now it is time to add several more. This is the updated list of IO instructions:
[left]

[b]Index  Name     Operands       Description[/b]
  0    nop                     Do nothing
  1    in       dest           Input
  2    out      src_B          Output
  3    keyboard dest           Read keyboard value
  4    screen   src_A, src_B   Set screen setting
  5    time_0   dest           Read lower 32 time bits
  6    time_1   dest           Read upper 32 time bits
  7    counter  dest           Read counter value

The details of each new instruction:

[b]keyboard[/b]
The keyboard component has two output pins we want to read "Key Down" and "Key Value". Merge them into one value like so:
D = Key Down
V = Key Value
[highlight]0000000D VVVVVVVV[/highlight]

[b]screen[/b]
The screen has three inputs, "Enable", "Setting index" and "Value". When executing the [highlight]screen[/highlight] instruction, "Enable" pin should be [T], "Setting index" should be the first operand and "Value" the second operand.

[b]time_0[/b] and [b]time_1[/b]
The time component outputs the time since 1st of January 1970 in nano seconds, as a 64 bit number. Since this architecture only deals with 32 bits, reading the value is split into two instructions. However combining times from two different cycles can produce weird behavior. To avoid this issue, only [highlight]time_0[/highlight] actually reads the time component. It stores the upper half for [highlight]time_1[/highlight] to read later. This means that [highlight]time_1[/highlight] should read 0 if [highlight]time_0[/highlight] has not been executed yet.

[b]counter[/b]
Read the value of the Counter custom component you built earlier.
[/left]
```

### Instruction Aliases

```text
Like previous levels, this level introduces new instructions. However, in this level you are not supposed to add additional hardware, but instead change the assembly specification to reuse already existing instruction hardware for new instructions.

Click on the "spec.isa" tab in the Assembler window and study how instructions are implemented. Then add the following three new instructions:

[b]mov[/b]
"[b]mov[/b]" is short for "move" and takes two operands. Move the value of the second operand to the first.

[b]neg[/b]
"[b]neg[/b]" is short for "negate" and takes two operands. Negate the value of the second operand and store the result in the first.

[b]not[/b]
"[b]not[/b]" takes two operands. Not the value of the second operand and store the result in the first.


Once these instructions are added, the assembly code will be valid and running it will complete the level.
```

### Symphony ALU

```text
This is a copy of the Overture ALU. Time to upgrade it to 32 bits and a few new instructions.

Instructions:

[color=#87a8c8]0 NAND
1 OR
2 AND
3 NOR
4 ADD
5 SUB
6 XOR
7 LSL
8 LSR
9 ASR
10 CMP[/color]

The cmp operation is the one you already created a custom component for in the previous level.
```

### Comparison Flags

```text
In this level you compare two 32 bit inputs in three different ways. Each result is a 1 bit value called a "flag".

Flags:
Set bit 0 to 1 if the inputs are equal.
Set bit 1 to 1 if the first input is lower (unsigned) than the second input.
Set bit 2 to 1 if the first input is less (signed) than the second input.

To learn about making 32 bit components, open "Symphony counter" level first if you haven't already.
```

### Condition Match

```text
In this level the first input contains flags and the second input contains a condition. We need to test if the flags match the condition. Compare the three flag input bits pairwise with the first three bits of the condition input. If any of the three pairs the inputs are both [T], the condition is matched.

Finally, the 4th bit of the condition inverts the result. So if the condition matched, but the 4th bit is [T], output [F].
```

### Symphony Counter

```text
Build a 32 bit counter that increments by 4, but can alternatively be overwritten with a specific value.

Also, connect the provided memory probe to a 32-bit component holding the counter memory value.
```

```text
To change word component width, you will need to use a Bit size tool  near the left panel.

Right click on the Bit size tool to specify the target width.
Left click on components when the tool is active will set the component's bit width.
Newly placed word components will have this bit width by default.
```

### Instruction Decoder

```text
Build a circuit that splits Symphony instructions into their different parts. This will be very helpful later.

There are two encodings for Symphony instructions, depending on whether we are using immediate values or not:

[left]
              [color=#ff9800]M[/color] = Mode
              [color=#ff6680]O[/color] = Opcode
            [color=#9966ff]0[/color]/[color=#9966ff]1[/color] = Immediate mode
              [color=#ffe666]D[/color] = Destination register
              [color=#66ff99]A[/color] = Argument A
              [color=#66ccff]B[/color] = Argument B (possibly immediate)
              [color=#bbbbbb]0[/color] = Unused / zero
[/left]

[color=#bbbbbb]0[/color][color=#ff9800]MM[/color][color=#9966ff]0[/color][color=#ff6680]OOOO[/color] [color=#ffe666]DDDD[/color][color=#66ff99]AAAA[/color] [color=#bbbbbb]0000[/color][color=#66ccff]BBBB[/color] [color=#bbbbbb]00000000[/color]

[color=#bbbbbb]0[/color][color=#ff9800]MM[/color][color=#9966ff]1[/color][color=#ff6680]OOOO[/color] [color=#ffe666]DDDD[/color][color=#66ff99]AAAA[/color] [color=#66ccff]BBBBBBBB BBBBBBBB[/color]
```

```text
Use the static indexer component to solve this level.

The static indexer component is a special component that allows you to shift certain bits from a wire, without any delay or gates cost. Changing the number on the component will shift bits (positive number shifts to the right). Then click on the component to set the output width.
```

### The bus

```text
This level has 2 byte inputs and 2 byte outputs. Your goal is to move a number from one input to one output.

The first bit input, determines which input you should move from ([F] = A, [T] = B). The second bit input determines which output you should move to ([F] = X, [T] = Y).

At each cycle, only 1 output should receive the value, the other output should receive Z (no value). This will be indicated by "-" on the output.
```

### Tower of Hanoi

```text
In the game "Towers of Hanoi", you must move a stack of disks from one peg to another. The challenge is that only one disk can be moved at a time, only the top disk on a stack can be moved, and a larger disk can never be placed on top of a smaller one.
```

```text
The first 4 inputs will give you the following in order:
disk_nr - The highest disk number in the pile (2 to 4)
source - Which location number to move from
destination - Where to move the pile to
spare - The 3rd spot that is neither the source nor the destination

Control the magnet with the following outputs:
[left]
                0 - Move the magnet to spot 0
                1 - Move the magnet to spot 1
                2 - Move the magnet to spot 2
                5 - Toggle the magnet on or off
[/left]

Play with the magnet manually by using the arrow keys to move and enter to toggle.
```

```text
This level is difficult, but it is a good opportunity for you to use functions. In fact the following solution to the level contains a "recursive function", which means a function that calls itself.

[b]Tower of Hanoi algorithm:[/b]
[box][left]func move(disk_nr, source, dest, spare) {
    if disk_nr is 0 {
        move disk from source to dest
    } else {
        move(disk_nr - 1, source, spare, dest)
        move disk from source to dest
        move(disk_nr - 1, spare, dest, source)
    }
}[/left][/box]

[tip]Remember to push register values to the stack to store them in function that changes them.[/tip]
```

```text
Amazing, you finished all our tests! Look how far you have come from the NAND gate.

Humans are truly worthy of joining the Galactic Federation.
```

### XNOR Gate

```text
Most Earthlings were giant reptiles last we were here. None of them were intelligent or polite enough to join the Galactic Federation.
```

```text
Create the inverse XOR gate, known as XNOR.
```

### XOR Gate

```text
Last level you built a circuit that was [T] when we were at the second cycle. This level build a circuit that is [T] when we are at the second OR third cycle.
```

```text
This is also known as an XOR gate. This level is difficult, so don't be discouraged if you don't get the answer right away.

[tip]You can connect one output to more than 1 gates.[/tip]
```
