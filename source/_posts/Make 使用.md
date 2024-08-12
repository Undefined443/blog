[GNU Make](https://www.gnu.org/software/make/manual/html_node/index.html)

参考：[Make 命令教程 | 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2015/02/make.html)

## Makefile 文件的格式

Makefile 文件由一系列 `规则`（rules）构成。每条 `规则` 的形式如下。

```makefile
<target>: [prerequisites]
	[commands]
```

上面第一行冒号前面的部分，叫做 `目标`（target），冒号后面的部分叫做 `前置条件`（prerequisites）；**第二行必须由一个 tab 键起首**，后面跟着 `命令`（commands）。

`目标` 是必需的，不可省略；`前置条件` 和 `命令` 都是可选的，但是两者之中必须至少存在一个。

**每条规则就明确两件事：构建目标的前置条件是什么，以及如何构建。下面就详细讲解，每条规则的这三个组成部分。**

### 目标（target）

一个 `目标`（target）就构成一条规则。目标通常是文件名，指明 `make` 命令所要构建的对象。目标可以是一个文件名，也可以是多个文件名，之间用空格分隔。

```makefile
c : a b  # 构建 c 需要 a 和 b 两个文件
	cat a b > c  # 用 cat 命令将 a 和 b 合并为 c
```

除了文件名，目标还可以是某个操作的名字，这称为 `伪目标`（phony target）。

```makefile
clean:
	rm *.o
```

上面代码的目标是 `clean`，它不是文件名，而是一个操作的名字，属于 `伪目标`，作用是删除对象文件。

```sh
make clean
```

但是，如果当前目录中，正好有一个文件叫做 `clean`，那么这个命令不会执行。因为 `make` 发现 `clean` 文件已经存在，就认为没有必要重新构建了，就不会执行指定的 `rm` 命令。

为了避免这种情况，可以明确声明 `clean` 是 `伪目标`（phony target），写法如下。

```makefile
clean:
	rm *.o temp
.PHONY: clean
```

声明 `clean` 是 `伪目标` 之后，`make` 就不会去检查是否存在一个叫做 `clean` 的文件，而是每次运行都执行对应的命令。像 `.PHONY` 这样的内置目标名还有不少，可以查看[GNU Make 手册](https://www.gnu.org/software/make/manual/html_node/Special-Targets.html#Special-Targets)。

如果 `make` 命令运行时没有指定目标，**默认会执行 Makefile 文件的第一个目标**。

```sh
make  # 执行 Makefile 文件的第一个目标
```

### 前置条件（prerequisites）

`前置条件` 通常是一组文件名，之间用空格分隔。它指定了 `目标` 是否重新构建的判断标准：只要有一个前置文件不存在，或者有过更新（前置文件的 `last-modification` 时间戳比目标的时间戳新），`目标` 就需要重新构建。

```makefile
result: source
	cp source result
```

上面代码中，构建 `result` 的前置条件是 `source` 。如果当前目录中，`source` 已经存在，那么 `make result` 可以正常运行，否则必须再写一条规则，来生成 `source` 。

```makefile
source:
	echo "this is the source" > source
```

上面代码中，`source` 后面没有前置条件，就意味着它跟其他文件都无关，只要这个文件还不存在，每次调用 `make source`，它都会生成。

```sh
make result
make result
```

上面连续执行两次 `make result` 命令。第一次执行会先新建 `source`，然后再新建 `result`。第二次执行，`make` 发现 `source` 没有变动（时间戳早于 `result`），就不会执行任何操作，`result` 也不会重新生成。

如果需要生成多个文件，往往采用下面的写法。

```makefile
source: file1 file2 file3
```

上面代码中，`source` 是一个 `伪目标`，只有三个前置文件，没有任何对应的命令。

```sh
make source
```

执行 `make source` 命令后，就会一次性生成 `file1`，`file2`，`file3` 三个文件。这比下面的写法要方便很多。

```sh
make file1
make file2
make file3
```

### 命令（commands）

`命令`（commands）表示如何更新目标文件，**由一行或多行的 shell 命令组成**。它是构建 `目标` 的具体指令，它的运行结果通常就是生成目标文件。

**每行命令之前必须有一个 tab 键。**

需要注意的是，**每行命令在一个单独的 shell 中执行**。这些 shell 之间没有继承关系。

```makefile
var-lost:
	export foo=bar
	echo "foo=[$$foo]"
```

```sh
make var-lost
```

上面代码执行后，取不到 `foo` 的值。因为两行命令在两个不同的进程执行。一个解决办法是将两行命令写在一行，中间用分号分隔。

```makefile
var-kept:
	export foo=bar; echo "foo=[$$foo]"
```

另一个解决办法是在换行符前加反斜杠转义。

```makefile
var-kept:
	export foo=bar; \
	echo "foo=[$$foo]"
```

最后一个方法是加上 `.ONESHELL:` 命令。

```makefile
.ONESHELL:
var-kept:
	export foo=bar;
	echo "foo=[$$foo]"
```

> 调用 shell 变量需要两个美元符号

---

读完上面你就已经掌握了 Makefile 的基本内容。

## Makefile 文件的语法

### 注释

井号 `#` 在 Makefile 中表示注释。

```makefile
# 这是注释
result.txt: source.txt
	# 这是注释
	cp source.txt result.txt  # 这也是注释
```

### 回声（echoing）

正常情况下，`make` 会打印每条命令，然后再执行，这就叫做回声（echoing）。

```makefile
test:
	# 这是测试
```

执行上面的规则，会得到下面的结果。

```sh
$ make test
# 这是测试
```

在命令的前面加上 `@`，就可以关闭回声。

```makefile
test:
	@# 这是测试
```

现在再执行 `make test`，就不会有任何输出。

由于在构建过程中，需要了解当前在执行哪条命令，所以通常只在注释和纯显示的 echo 命令前面加上 `@`。

```makefile
test:
	@# 这是测试
	@echo TODO
```

### 通配符

`通配符`（wildcard）用来指定一组符合条件的文件名。Makefile 的通配符与 bash 一致，主要有星号 `*`、问号 `?` 和 `[ ]`。比如，`*.o` 表示所有后缀名为 `.o` 的文件。

```makefile
clean:
	rm -f *.o
```

### 模式匹配

`make` 命令允许对文件名进行类似正则运算的匹配，主要用到的匹配符是 `%`。比如，假定当前目录下有 `f1.c` 和 `f2.c` 两个源码文件，需要将它们编译为对应的对象文件。

```makefile
%.o: %.c
```

等同于下面的写法。

```makefile
f1.o: f1.c
f2.o: f2.c
```

使用匹配符 `%`，可以将大量同类型的文件，只用一条规则就完成构建。

### 后缀规则

后缀规则在 Makefile 中用于定义如何从一种文件类型转换成另一种文件类型。

下面是几个关键点来帮助理解 Makefile 后缀规则（又称为隐晦规则）的功能：

1. **定义**：后缀规则通过一个点（`.`）后接一系列字符的形式来标识文件类型。这种规则说明了如何将一种后缀名的文件转换为另一种后缀名的文件。例如，从 `.c` 文件编译生成 `.o`（对象文件）的规则。

2. **格式**：一个典型的后缀规则看起来像这样：
   ```makefile
   .suffix1.suffix2:
   	command
   ```
   其中 `suffix1` 表明源文件类型，`suffix2` 表明目标文件类型，而 `command` 是执行的命令行，用以从 `suffix1` 类型文件生成 `suffix2` 类型文件。

3. **例子**：如果有 `.c` 到 `.o` 的转换规则，它可能看起来像这样：
   ```makefile
   .c.o:
   	$(CC) -c $(CFLAGS) $< -o $@
   ```
   在这个例子中，`.c.o:` 定义了从 `.c` 源代码文件编译生成 `.o` 对象文件的规则。`$(CC)` 是编译器（通常是 `gcc` 或 `clang`），`$(CFLAGS)` 包含编译器标志，`$<` 是规则的依赖项（这里是 `.c` 文件），`$@` 是规则的目标（这里是 `.o` 文件）。

4. **优点**：使用后缀规则可以简洁地定义文件转换方法，减少 Makefile 的复杂度。这种规则特别适用于简单项目或者那些文件转换步骤非常标准化的场景。

5. **局限性**：虽然后缀规则在某些情况下很有用，但它们不具备模式规则的灵活性和强大功能。随着项目复杂度增加，使用模式规则（使用`%`通配符）可能更合适，因为它们允许更为复杂和灵活的文件名处理。

6. **过时警告**：值得注意的是，在新版的 `make` 工具中，后缀规则有时被视为过时的，推荐使用更灵活强大的模式规则。尽管如此，在理解遗留项目或维护较老的构建系统时，了解后缀规则仍然很重要。

### 变量和赋值符

Makefile 允许使用等号自定义变量。

```makefile
txt = Hello World
test:
	@echo $(txt)
```

上面代码中，变量 `txt` 等于 Hello World。调用时，变量需要放在 `$( )` 之中。

**调用 shell 变量，需要在美元符号前，再加一个美元符号（$$）**，这是因为 `make` 命令会对美元符号转义。

```makefile
test:
	@echo $$HOME
```

有时，变量的值可能指向另一个变量。

```makefile
v1 = $(v2)
```

上面代码中，变量 `v1` 的值是另一个变量 `v2`。这时会产生一个问题，`v1` 的值到底在定义时扩展（静态扩展），还是在运行时扩展（动态扩展）？如果 `v2` 的值是动态的，这两种扩展方式的结果可能会差异很大。

为了解决类似问题，Makefile 一共提供了四个赋值运算符（`=`、`:=`、`?=`、`+=`）：

```makefile
# 在执行时扩展，允许递归扩展
VARIABLE = value

# 在定义时扩展
VARIABLE := value

# 只有在该变量为空时才设置值
VARIABLE ?= value

# 将值追加到变量的尾端
VARIABLE += value
```

更加具体的区别参阅 [What is the difference between the GNU Makefile variable assignments =, ?=, := and +=? | StackOverflow](https://stackoverflow.com/questions/448910/makefile-variable-assignment)

### 内置变量（Implicit Variables）

`make` 命令提供一系列内置变量，比如，`$(CC)` 指向当前使用的编译器（如 GCC），`$(MAKE)` 指向当前使用的 make 工具。这主要是为了跨平台的兼容性，详细的内置变量清单见 [GNU Make 手册](https://www.gnu.org/software/make/manual/html_node/Implicit-Variables.html)。

```makefile
output:
	$(CC) -o output input.c
```

### 自动变量（Automatic Variables）

`make` 命令还提供一些自动变量，它们的值与当前规则有关。主要有以下几个。

1. `$@`

`$@` 指代当前目标，就是 `make` 命令当前构建的那个目标。比如，`make foo` 的 `$@` 就指代 `foo`。

```makefile
a b:
	touch $@
```

等同于下面的写法。

```makefile
a:
	touch a
b:
	touch b
```

2. `$<`

`$<` 指代第一个前置条件。比如，规则为 `t: p1 p2`，那么 `$<` 就指代 `p1`。

```makefile
a: b c
	cp $< $@
```

等同于下面的写法。

```makefile
a: b c
	cp b a
```

3. `$?`

`$?` 指代比目标更新的所有前置条件，之间以空格分隔。比如，规则为 `t: p1 p2`，其中 `p2` 的时间戳比 `t` 新，`$?`就指代 `p2`。

4. `$^`

`$^` 指代所有前置条件，之间以空格分隔。比如，规则为 `t: p1 p2`，那么 `$^` 就指代 `p1 p2` 。

5. `$*`

`$*` 指代匹配符 `%` 匹配的部分， 比如 `%` 匹配 `f1.txt` 中的 `f1`，`$*` 就表示 `f1`。

6. `$(@D)` 和 `$(@F)`

`$(@D)` 和 `$(@F)` 分别指向 `$@` 的目录名和文件名。比如，`$@` 是 `src/input.c`，那么 `$(@D)` 的值为 `src`，`$(@F)` 的值为 `input.c`。

7. `$(<D)` 和 `$(<F)`

`$(<D)` 和 `$(<F)` 分别指向 `$<` 的目录名和文件名。

所有的自动变量清单，请看 [GNU Make 手册](https://www.gnu.org/software/make/manual/html_node/Automatic-Variables.html)。下面是自动变量的一个例子。

```makefile
dest/%.txt: src/%.txt
	@[ -d dest ] || mkdir dest
	cp $< $@
```

上面代码将 `src` 目录下的 `txt` 文件，拷贝到 `dest` 目录下。

代码首先判断 `dest` 目录是否存在，如果不存在就新建，然后，`$<` 指代前置文件（`src/%.txt`）， `$@` 指代目标文件（`dest/%.txt`）。

---

下面的内容不太常用

### 判断和循环

Makefile 使用 bash 语法，完成判断和循环。

```makefile
ifeq ($(CC),gcc)
  libs=$(libs_for_gcc)
else
  libs=$(normal_libs)
endif
```

上面代码判断当前编译器是否为 gcc ，然后指定不同的库文件。

```makefile
LIST = one two three
all:
	for i in $(LIST); do \
		echo $$i; \
	done

# 等同于

all:
	for i in one two three; do \
		echo $i; \
	done
```

上面代码的运行结果：

```
one
two
three
```

### 函数

Makefile 还可以使用函数，格式如下。

```makefile
$(function arguments)
# 或者
${function arguments}
```

Makefile 提供了许多[内置函数](https://www.gnu.org/software/make/manual/html_node/Functions.html)可供调用，下面是几个常用的内置函数。

1. shell 函数

`shell` 函数用来执行 shell 命令

```makefile
srcfiles := $(shell echo src/{00..99}.txt)
```

2. wildcard 函数

`wildcard` 函数用来在 Makefile 中替换 bash 的通配符。

```makefile
srcfiles := $(wildcard src/*.txt)
```

3. subst 函数

`subst` 函数用来文本替换，格式如下。

```makefile
$(subst from,to,text)
```

下面的例子将字符串 `feet on the street` 替换成 `fEEt on the strEEt`。

```makefile
$(subst ee,EE,feet on the street)
```

下面是一个稍微复杂的例子。

```makefile
comma := ,
empty :=
# space 变量用两个空变量作为标识符，当中是一个空格
space := $(empty) $(empty)
foo := a b c
bar := $(subst $(space),$(comma),$(foo))
# bar is now 'a,b,c'.
```

4. patsubst 函数

`patsubst` 函数用于模式匹配的替换，格式如下。

```makefile
$(patsubst pattern,replacement,text)
```

下面的例子将文件名 `x.c.c bar.c`，替换成 `x.c.o bar.o`。

```makefile
$(patsubst %.c,%.o,x.c.c bar.c)
```

5. 替换后缀名

替换后缀名函数的写法是：`变量名:后缀名替换规则`。它实际上是 `patsubst` 函数的一种简写形式。

```makefile
min: $(OUTPUT:.js=.min.js)
```

上面代码的意思是，将变量 `OUTPUT` 中的后缀名 `.js` 全部替换成 `.min.js`。

## Makefile 实例

1. 执行多个目标

```makefile
.PHONY: cleanall cleanobj cleandiff

cleanall: cleanobj cleandiff
	rm program

cleanobj:
	rm *.o

cleandiff:
	rm *.diff
```

上面代码可以调用不同目标，删除不同后缀名的文件，也可以调用一个目标（`cleanall`），删除所有指定类型的文件。

2. 编译 C 语言项目

```makefile
edit: main.o kbd.o command.o display.o
	cc -o edit main.o kbd.o command.o display.o

main.o: main.c defs.h
	cc -c main.c
kbd.o: kbd.c defs.h command.h
	cc -c kbd.c
command.o: command.c defs.h command.h
	cc -c command.c
display.o: display.c defs.h
	cc -c display.c

clean:
	rm edit main.o kbd.o command.o display.o

.PHONY: edit clean
```

[如何用 Make 来构建 Node.js 项目 | 阮一峰的网络日志](https://www.ruanyifeng.com/blog/2015/03/build-website-with-make.html)
