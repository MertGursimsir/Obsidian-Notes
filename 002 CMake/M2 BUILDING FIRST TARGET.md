CMake generates build system files inside the directory from where cmake command was executed.

CMakeCache.txt (used by cmake in the subsequent rounds of cmake command) ve Makefile en önemli dosyalardır.


add_executable( <FINAL_EXECUTABLE>   <SOURCE_FILES>)
	build işleminin ardından executable istediğimiz durumlarda bu komut kullanılır.


cmake_minimum_required(VERSION 3.0.0)
project(Calculator_Project VERSION 1.0.0)
add_executable(calculator addition.cpp division.cpp main.cpp print_result.cpp)

Bir dosyada bir şey değiştirdiğimizde make dememiz yeter.
Değişen dosya compile edilir, sonra linklenir.

Şiiiimdi yukarıdaki gibi add_executable diyip hepsini verince, bakan biri neyin neye bağımlı olduğunu anlamaz.
Bu büyük projelerde sorun olabilir.
Projeyi modüler ve hiyerarşik yapmak doğru olacaktır.

![[Pasted image 20250923155357.png]]


Print için ve matematik operasyonlar için 2 ayrı library yapacağız.
Sonra da bunları main executable'ımıza linkleyeceğiz.


add_library(my_math addition.cpp division.cpp)
add_library(my_print print_result.cpp)
add_executable(calculator main.cpp)

Sonra ise bu library'leri executable'ımıza linklememiz lazım.

target_link_libraries( <EXECUTABLE> <LIB1> <LIB2> )

target_link_libraries(calculator my_math my_print)


Bu library'ler static. Yani executable'ın içerisine gömülüdüüüürr.


TARGETS IN CMAKE: 
	Executables
	Libraries


---

Target Properties:
	INTERFACE_LINK_DIRECTORIES
	INCLUDE_DIRECTORIES
	VERSION
	SOURCES

Properties are automatically set when we run the commands like
	target_link_libraries
	target_include_directories

Properties can be modified or retrieved by:
	set_target_properties
	set_property
	get_target_property
	get_property




IF TARGET B IS DEPENDENCY OF TARGET A:
	Target A can only be built after target B is successfully built.

my_math and my_print targets are dependencies of the calculator target.


A target is capable of propagating its properties in their dependency chain:
	target_link_libraries(calculator PUBLIC my_math my_print)
	target_link_libraries(calculator INTERFACE my_math my_print)
bu keywordler INTERFACE_LINK_LIBRARIES property'sinin setlendiğini belirtir.
This properties available to all the targets that depend upon my app.


PRIVATE is used when we dont want to set and propagate a property to other targets.
	target_link_libraries(calculator PRIVATE my_math my_print)



YOU CAN ADD MORE THAN 1 EXECUTABLE IN CMAKELISTS.TXT

YOU CANT HAVE 2 TARGETS OF THE SAME NAME

WE HAVE TARGET FILES SAVED IN COMPUTER, ALL LIBRARIES AND EXECUTABLES

