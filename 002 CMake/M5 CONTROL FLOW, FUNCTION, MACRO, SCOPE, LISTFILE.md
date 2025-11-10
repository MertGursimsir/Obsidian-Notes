
if (condition)
	commands
elseif (condition)
	commands
else
	commands
endif()


TRUE:
	1, ON, YES, TRUE, Y, NON-ZERO NUMBER
FALSE:
	0, OFF, NO, FALSE, N, IGNORE, NOTFOUND, EMPTY STRING, STRING ENDING WITH -NOTFOUND


set(VAR YES)
if (VAR)   ----->> dont dereference, condition does it automatically, JUST ONCE
	eğer iç içe bir şekilde tanımlıysa, örneğin set(VAR2 VAR) dersen ve if (VAR2) dersen VAR2 bir kere açılır. Tekrar açmak için if(${VAR2})

---

Unary tests
	DEFINED
		Checks if variable is set
	COMMAND
		Checks if command exists or not
	EXISTS
		Checks if file or directory exists or not


set(Name Mert)
if (DEFINED Name) ---> enters bc Name is defined

if (COMMAND target_link_libraries)  ---> enters bc this is a command

if (EXISTS /home/mert/Desktop/test.txt) ---> dosya varsa girer


---

Binary conditions:
	(VAR1 STRLESS VAR2)
	(VAR1 STRGREATER VAR2)
	(VAR1 STREQUAL VAR2)

---
Boolean operators:
	NOT
		if (NOT DEFINED VAR)
	OR
	AND



===

LOOPS

while(condition)
	commands
endwhile()


foreach(variable items)
	commands
endforeach()

--> foreach(Name Alice Bob Charlie)
--> foreach(Name Alice;Bob;Charlie)
	Name becames Alice, Bob, Charlie in order


--> foreach(x RANGE 10)
	0 to 10
--> foreach(x RANGE 10 20)
	10 to 20
--> foreach(x RANGE 10 20 3)
	10 to 20 with step size 3
ilk ve son sayılar dahil


--> foreach(x IN LISTS ~list1~  ~list2~  ~list3~)



===


FUNCTIONS

function(FUNCTION_NAME  FUNCTION_ARGS)
	COMMANDS
endfunction()

set(Name Mert)
function(${Mert})
	can be run at anywhere in same script

Aynı isimde art arda 2 fonksiyon tanımlarsan:
	direkt fonksiyon ismiyle SONRA tanımladığın fonksiyon çağrılır
	 \_function ile İLK tanımladığın fonksiyon çağrılır.



function(my_func var)
	...
endfunction()

my_func(var1 var2)
	if you call func like this:
		var1 becomes ARGV0 (named argument)
		var2 becomes ARGV1 (optional argument)

![[Pasted image 20250929102739.png]]

if (DEFINED ARGV1)
	bu şekilde de yapılabilir.



 fonksiyon çağrıldıktan sonra parent scope'daki tüm değişkenler kopyalanır, yani fonksiyon içerisinden değiştirebilirsin.
 AMA fonksiyonda yaptığın değişiklik dışarıyı etkilemez, çünkü kopyalandı.

Dışarıdakini değiştirmenin bir yolu var. Bunu return etmek gibi kullanabiliriz.

set(Name Mert)
function(prints)
	set(Name Ahsen PARENT_SCOPE)
	message(${Name}) --------> still prints Mert bc its function scope
endfunction()

prints()
message(${Name}) --------> changes to Ahsen

![[Pasted image 20250929104953.png]]

APPEND falan dersen (list() veya string() ile) yine bu function scope'da yapılır.



===

function call gibi, add_subdirectory de yeni bir scope oluşturur. --> directory scope

![[Pasted image 20250929105334.png]]
top one is ROOT level
![[Pasted image 20250929105425.png]]
Charlie

Charlie
Bob

Charlie


directory'deki cmake'de yine set(Name Bob PARENT_SCOPE) diyebilirsin:
	Charlie
	Charlie
	Charlie
	Bob


SADECE ADD_SUBDIRECTORY VE FUNCTION yeni scope oluşturur.



===

MACRO

macro(NAME ARGS)
	...
endmacro()

fonksiyondan farkı, macro'nun PARENT SCOPE kullanmasıdır.
macro is like #define, like just string replacement


![[Pasted image 20250929144041.png]]
In set command, we are creating a new variable within current scope (parent scope).
macros do not introduce a new scope, any changes made inside a macro is always a part of the current scope. 
In macro, it is like: ${name_var} --> Charlie

CMAKE IS CASE INSENSITIVE 


====


CMakeLists.txt files are called the ListFiles
Bu listfiles dışında cmake kodlarının yazıldığı MODULE kavramı vardır.
Bu modüller .cmake uzantısıyla biter, include ederek kullanılır:

include(ProcessorCount)
ProcessorCount(VAR)
message("Number of processors: ${VAR}")

standard modules are in: /usr/share/cmake-3.22/Modules



Kendi modülümüzü de yazabiliriz:
1. +module7
	1.    -CMakeLists.txt
	2.    -my_module.cmake
	3.    +build

CMakeLists.txt:
1. cmake_minimum_required(VERSION 3.0.0)
2. project(Calculator_Project VERSION 1.0.0)
3. include(my_module)
my_module.cmake:
4. message("Hello from the my_module.cmake file!")

Şimdi cmake dersek hata verir çünkü .cmake dosyasını içeren path'ı tanımlamalıyız.
Path'i tanımlamak için CMAKE_MODULE_PATH variable'ımız vardır.
Bu variable module'leri aramak için path'ler listesi içerir.
Bu bir cache variable'dır.

CMakeLists.txt böyle olmalı:
1. cmake_minimum_required(VERSION 3.0.0)
2. project(Calculator_Project VERSION 1.0.0)
3. list(APPEND  CMAKE_MODULE_PATH  ~path-to-module5-directory~)
4. include(my_module)

add_subdirectory'ye benziyor.
AMA include dediğimizde yeni scope oluşturmuyoruz.

Yani .cmake dosyamızda bir değişken set eder veya bir değişkeni modify edersek ana CMakeLists.txt'mizde bu değişikliği görürüz.

Reusable code ve ana CMakeLists.txt'nin uzunluğunu readability için azaltmak için modüller kullanılır.