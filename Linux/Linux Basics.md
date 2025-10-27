- `echo` = a shell built-in command that **prints text or variables** to the screen
- `$?` = a special shell variable that holds the exit status of the last command executed

- `more <SOURCE_CODE_FILE>` = display the code to the terminal
- `<CC> -c <source_file>.c` = create object file

## What are differences between `apt-get` and `apt` #LinuxBasics
- `apt-get` 
	- older tool
	- very stable and low-level
- `apt`
	- more user-friendly that combines features from `apt-get`, `apt-cache` and more
	- designed for interactive use in the terminal
	- for everyday use, `apt` is simpler, cleaner and recommended


## What does `sudo apt update` do? #LinuxBasics 
- Download the latest list of available packages and their versions from the repositories configured on your system
- It makes sure your system knows about the newest versions
- ensures that when running `sudo apt install` or `sudo apt upgrade`, the system installs the most up-to-date versions

## What does the phrase "mounted on" mean?
- Linux file system is like a giant tree with:
	- root: `/`
	- branches: other folders (`/home`, `/boot` , etc.)
- When we mount a partition to file system, meaning we are connecting that storage device to a specific branch of the tree - so that you can access its files