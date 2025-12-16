- Dosyadaki linki indirmek:
	- wget $(cat file.txt)

- jar mert/folder parametre:
	- java -jar test.jar mert/folder

- rsync -a --delete --remove-source-files /tmp/test/ /opt/test/
	- -a --> preserves permissions
	- --delete --> deletes files in `/opt/test` that don't exist in `/tmp/test`
	- --remove-source-files --> remove /tmp/test

- run one after another:
	- command1 ; command2

- art arda çalışsın (önceki hatalıysa sonraki çalışmaz):
	- command1 && command2

- ln -s {original file} {symlink name}

- Yanlış yüklenen paketlerin listesi:
	- dpkg -l | grep -v '^ii'
		- ii --> installed properly

- Yanlış yüklenen paketleri düzeltmek:
	- sudo apt --fix-broken install

- Paketleri tekrar yüklemek:
	- sudo apt install --reinstall gdm3

- Display manager set etmek:
	- sudo dpkg-reconfigure gdm3
	- sudo systemctl restart gdm3

- if testfile.txt doesnt exist, do sth:
	- if ! test -f 'testfile.txt'
		then 
		    # some commands here
		fi

- cut -b 1 message.txt  --> shows 1 byte
- cut -c 1 message.txt --> shows 1 character
- cut -b 7,8,9 message.txt    OR    cut -b 7-9 message.txt  --> shows from 7. byte to 9. byte
- cut -d " " -f 1,2 message.txt  --> seperate with space and shows first two (for each line seperately)

- Metni decode etmek:
	- echo QWxhZGRpbjpvcGVuIHNlc2FtZQ== | base64 --decode > test.jpg

- `find . -type f -name "*download*"`
	- download kelimesi geçen dosyaları getirir

- id --> kullanıcı ve gruplarını listeler

- sudo groupadd testgroup
- sudo usermod -aG testgroup testuser
	- testgroup grubunu oluşturur, testuser'ı bu gruba ekler
	- AFTER APPEND, RUN "newgrp testgroup" TO APPLY CHANGES
- sudo gpasswd -d testuser testgroup
	- testuser'ı testgroup'tan çıkarır

- lsb_release -a
	- ubuntu versiyonu getirir

- iperf3 -c 10.11.12.13 -p 5000
	- bağlandığın ip'deki bilgisayarla iletişimde bitrate'e bakarsın

- pkill -HUP -f “cinnamon --replace”
	- This command sends a HUP (HangUp) signal to the cinnamon process, which will restart Cinnamon without terminating your X session.

- while read input_val; do echo $input_val done
	  run line_number times
	  if input has 3 line, runs 3 times
	  if input is like  --> a b c --> then runs once and prints a b c

- copy con test.txt
	- creates test.txt and you can edit until you do ctrl+z and enter in WINDOWS

- type test.txt
	- shows content of test.txt for WINDOWS

- set "DATE=%~1"
	- set variable date in bat script in WINDOWS
	- you can use DATE with %DATE%

- ip -br a
- ethtool eno1
	- Link detected: yes  --> kablo takılı demek

- find . -name Makefile | xargs -I {} rm -f {}
	- find under current dir files named Makefile and remove

- lib2: Depends: lib1 (= 2.2) but 2.3 is to be installed
	- sudo apt install lib1=2.2 lib2

- git config --global http.sslVerify false

- comm -23 \
  <(cd source_dir && find . -type f | sort) \
  <(cd target_dir && find . -type f | sort)
	- Finds all packages and shows files in source_dir that doesnt exist at target_dir

 - If you added file to .gitignore after it is tracked by git, you should do:
	 - git rm --cached -r --ignore-unmatch '* * /file_name' 
		 - * 'lar bitişik
	 - Then:
		 - git commit -m "Remove cache.json from tracking"

- xdg-open PATH
	- Path'in directorysini açar.

- jobs
	- Then you can "kill %1" or "kill %2" the processes that you see as output of jobs

- os versiyon:
	cat /etc/os-release

- Dosyayı parçalara bölmek
	split -b 2G bigfile.tar bigfile.tar.part_
- Parçaları birleştirmek
	cat bigfile.tar.part_* > bigfile.tar

