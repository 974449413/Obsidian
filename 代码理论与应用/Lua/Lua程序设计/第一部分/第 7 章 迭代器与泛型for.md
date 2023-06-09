# 迭代器与泛型for
---

# 7.1 迭代器与 closure
---

closure：闭包
- 个人理解：即使用引用捕获捕获列表的 lambda 表达式

迭代器：一种可以遍历一种集合中所有元素的机制
- Lua 中，将迭代器表示为函数，即每调用一次函数，即返回集合中的"下一个"元素

迭代器：
- 每个迭代器都需要在每次成功调用之间保持一些状态
- 以知道它当前的位置，以及如何进入到下一个位置

closure 结构一般涉及两个函数：
- closure 本身
- 用于创建该 closure 的工厂函数

为数组编写一个简单的迭代器：
```
function values(t)
	local i = 0
	return function() i = i + 1; return t[i] end
end
```

这个 values 就是一个工厂：
- 每当调用这个工厂的时候，它就创建一个新的 closure
- 这个 closure 将它的状态保存在外部遍历 t 和 i 中，每当调用这个迭代器时，它就从列表 t 上返回下一个值
- 直到最后一个元素返回后，迭代器就会返回 nil，以此表示迭代器的结束

可以在 while 循环中使用这个迭代器：
```
t = {10, 20, 30}
iter = values(t) -- 创建迭代器
while true do
	local element = iter() -- 调用迭代器
	if element == nil then
		break
	end
	print(element)
 end
```

使用泛型 for 则更为简单，它正是为这种迭代而设计的：
```
t = {10, 20, 30}
for element in values(t) do
	print(element)
end
```

再次纠正泛型 for 的语法：
```
for 返回值(可能有多个) in 创建迭代器的函数(参数) do
	<content>
end
```

泛型 for 为一次迭代循环做了所有的薄记工作：
- 它在内部保存了迭代器函数，不再需要迭代器变量
- 每次更新迭代时调用迭代器
- 在迭代器返回 nil 时结束循环

总结，如何写创建迭代器的函数：
```
function createfactory(参数)
	定义起始索引减 1
	return function iter() 索引 = 索引 + 1 return 参数[索引] end
```
目的：
- 相当于在迭代器外部维持了一个全局变量
- 每次调用，都会用这个全局变量去进行索引
- 所以迭代器可以根据调用的次数，去返回对应的值

# 7.2 泛型 for 的语义
---

上述的迭代器有一个缺点：
- 需要为每个新的循环都创建一个新的 closure
- 在有些情况下，创建一个新的 closure 的开销还是蛮大的
- 因此在这种情况下，我们希望通过泛型 for 的自身来保存这些迭代器的状态

泛型 for 在循环过程中保存了迭代器的函数，实际上它保存了 3 个值：
- 迭代器函数
- 恒定状态
- 控制变量

泛型 for 的语法：
```
for <var-list> in <exp-list> do
	<body>
end
```
- \<var-list>：是一个或多个变量的列表，以逗号分割
	- 变量列表的第一元素称为"控制元素"，在循环过程中该值绝不会为 nil
	- 当这个值为 nil 的时候，循环就结束了
- \<exp-list>：是一个或多个表达式的列表，以逗号分割
	- 通常表达式只有一个元素，即对迭代器工厂的调用

for 做的第一件事就是对 in 后面的表达式求值，这些表达式返回 3 个值供 for 保存：
- 迭代器函数
- 恒定状态(指的就是表达式)
- 控制变量的初值

多余值会被丢弃，不足的话以 nil 补足：
- 类似于多重赋值了

之后，for 的做法：
- 会以恒定状态和控制变量来调用迭代器函数
- 然后 for 将迭代器函数返回值赋予变量列表中的变量
	- 一旦第一个返回值为 nil，那么循环终止；
	- 否则 for 执行它的循环体，随后再次调用迭代器函数，并重复这个过程

等价于以下代码：
```
do
	local _f, _s, _var = <explist>
	while true do
		 local var_1, ..., var_n = _f(_s, _var)
		 _var = var_1
		 if _var == nil then
			 break
		 end
	end
end
```

# 7.3 无状态的迭代器
---

无状态迭代器：一种自身不保存任何状态的迭代器
- 因此我们可以在多个循环中使用同一个无状态的迭代器，避免创建新的 closure 开销
- 即每次迭代都以恒定的状态和控制变量来调用迭代器函数
- 一个典型的例子就是 ipairs

for 循环调用迭代器创建函数的时候，要返回三个变量：
- 迭代器函数
- 恒定状态(指的就是遍历的对象)
- 当前索引值(控制变量)

pair 返回的是 Lua 中的一个基本函数作为迭代器：
- next

有些用户喜欢直接使用 next：
```
for k,v in next,t do
	<loop body>
end
```
Lua 会自动将 for 循环中表达式列表的结果调整为 3 个值：
- 上例中得到了 next、t 和 nil
- 其实后面的就是调个函数，得到迭代器函数，表，初始索引罢了

总结：
- 就是如果迭代器内部维护了一个索引变量，那么它就是无状态迭代器
	- 可以对同一个状态创建不同的迭代器来遍历
	- 它们之间互不干扰
- 如果迭代器通过外部某一个索引变量来进行变量，那么它就是有状态迭代器

# 7.4 具有复杂状态的迭代器
---

通常，迭代器需要保存许多状态；但是泛型 for 只能提供：
- 一个恒定的状态
- 一个控制变量

解决办法：
- 使用 closure
- 将迭代器所需的所有状态都打包为一个 table，保存在恒定状态中

虽然在循环过程中，恒定状态总是同一个 table：
- 但是这个 table 的内容却可以发生改变
- 所以这种迭代器可以在恒定状态保存所有数据，就可以忽略泛型 for 提供的第二个参数了
	- 即控制变量参数

- 尽可能尝试编写无状态迭代器
- 如果无法套用这个模型，就应该尝试使用 closure

# 7.5 真正的迭代器
---

我们之前说的其实是生成器，它没有做实际的迭代