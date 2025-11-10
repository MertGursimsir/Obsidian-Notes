cmake -P ~script_name~ -----------> script mode, no build files are generated

printing:
	message(~optional_display_mode~   "hello world")

Display modes:
	STATUS
	DEBUG
	WARNING
	FATAL ERROR

message("hello world") -------------then----------> cmake -P CMakeLists.txt

project command is not scriptable so we are not using it.
We can use cmake_minimum_required




We can set variables:
	set(~variable_name~   ~variable_value~)
all variables are of STRING type.

Dereference:
	${variable_name}
Daha önce tanımlanmamışı dereference edersen empty string alırsın.


set(NAME Mert)
set(NAME "Mert Gürşimşir")
	set(NAME Mert Gürşimşir) ------> NAME = Mert;Gürşimşir
		This creates list of 2 strings
	String is also a list with 1 item
set(AGE 25)

![[Pasted image 20250924141420.png]]



set(NAME Alice)
set(Alice Bob)
message(\${${NAME}})   ------------------------> prints Bob

---

list(~SUBCOMMAND~   ~NAME_OF_LIST~   ~ARGS~   ~RETURN_VALUE~)

Subcommands:
	APPEND
	INSERT
	FILTER
	GET
	JOIN
	REMOVE_ITEM
	REMOVE_AT
	REMOVE_DUPLICATES
	SORT
	REVERSE

Index:
![[Pasted image 20250924143658.png]]


list(APPEND VAR 1.6 XX)  ---> appends 1.6 and XX to list
list(REMOVE_AT 2 -3) ---> removes c and "Hello There"
list(REMOVE_ITEM VAR a 2.7)  ---> removes a and 2.7
list(INSERT VAR 2 XX 2.7)  ---> inserts XX and 2.7 such as XX is 2nd index
list(REVERSE VAR)  ---> reverses the VAR
list(REMOVE_DUPLICATES VAR)   ---> removes duplicates
list(SORT VAR)   ---> sorts the VAR

All operations are done at VAR itself.

![[Pasted image 20250924144926.png]]

![[Pasted image 20250924145102.png]]

FIND will return -1 if it doesnt find anything.


---

string()
	FIND
	REPLACE
	PREPEND
	APPEND
	TOLOWER
	TOUPPER
	COMPARE

set(VAR "MERT GÜRŞİMŞİR")
	string(FIND ${VAR} "GÜR" find_var)
	message(${find_var})   ----------->  5 (starting index)
-
	string(REPLACE "ŞİMŞİR" "ŞİMŞEK" replaced_var ${VAR})
	message(${replaced_var})
-
	string(PREPEND replaced_var "DENİZ")
	message(${replaced_var})  ----> DENİZMERT GÜRŞİMŞEK
-
	string(APPEND replaced_var "ŞİMŞİR")
	message(${replaced_var}) ---> DENİZMERT GÜRŞİMŞEKŞİMŞİR
-
	string(TOLOWER ${replaced_var} lower_case_var)
	message(${lower_case_var}) ---> denizmert gürşimşekşimşir
-
	string(TOUPPER ${replaced_var} upper_case_var)
	message(${upper_case_var}) ---> DENİZMERT GÜRŞİMŞEKŞİMŞİR
-
	string(COMPARE EQUAL ${upper_case_var} "DENİZMERT GÜRŞİMŞEKŞİMŞİR" is_equal)
	message(${is_equal})   ----> 1

