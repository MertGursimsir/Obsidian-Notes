Makefile:
![[Pasted image 20250610081009.png]]
make all    IS SAME AS   make
	all is default
Here "make" = "make all" = "make hellow"

![[Pasted image 20250610094057.png]]


CMake:
	To create makefile
	uses a config file called CMakeLists.txt

I have:
	/home/mert/projects/testProject/src/hellow
			- hellow.c
			- CMakeLists.txt
			- build/
				Here run "cmake .."
				Then we got in build/:
					- Makefile
					- CMakeCache.txt
					- cmake_install.cmake
					- CMakeFiles/
					Run make here "build/" and we got:
						- hellow --> binary

CMakeLists.txt:
	cmake_minimum_required(VERSION 3.10)
	project(hellow)
	add_executable(hellow hellow.c)

CMakeLists.txt:
	cmake_minimum_required(VERSION 3.10)
	project(main)
	add_executable(main main.c random.c random.h)
	target_link_libraries(main PRIVATE m)

add_executable'daki sonra gelenler ilkini oluşturmak için bir araya gelir.
target_link_libraries'teki m, math library için
We got in src:
	main.c --> includes random.h
	random.c --> includes math.h
	random.h --> just a function header
	CMakeLists.txt

gcc ile yapsak:
	gcc -o main main.c random.c -lm
		output filename
		source file 1
		source file 2
		link with maths library (-lm)
	OR
	gcc -c main.c
	gcc -c random.c
	gcc -o main main.o random.o -lm