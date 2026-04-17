Display manager: manage user logins and graphical interfaces
initialization and startup is the 1st function where display managers launch during boot
starts graphical session like gnome, kde, xfce etc.
lets you switch users

XDM
- text-based login
GDM (GNOME Display Manager)
LIGHTDM

---
Context area is a memory area that Oracle creates when processing SQL statement. Holds data and metadata.
**Cursor**: holds multiple rows returned by an SQL statement. Pointer to context area.
DECLARE - OPEN - FETCH - CLOSE

DECLARE
	CURSOR c IS SELECT * FROM student;
	student_record student%ROWTYPE;   ---> gets all row values
BEGIN
	OPEN c;
	LOOP
		FETCH c INTO student_record;
		EXIT WHEN c%NOTFOUND; ---> fetch metadata of currently running SQL
		DBMS_OUTPUT.PUT_LINE(student_record.id || student_record.name);
	END LOOP;
	DBMS_OUTPUT.PUT_LINE(c%ROWCOUNT);
	CLOSE c;
END;

---
garbage collection in python: When the number of allocations minus the number of deallocations is greater than the threshold number, the garbage collector is run.
	import gc
	gc.get_threshold()

---
SINGLETON IN PYTHON:
![[Pasted image 20250507085115.png]]
![[Pasted image 20250507085033.png]]
![[Pasted image 20250507085134.png]]


---


Hooks are expected to validate, not mutate.


`prefetch_count=3` sets a **limit** of "3 unacknowledged messages max" **per consumer**.


If user has root privileges but cant run sudo:
	su -
	usermod -aG sudo mert

