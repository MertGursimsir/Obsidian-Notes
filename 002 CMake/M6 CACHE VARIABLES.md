![[Pasted image 20250929151015.png]]


GLOBAL SCOPE
--
2 types of global variables:
- Persistent cache variables
- Environment variables

build alınca oluşan CMakeCache.txt dosyasındaki variableların hepsi cache variablelarıdır.

Cache variables:
	set by CMake, depending on the development environment
	set by commands inside CMakeLists.txt
		project(Calculator VERSION 1.0.0)
			bu komut da cache variable set eder
		set(A "123" CACHE STRING "This command sets variable A in persistent cache")
			CACHE: specifies that variable belongs to the global scope and has to be stored in CMakeCache.txt file.
			STRING: variable type, followed by summary of variable
			In cmakecache file:
				![[Pasted image 20250929153510.png]]

Cache variable dereferencing:
	$CACHE{variable_name}
message($CACHE{A})

If you just do ${A} and A is not set with like set(A "000"), then you can still access it.
CACHE ile dereference edersek direkt persistent cache file'a bakılır, local olarak bakılmaz.


Environment variables:
	global scope again
	not stored in CMakeCache.txt

set(ENV VAR_NAME VAR_VALUE)
$ENV{VAR_NAME}



=== 

MODIFICATION OF CACHE VARIABLES

Başkasının geliştirdiği library'yi install ederken cache variableları değiştirmek gerekebilir.
Örneğin şu şekilde setleyelim:
	set(Name Mert CACHE STRING "Name variable")
	message($CACHE{Name})
build aldıktan sonra CMakeCache.txt içerisinde Name değişkenini görebiliriz.
sonra:
	set(Name Ahsen CACHE STRING "Name variable")
dersek ve build alırsak, Name değişkeni değişmez.

1. Run cmake first time
2. CMakeCache.txt is created
3. Modify/Remove previous cache variables ---->> THIS WILL BE REJECTED

3 options to modify cache variable:
1. Edit CMakeCache.txt file
2. Use FORCE keyword
	1. set(Name Ahsen CACHE STRING "Name variable" FORCE)
	2. not recommended
	3. higher priority over -D
3. Use -D flag
	1. cmake -DName=Ahsen ..
	2. recommended


===


Cache variables:
	- CMAKE_VERSION
	- CMAKE_MAJOR_VERSION
	- CMAKE_MINOR_VERSION
	- CMAKE_PATCH_VERSION
	- --
	- CMAKE_PROJECT_NAME
	- PROJECT_NAME
		- Top levelde ve her bir subdirectory'de ayrıca project ile project name verebiliriz.
		- CMAKE_PROJECT_NAME: Root level
		- PROJECT_NAME: Current (most recent)
	- --
	- CMAKE_GENERATOR
		- Tells CMAKE about the build system
		- Also can be changed with -G option
		- 