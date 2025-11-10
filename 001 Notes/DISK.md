df -h -> disk free (human readable), shows free disk space
	can ignore tmpfs (temp fs)
	shows all storage devices

lsblk -> list block devices, shows info about all available block devices (such as SSD, HDD, USB...) and their partitions.
lsblk -f /dev/sdb3

/dev/sdb -> whole storage device
/dev/sdb3 -> 3rd partition on that device
That partition might use the **ext4** filesystem to organize files
	how files are named, how folders are created
	how to find things fast, how space is tracked

Disk -> whole bookshelf
Partition -> one shelf or section of it

df -h -T  OR df -hT -> gives filesystem type (like **ext4**)

df -hTx tmpfs -> exludes tmpfs filesystem

watch df -hTx tmpfs -> repeat the command over and over, you can see changes

---

du -> give path and show you how much space is used in that directory

du -h /home/mert -> human readable

du -h --max-depth 1 /home/mert -> limit how deep within directory tree

du -hs /home/mert -> summary, gives total disk usage in given path

You can give MULTIPLE directories:
	sudo du -hs /home/mert /etc

sudo du -hsc /home/mert /etc -> gives total of given directories

du -hsc /home/mert/* -> give summary of every subdirectory

---

ncdu -> shows disk usages and sorts for user