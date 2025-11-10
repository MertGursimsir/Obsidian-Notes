Çok dosya, çok library varsa bütün dosyaları aynı klasörde tutmak mantıksız.

build klasörünün ismi farklı olabilir.
Hatta release debug diye ayırıp 2 farklı compiler optimization levels kullanabilirsin.

CMakeLists.txt dosyası sadece kendi klasöründeki dosyaları görür.
yani addition.cpp'yi temp klasörü altına alırsan artık temp/addition.cpp ile dosyaya erişirsin.

![[Pasted image 20250924092709.png]]

.cpp dosyaları ilgili dizinlere taşındı.

Artık cmake dosyasında (CMakeLists.txt) build tree'ye bu klasörleri eklemeliyiz:

	add_subdirectory(my_math_dir)
		subdirdeki txt çalışır ve my_math oluşur
	add_subdirectory(my_print_dir)
		subdirdeki txt çalışır ve my_pring oluşur

Artık add_library'leri kaldırabiliriz.
Bu komutlarla CMake'e bu folder'lara gir, oradaki CMakeLists.txt'yi bul ve tek tek çalıştır deriz.

![[Pasted image 20250924093111.png]]
![[Pasted image 20250924093600.png]]

![[Pasted image 20250924094459.png]]



HEADARLARI NAPCAZZ!
Onları da subdirlere alalım.
Hatta bu subdirleri include ve src diye ayıralım.

Ayrıcaaa bu headerları ilgili cpp dosyalarında da include etmek doğru olur.
	bunu yaparak fonksiyonun yanlış deklare edilmesini önleriz.
	gelecekte compatability issue olma ihtimali azalır.

Pekiiii headerları include ettik ama farklı klasördeler.
Nasıl görülecek?
	include dediğinde: preprocesser o header dosyasını bulur ve dosyanın contenti ile o satırı değiştirir.
	HEADER ve SOURCE aynı dizindeyse sorun yokk.
	Değilse explicit olarak preprocessor'a headerı nerede bulacağını söylememiz gerekir.

Nasıl söyleriz?
- header include edilirken relative path verilerek yapılabilir.
- cmake'e bu işi halletmesini söyleyebiliriz.
	- target_include_directories


target_include_directories( ~TARGET~   ~SCOPE~  ~DIR1~  ~DIR2~)
DIRS -----> folders that contain headers

target_include_directories to my_math CMakeList:
	target_include_directories(my_math PUBLIC include)
![[Pasted image 20250924104655.png]]

Aaayrıca unutmadan diyim: addition.cpp değil, src/addition.cpp demek lazım cmake dosyasında.

Aynısını print için de yaparız.

DİKKKAAATTT!!!!
root level cmake dosyasında target_include_directories demedik.
Demememize rağmen main.cpp içinde relative path falan vermeden include edebiliyoruz!?
ÇÜÜNKÜÜ **PUBLIC** keywordünü kullanarak target_include_directories yaptık.


TARGETS HAVE: 
	PROPERTIES
		target_include_directories(my_math PUBLIC include)
		target_include_directories(my_math INTERFACE include)	
		*PROPERTY*: INTERFACE_INCLUDE_DIRECTORIES
			for target: my_math
			set to: /home/gursimsir/Desktop/CMAKE/module3/my_math_dir/include
		target_include_directories(my_math PRIVATE include)	
		*PROPERTY*: INTERFACE_INCLUDE_DIRECTORIES
			for target: my_math
			set to: ~not-set~	
				yani main.cpp wont be able to find the include directory
	DEPENDENCIES


Root level'da naptık:
![[Pasted image 20250924110336.png]]
Buradaki cmake her iki dependency'nin de propertylerini okur.
![[Pasted image 20250924110514.png]]
Okunduktan sonra da bu librarylerin include directoryleri calculator target'ına görünür olur.


PUBLIC - PRIVATE - INTERFACE NASIL KARAR VERECEĞİZ????????

target bu include directory'ye ihtiyaç duyuyor mu?
	EVET
	Örneğin my_math include directory'sine ihtiyaç duyuyor çünkü headerı include ediyoruz.

target'a bağımlı olan diğer üst targetlar bu include directory'ye ihtiyaç duyuyor mu?
	EVET
	calculator my_math'e bağımlı ve onun include directory'sine ihtiyacı var çünkü o da include ediyor.

Her iki cevap da EVET ise:
	PUBLIC
İlki hayır ikincisi EVET ise:
	INTERFACE
İlki yes ikincisi NO ise:
	PRIVATE


Aşağıdaki komutlar da scope gerektirir:
![[Pasted image 20250924112243.png]]
target...(TARGET_NAME  |   SCOPE  |   ARGUMENTS)

Şu şekilde birleştirebiliriz:
	target_include_directories(TARGET   PRIVATE   XXX   PUBLIC   YYY)

include içine ek bir klasör açıp (my_math isminde), include ederken "my_math/division.h" gibi include ederek headerların nereden geldiğini daha anlaşılır yapabiliriz.


include ve top level library folder name aynı olabilir. yani my_math_dir yerine my_math.

