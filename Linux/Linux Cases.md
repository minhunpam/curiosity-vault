## Linux System runs out of disk space #LinuxCases
- When **Linux system runs out of disk space**, you can check the disk space with `df -h` 
	- `df` = disk free --> shows information about disk space usage of file systems
	- `-h` = human-readable
-  We can `du -h ~ | sort -hr | head -20`
	- `du -h ~`
		- `du` = disk usage - estimates file and directory space usage
		- `~` = home directory
	* `sort -hr`
		* `sort` = sorts lines of text
		* `-r` = reverse order - biggest items at the top
	* `head -20`
		* `head` = shows the first lines of outputs
		* `-20` = shows the first 20 lines
-  After that, depends on the case, a lot of disk space could be used by cache and config files, you can free them up as following:
	- `rm -rf ~/.cache/vscode-cpptools` (an example)
		-  `rm` = remove command
		- `-r` = tells `rm` to delete folders and everything inside them (including subfolders and files)
		- ⚠️ `f` = "force" -- tells `rm` not to ask for confirmation (it will delete without warnings, even if files are write-protected) ⚠️

## `doxygen -g` #LinuxCases 
- `-g` stands for "generate"
- generate a default `Doxygen` configuration file in your current directory that contains all the settings you can customize for your project's documentation
- **⚠️ But flags like `-g` can mean different things in other programs**



