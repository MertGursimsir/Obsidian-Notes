### COMMENTS
	\# comment
	
	#[[
		multi line comment
	#]]
	
	##[[
		DISABLED multi line comment
	##]]


### USING CMAKE VARIABLES IN CPP
Let's say we want value of variable CMAKE_SOURCE_DIR in cpp.
Add this to cmake:
	add_definitions(-Dsomevariablename="${CMAKE_SOURCE_DIR}")
You can use variable in cpp:
	std::cout << somevariablename;
	OR
	\#ifdef somevariablename
		do_something()
	\#endif


### RUNNING CMAKE IN SCRIPT MODE
which cmake --> /usr/bin/cmake
In first line of CMakeLists.txt, add this:
	#!/usr/bin/cmake -P
Then:
	chmod +x CMakeLists.txt
Then:
	./CMakeLists.txt

or you can simply do:
	cmake -P CMakeLists.txt


### DEBUG/RELEASE MODE
2 case scenarios:
1. You are making a project which does not depend on any external library.
	- cmake -DCMAKE_BUILD_TYPE=Debug ..
	- cmake -DCMAKE_BUILD_TYPE=Release ..
	- release mod daha hızlı ve küçük olur çünkü compiler flagleri ona göre ayarlanır.

2. You want to use an external library in the Debug/Release mode.
	1. You will DOWNLOAD the external library's source codes, COMPILE it in both debug and release modes. You must have SEPERATE FOLDERS containing the debug binaries and release binaries
		1. /some/path/foo/debug/libfoo.so
		2. /some/path/foo/release/libfoo.so`
	2. Executable in my project: my_app,      External library: foo
		1. target_link_libraries(my_app 
				debug /some/path/foo/debug/libfoo.so
				optimized /some/path/foo/debug/libfoo.so)
		When we run:
			cmake -DCMAKE_BUILD_TYPE=Debug ..
				debug libfoo.so will be linked against my_app
			cmake -DCMAKE_BUILD_TYPE=Release ..
				release libfoo.so will be linked against my_app
		If you don't give CMAKE_BUILD_TYPE variable, it will give error.