apt update
	updates list of available packages and their versions
	not install or upgrade any packages
	---
	downloads latest package metadata from repositories listed in sources.list and sources.list.d/*
	---
	stores in /var/lib/apt/lists
	---
	you can see available packages using apt list or looking into files directly

apt upgrade
	using the downloaded metadata, identifies installed packages that have newer versions available.
	---
	downloads and install updated versions of packages you have already installed only if:
		- upgrade doesn't require removing any packages
		- upgrade doesn't require installing new dependencies
	only touches existing packages
	---
	apt full-upgrade -> upgrade even if it requires installing/removing other packages