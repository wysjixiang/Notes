1. **断言的重要性！用于检测输出。**

2. **在C++中enable在sv文件中声明的enum logic变量Making the `operation_t` typedef available in the C++ testbench**

   [具体查看解释用法查看该链接](https://www.itsembedded.com/dhd/verilator_3/)

   Before we can start checking that our ALU correctly adds or subtracts two numbers, we must first make our operation typedef available for us in our `tb_alu.cpp` testbench.

   At the top of our ALU code, we have the following:

   ```verilog
   typedef enum logic [1:0] {
        add     = 2'h1,
        sub     = 2'h2,
        nop     = 2'h0
   } operation_t /*verilator public*/;
   ```

   Notice the `/*verilator public*/` comment, after the `operation_t` name. This tells verilator to convert this typedef to C++, and make it publicly available - this happens during the [verilation (HDL to C++ conversion) step](https://www.itsembedded.com/dhd/verilator_1/#conversion-results).

   These types of comments are called **directives** （**指令**）, or **pragmas** （**编译注释**）- they give Verilator additional information on how to process your HDL code. You can find a list of them in the [Verilator Language Extensions guide](https://verilator.org/guide/latest/extensions.html).

   If we wouldn’t have added the `/*verilator public*/` comment, this typedef would not be provided in our header file.

   ***NOTE:*** If the DUT you’re working on has submodules, and you use the `public` directive inside any of the submodules, you may need to find and include additional header files generated from those specific submodules.

   