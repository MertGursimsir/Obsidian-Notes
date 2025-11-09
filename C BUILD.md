PROJE ŞEMASI:
	env
		- include
		- lib
	src
		- main.cpp

**g++ -Ipp_env/include -c src/main.cpp -o main.o**
	-I ile include pathi belirtiriz, main.cpp'yi 
	-c sadece derle, object file oluştur (just compile, dont link)
**g++ -Ipp_env/include -c pp_env/lib/math_utils.cpp -o math_utils.o**
**g++ main.o math_utils.o program**
	object dosyaları birleştir ve executable oluştur


ifndef - define - endif --> aynı headerı birden çok include etmeyelim diye
extern --> bu fonksiyon diğer dilde yazılmış kodla interact edecek. generated assemblynin ABI'ye (application binary interface) uyduğundan emin ol dersin compilera. (Bazı dillerin assembly halleri diğer dilin assembly haline uymayabilir.)


main.c
PRE-PROCESSOR
comments removed, macros expanded, resolve conditional compilation, resolve includes (replace that line with contents of the header file)
main.i
COMPILER
translates to assembly
main.s
ASSEMBLER
translates to machine code
main.o
LINKER
resolves position within the binary where functions will be placed
we may have multiple object files, linker combine them into a single self-contained executable. 2 ways:
	static -> takes machine code of each required function from library and copy it into final executable
	dynamic -> libraries are precompiled into dynamic shared library (so) file. they dont contain start point to start execution. linker wont copy functions from library directly into executable, instead it will insert a reference to the library that contains machine instructions for that function.
		At runtime if program needs a function from dynamic library, os will load required function into program's address so program can use it as if it were part of the executable.


efficient olması gereken kısımları assembly yazıp birleştirebilirsin.

gcc doesnt support just c. it also supports c++ objective-c fortran ada d go.

gcc originally was gnu c compiler.
it evolved into (GNU compiler collection)


