### 1.Makefile

​	[Makefile1](https://www.cntofu.com/book/25/ex2.md)

​	[Makefile2](https://www.cntofu.com/book/25/ex28.md)

CFLAGS=-g -O2 -Wall -Wextra -Isrc -rdynamic -DNDEBUG $(OPTFLAGS)
LIBS=-ldl $(OPTLIBS)
PREFIX?=/usr/local

SOURCES=$(wildcard src/***/*.c src/*.c)
OBJECTS=$(patsubst %.c,%.o,$(SOURCES))

TEST_SRC=$(wildcard tests/*_tests.c)
TESTS=$(patsubst %.c,%,$(TEST_SRC))

TARGET=build/libYOUR_LIBRARY.a
SO_TARGET=$(patsubst %.a,%.so,$(TARGET))

#### The Target Build
all: $(TARGET) $(SO_TARGET) tests

dev: CFLAGS=-g -Wall -Isrc -Wall -Wextra $(OPTFLAGS)
dev: all

$(TARGET): CFLAGS += -fPIC
$(TARGET): build $(OBJECTS)
       ar rcs $@ $(OBJECTS)
       ranlib $@

$(SO_TARGET): $(TARGET) $(OBJECTS)
       $(CC) -shared -o $@ $(OBJECTS)

build:
       @mkdir -p build
       @mkdir -p bin

#### The Unit Tests
.PHONY: tests
tests: CFLAGS += $(TARGET)
tests: $(TESTS)
       sh ./tests/runtests.sh

valgrind:
       VALGRIND="valgrind --log-file=/tmp/valgrind-%p.log" $(MAKE)

#### The Cleaner
clean:
       rm -rf build $(OBJECTS) $(TESTS)
       rm -f tests/tests.log
       find . -name "*.gc*" -exec rm {} \;
       rm -rf `find . -name "*.dSYM" -print`

#### The Install
install: all
       install -d $(DESTDIR)/$(PREFIX)/lib/
       install $(TARGET) $(DESTDIR)/$(PREFIX)/lib/

#### The Checker
BADFUNCS='[^_.>a-zA-Z0-9](str(n?cpy|n?cat|xfrm|n?dup|str|pbrk|tok|_)|stpn?cpy|a?sn?printf|byte_)'
check:
       @echo Files with potentially dangerous functions.
       @egrep $(BADFUNCS) $(SOURCES) || true



### 2.具体语法-一定多看看链接里面的内容！

[语法介绍](https://www.itsembedded.com/dhd/vivado_sim_3/)

[进阶语法及verilator功能](https://itsembedded.com/dhd/verilator_2/)

 1. 在=前面的：可以去掉字符串中的“ “。但通过：=赋值的这个变量还是字符串，要是想提取字符串内容，需要使用$()；

 2. **基本syntax：**

    target_name : dependency_1 dependency_2 dependency_3
        bash_command_that_generates_the_target <parameters>

 3. **Make has other constructs besides targets, like conditional statements or message printing commands.**

    ifeq (xxx,yyy)
    <-SPACES-> $(info "some text")
    endif

    first_target : some_dependency
    <-TAB-> some_bash_command
    <-TAB-> another_bash_command

    another_target : first_target some_other_dependency
    <-TAB-> yet_another_bash_command
    ...

 4. **Targets and dependencies** 

    就是一个依赖关系！

    cake : batter
        bake_in_oven --degrees 300C --time 1hr --input batter --output cake

    batter : flour water cocoa_powder egg_whites
        mix flour water cocoa_powder egg_whites --output batter

    egg_whites : eggs
        separate eggs egg_whites egg_yolks egg_shells

5. **Note:** You might be thinking it’s strange that the target list is, in some way, “backwards”, because instead of preparing all the ingredients and mixing the batter, we’re attempting to bake the cake right away. I suggest looking at it the other way around: imagine you’re asking `make` to bake you a cake. `make` then figures out what ingredients are missing, prepares all the ingredients, then the batter, and then bakes the cake for you.

   总而言之：就是先想好最终目标，再一层一层确定依赖关系，以Topdown的思想来写makefile，比如上面的代码。

6. ***.*PHONY**

   phony：虚假的。用phony来修饰tatget，表示这个target不会生成任何输出。`.PHONY: waves` marks the `waves` target as not generating any output files.

7. **“@”** character before `echo` prevents printout of the actual `echo` command.**If you don’t prepend “@” before a recipe command, make will show the command that it’s executing as well as the command’s output. If you do prepend “@”, it will only show the output.**

8. **使用ifeq来判断文件、变量是否存在或声明**

   ifeq ($(SOURCES),) 

   some_target :   

   ​	@echo "Print message saying that step was skipped" 

   else 

   some_target : $(SOURCES)    

   ​	run_build_command $(SOURCES) 

   endif

9. **使用？=选项来给一些可能根据配置、运行环境而变的变量赋值，因为在使用make的时候可以后面接一个参数来改变这个变量，就像verilog中例化子模块的parameter参数一样！** 

   比如： SUB ?= VHDL 	The ?= assignment only sets SUB to VHDL if SUB does not have a value

   $： make	SUB=SV

10. **`make lint` target, which calls Verilator with `--lint-only`. This is useful to quickly parse your Verilog/SystemVerilog source files and check for problems. This can be used to check over your sources even if you’re not using Verilator for simulating.**

11. 把初始化值设置为随即！**To make our testbench initialize signals to random values, we first need to call `Verilated::commandArgs(argc, argv);` before creating the DUT object:**

    Then, we need to update our verilation target build command by adding `--x-assign unique` and `--x-initial unique`. Line 31 of our Makefile should now therefore look like this:

    ```bash
    verilator -Wall --trace --x-assign unique --x-initial unique -cc $(MODULE).sv --exe tb_$(MODULE).cpp
    ```

    Lastly, we need to pass `+verilator+rand+reset+2` to our simulation executable, to set [set the runtime signal initialization technique to random](https://manpages.debian.org/unstable/verilator/verilator.1.en.html). This means changing the Line 21 in our Makefile to:

    ```bash
    @./obj_dir/V$(MODULE) +verilator+rand+reset+2
    ```