python -m project.main
	bana modül ismi ver, sys.path'ten bul ve içindeki __main__ kısmını çalıştır.
	sys.path[0] project klasörünün üst dizinidir.
	project/__init__.py varsa bu klasör package olur (boş olsa da olmalı)
python main.py
	sys.path[0] main.py'ın bulunduğu yerdir.
	yani klasör 1. sıradadır

python sırayla şuralara bakar:
- çalıştığın dizin
- sys.path'teki dizinler
	- site-packages gibi yerler...
	- environment variable olarak tanımlanan PYTHONPATH
- built-in modüller


pypy --> jit var (bytecode'u sık kullanılan kodları makine koduna çevirir)
cpython --> jit yok (py - pyc (bytecode) - pvm (python virtual machine) interpretes the pyc)