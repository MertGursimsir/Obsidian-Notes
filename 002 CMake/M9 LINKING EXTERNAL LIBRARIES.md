 External library name: XYZ
 Library file name: libXYZ.so or libXYZ.a

- Download the source code
- cmake - make - sudo make install
- XYZ-config.cmake in the installed location
- Use find_package(XYZ)

What if installed library does not contain XYZ-config.cmake file?
	ask developers to provide it
	write your own Find\*cmake file
	write \*config module (NOT NECESSARY)

XYZ-config.cmake file kept inside standart installation locations.
FindXYZ.cmake is kept inside your project folder.


What if XYZ don't use CMake based build generation process?
	Even though, some packages are supported by CMake and CMake provides the needed find* modules for those packages.
	Also there are some packages which themselves provided the \*config modules, even when those packages are not built using cmake based build system.


	pkg-config:
	- keeps track of all the installed libraries
	- cmake supports this software
	- If external library has pkg-config file, we can use it to link against that lib in our cmake project.

	Even if pkg-config file is not available, we can always write our own Find* module.
	Find* module is more flexible than *cnofig


CASES:
![[Pasted image 20251001100155.png]]



Örneğin opencv için eğer CMakeLists.txt'si varsa:  
	Gerektirdiği bağımlılıkları indirirsin (apt install ...)  
	Kaynak kodu indirip cmake çalıştırırsın.  
	make dersin  
	sudo make install dersin  
  
make -j7 --------> runs building in 7 processes  
if this cause problems, dont use -j

Bu opencv'yi nasıl kullanırız?
	find_package(OpenCV REQUIRED)   ---> opencv'nin optional olmadığını söylüyoruz. Yani bulunmazsa hata atılır. REQUIRED demezsen optional olur.
	cmake will look for OpenCVConfig.cmake
	---
	linklememiz gerekir ama libopencv.so veya libopencv.a diye bir dosya yok!!!!!!
	opencv package provides a collection of various libraries
	bu libraryler bir variable'de tutulur, bu variable da OpenCVconfig.cmake dosyasında set edilir.
	
VARIABLE NAMING:
	Package name: XYZ
		Libraries: XYZ_LIBRARIES, XYZ_LIBS...
			Contains list of all libraries
		Include directories: XYZ_INCLUDES, XYZ_INCLUDE_DIRS, ...
			Contains list of all include directories

Yani OpenCVConfig.cmake dosyasına bakarak bu variable'ları görebiliriz.
	/usr/local/lib/cmake/opencv4/OpenCVConfig.cmake
		#    This file will define the following variables:
	    # - OpenCV_LIBS                              : The list of all imported targets for OpenCV modules.
		# - OpenCV_INCLUDE_DIRS          : The OpenCV include directories.
		.......

target_include_directories(DisplayImage PRIVATE ${OpenCV_INCLUDE_DIRS})
target_link_libraries(DisplayImage PRIVATE ${OpenCV_LIBS})




---


![[Pasted image 20251006103217.png]]

Library cmake based processle build edilmediyse nasıl linkleriz?
Şimdi pkg-config veya .pc dosyası olan bir library build edeceğiz.

There are following files inside a package:
	Compiled library
	Header files
	Symbolic likns
	Pkg-Config (.pc) files
	...

Eğer bu dosyaları bulamazsan xyz-dev veya xyz-devel paketlerini indirebilirsin. Böylece development dosyaları da alınmış olur.

Now we will use 
	GTK3 library in main.cpp
	Pkg-Config in CMake

sudo apt install libgtk-3-dev
	bundan sonra .pc dosyasını ararız
	find /usr -name \*gtk\*pc
		/usr/lib/x86_64-linux-gnu/pkgconfig/gtk+-3.0.pc

Bu .pc dosyasının içinde gtk-3 lib'inin ve include dir'in konumunu buluruz.
Ayrıca public ve private dependencyleri de görürüz.

Burada .pc içerisindeki lib ismini değil .pc ismini dikkate almalıyız.

find_package(PkgConfig REQUIRED)
	This will find and execute the FindPkgConfig.cmake file from the default modules folder of cmake (/usr/share/cmake/modules)
	Bunu bulduktan sonra "pkg_check_modules()" fonksiyonu kullanılabilir.
	Bu fonksiyon .pc dosyalarını bulur.
pkg_check_modules(GTK3 REQUIRED gtk+-3.0)
	first argument is prefix, can be anything
	second argument is name of the .pc file.
	pkg_search_module de aynı mantıkla çalışır ama .pc yoksa hata atmaz.
add_executable(${PROJECT_NAME}\_app main.cpp)
target_include_directories(${PROJECT_NAME}\_app PRIVATE ${GTK3_INCLUDE_DIRS})
target_link_libraries(${PROJECT_NAME}\_app PRIVATE ${GTK3_LIBRARIES})


What if .pc is not under /usr/lib that is a standard location?
Then we need to tell pkg-config where to look for.
	We need to store this path in either of two:
		CMake Variable:                     CMAKE_PREFIX_PATH
		Environment Variable:         PKG_CONFIG_PATH
Append to them, DON'T MODIFY.

![[Pasted image 20251006112949.png]]
You must add these commands before using PkgConfig tool. (before find_package)




------------------------------------



![[Pasted image 20251006113347.png]]

Assumption:
	Project: MyProject
	Executable: MyApp
	Dependency: External Library abc

abc.h   --->>  ~/Downloads/abc/include/abc.h
libabc.so --->> ~/Downloads/abc/lib/libabc.so

Simplest way is adding full paths:
	add_executable(MyApp main.cpp)
	target_include_directories(MyApp PRIVATE ~/Downloads/abc/include)
	target_link_libraries(MyApp PRIVATE ~/Downloads/abc/lib/libabc.so)


![[Pasted image 20251006115130.png]]
Aynı dizinde çok fazla header ve so dosyan varsa, so dosyalarını tek tek yazmak yerine:
	target_link_directories(~target~   ~scope~   ~dir1~   ~dir2~   ~dir3~)
Böylece so'ların aranacağı directoryleri belirtmiş olursun.
	target_include_directories(MyApp PRIVATE ~/Downloads/abc/include)
	target_link_directories(MyApp PRIVATE ~/Downloads/abc/lib)
	target_link_libraries(MyApp PRIVATE libabc.so libabc1.so libabc2.so)




![[Pasted image 20251006131524.png]]
Peki ya dizinler böyle bir şeyse? O zaman şu komutları kullanabiliriz:
	find_library(...)
		Takes:
			library name
			probable paths where the library might exists
		Outputs:
			library path
		library bulunduktan sonra location'ı bir variable'da tutulur.
		sonrasında bu variable, library'yi bir hedefe linklemek için kullanılabilir.
			--------
		find_library(~VAR~   ~LIB_NAME~   HINTS   ~PATH1~   ~PATH2~)
			at the end var will include the library location
			if we need to find "libabc.so", then LIB_NAME is abc
		What if lib is not in PATH1 but in its subdirs?
			We should add all paths PATH1 PATH2 ...
			ALSO MORE ELEGANTLY, we can use PATH_SUFFIXES option:
			find_library(abc_LIBRARY abc 
					HINTS /home/mky/Downloads/abc /opt/abc
					PATH_SUFFIXES lib lib/abc-1.14)
					![[Pasted image 20251006134140.png]]
		If you are unsure that the name of the library either:
			libabc.so
			libabc-1.14.so
			libabc-1.15.so
		Then:
			![[Pasted image 20251006134327.png]]
		abc_LIBRARY will include full path: /home/......./libabc.so
	find_path(...)
		Almost same signature with find_library. This time we finds .h
		![[Pasted image 20251006134530.png]]
		abc_INLUDE will include the path: /home/..../include
These find commands also look for some default locations:
	find_library(...)
		- /usr/lib
		- /usr/lib/x86_64-linux-gnu
	find_path(...)
		- /usr/include
		- /usr/include/x86_64-linux-gnu

DEFAULT PATHS MIGHT BE DIFFERENT DEPENDING ON YOUR CPU ARCHITECTURE




----
----
----



We are going to assume there is no .pc file for GTK library and write a FindGTK3.cmake module for it.
First thing to do is search for find module on internet. If not then we should write our own module.

Our directory structure:
![[Pasted image 20251006140622.png]]


find_package komutu modül modunda kullanılacağı için ondan önce .cmake pathini tanımlamamız gerekir:
	list(APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/modules)
	find_package(GTK3 REQUIRED)

In FindGTK3.cmake, firstly we need to find:
	gtk library
	gtk/gtk.h file



find /usr -name \*gtk\*so
	Tam olarak so'nun ismini bilmediğimiz için bununla arama yapıp bakarız.
	libgtk-3.so   ---> default path'te (/usr/lib/x86_64-linux-gnu)
find_library(GTK3_LIBRARY NAMES gtk-3)
	HINTS neden yok? Çünkü default path'te.

find /usr -name gtk.h
	it is under /usr/include/gtk-3.0/gtk/gtk.h
find_path(GTK3_INCLUDE_DIR NAMES gtk/gtk.h PATH_SUFFIXES gtk-3.0)
	/usr/include default olduğundan yalnızca PATH_SUFFIXES olarak gtk-3.0 verdik.
	NAMES öyle çünkü it should match the file in the \#include directive:
	![[Pasted image 20251006142545.png]]


![[Pasted image 20251006144014.png]]

glib.h da gerekti o yüzden onu da ekledik.

glibconfig.h not found. Yani onu da ekleyeceğiz:
![[Pasted image 20251006144053.png]]
Yaani glibconfig.h find_path için default path'te değil. HINTS eklemek gerekiyor.

![[Pasted image 20251006144412.png]]

NAMES ile verdiğimiz direkt nasıl include ediyorsak o.
Böyle böyle eksik tüm headerlar eklenir.

![[Pasted image 20251006145447.png]]
Bu hata (DSO: Dynamic Shared Object) büyük ihtimalle linker'ın bir library'yi bulamamasından kaynaklıdır.
Bu örnekte libgio-2.0.so.0 bulunamamış.
	find_library(GIO_LIBRARY NAMES gio-2.0)


Now we want to set GTK3_FOUND variable to TRUE if all libs and paths are found.
This is automatically handled by a function:
	find_package_handle_standard_args(...)
This function is defined inside another module called:
	FindPackageHandleStandardArgs.cmake

include(FindPackageHandleStandardArgs)
find_package_handle_standard_args(GTK3 DEFAULT_MSG GTK3_LIBRARY GIO_LIBRARY GTK3_INCLUDE_DIR GLIB_INCLUDE_DIR.......)
	Arguments:
		name of the package
		error message to show
		variables to check if they are set
If all of the given variables are set, then GTK3_FOUND variable will be automatically set to true.

Bu GTK3_FOUND variable'ını FindGTK3.cmake ve CMakeLists.txt dosyalarında nasıl kullanırız?
	.cmake dosyasında setlemeleri if(GTK3_FOUND) bloğunda yaparız.
	.txt dosyasında add_executable'dan önce if(NOT GTK3_FOUND) bloğunda message(FATAL_ERROR "GTK3 not found") deriz.




