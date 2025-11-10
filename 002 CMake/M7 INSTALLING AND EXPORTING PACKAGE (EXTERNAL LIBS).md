![[Pasted image 20250929162224.png]]
Using a package:
	download
	compile
	install:
		compiled libraries (executables), header files, supporting files are copied from package directory to one of the predefined directories.
		In linux this directory is "**/usr/local**"
		copying the files from one folder to another
		

install()
	CMAKE_INSTALL_PREFIX  ---> /usr/local
	![[Pasted image 20250930080552.png]]
	for my_math target, we have addition.h and division.h headers
	in my_math's CMakeLists.txt file:
		install(FILES
			${CMAKE_CURRENT_SOURCE_DIR}/include/my_math/addition.h
			${CMAKE_CURRENT_SOURCE_DIR}/include/my_math/division.h
			DESTINATION
			${CMAKE_INSTALL_PREFIX}/include/my_math)
		install(TARGETS my_math
			DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/my_math)
	AFTER cmake .. COMMAND, WE SHOULD DO "sudo make install"

find_package()
	If you want to use my_math library in any other project, we need to find it.
	find_package(ABC)
		==searches for== ABC-config.cmake file
		==inside== /usr/local/lib/ABC

3 steps for your package to be found by find_package:
	add targets to export group	
	install the export group
	modify the target_include_directories()

install(TARGETS my_math EXPORT my_export DESTINATION ...)
install(EXPORT my_export FILE my_math-config.cmake DESTINATION ${CMAKE_INSTALL_PREFIX}/lib/my_math)
my_math-config.cmake file will be created, we need to install it too
EXPORT demek --> target tanımlarımı kaydet ki başka projeler bu target'ı kullanabilsin. Bu oluşturulan cmake dosyasında target'ın ismi, include path'leri linked libs vs. bulunur.
##############################################################
Bu şekilde HATA alırız.
![[Pasted image 20250930093904.png]]
target_include_directories dediğinde INTERFACE_INCLUDE_DIRECTORIES property'si otomatik hatada gözüken path'e set edilir.
EXPORT dediğinde bu property de export edilecek. Yani my_math library'sini kullanan herhangi başka biri de aynı include directory'sini kullanmalı.
BUNU ENGELLEMEK İÇİN GENERATOR EXPRESSIONS KULLANILIR.
These expressions are not resolved in the configuration stage but in building stage of cmake.
Bu sayede bu kütüphaneyi kullananlar kendi istedikleri include directoryleri kullanabilirler.
	target_include_directories(my_math PUBLIC 
		$<INSTALL_INTERFACE:include>
		$<BUILD_INTERFACE:\${CMAKE_CURRENT_SOURCE_DIR}/include>)	


USING PACKAGE
---
cmake_minimum_required(VERSION 3.0.0)
project(MY_MATH_TEST VERSION 1.0.0)
find_package(my_math)
if(my_math_FOUND)
	message("my_math found")
    add_executable(calc main.cpp)
    target_link_libraries(calc my_math)
else()
	message(FATAL_ERROR "my_math library not found")
endif()


find_package COMMAND:
1. module mode:  Findmy_math.cmake
2. config mode:     my_math-config.cmake

USING PACKAGE örneğimizde config mode kullandık.
module mode kullanmak istersen: 
	find_package(my_math MODULE)
config mode kullanmak istersen:
	find_package(my_math CONFIG)
Hiç vermezsen önce module mode bakılır, yoksa config mode kullanılır.

2 modun FARKI ne?
	MODULE modda cmake dosyası ilk olarak CMAKE_MODULE_PATH değişkeniyle belirtilen directorylerde aranır. Recommended to find packages that is inside our project.
	CONFIG mod genelde installed paketi kullanırken kullanılır.


cmake ..
make 
	Böyle derken cmake .. demeyi unutursak, make komutu otomatik cmake komutunu çağırır.
	İLK ÇALIŞTIRMA HARİÇ



