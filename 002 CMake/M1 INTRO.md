float addition(float, float);
float division(float, float);
void print_result(std::string, float);

main(){...}

ANA .cpp dosyasında fonksiyon tanımları (declarations) var.
	Ya da tanımları headerlara alıp ana cpp dosyamızda bu headerları include edebiliriz.
Fonksiyonlar ayrı dosyalarda implement edilmiş.

g++ main.cpp addition.cpp division.cpp print_result.cpp -o calculator
	4 farklı compiled binary dosya oluşur.

main.cpp compile edilirken compiler'a: "kullandığım 3 fonksiyon bir yerde var" demek yeterli. Bunu da headerları include ederek yapıyoruz. 

Compiler bu aşamada function definitionları umursamaz. Fonksiyonların çağrıldığı yere placeholder koyar. 

Bu placeholderlar fonksiyon çağrısının linking aşamasında resolve edileceğini belirtir.

Linker finds the compiled binaries (.o files) of cpp files. 
Then links them all to produce single executable.

If you have .h files and .o files (of addition, division, and print_result), you can build the project.

Böyle yaparsak çoklu dosya içeren projelerde ufak bir şey değiştirsek bile her şeyi baştan compile ve link ederiz.

![[Pasted image 20250923141618.png]]

AMA BÖYLE YAPARSAK sadece değişen dosyaları compile ederiz.
Sonra da linkleriz.


ÖRNEK MAKEFILE:
----------------------------------------------
calculator: main.o addition.o division.o print_result.o
	g++ main.o addition.o division.o print_result.o -o calculator

main.o: main.cpp
	g++ -c main.cpp

addition.o: addition.cpp
	g++ -c addition.cpp

division.o: division.cpp
	g++ -c division.cpp

print_result.o: print_result.cpp
	g++ -c print_result.cpp


Bu makefile içindeki komutlar ile tüm değişen dosyalar compile edilir ve linklenir.



Make --> executable oluşturmak için build system files (makefile) kullanır.
CMAKE --> Bizim için build system files generate eder.
	creates platform based build system files

