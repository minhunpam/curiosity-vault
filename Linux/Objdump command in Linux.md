- Dealing with object files without access to the source code, extracting maximum information from these files is crucial, especially for:
	- Debugging
	- Reverse Engineering
	- System analysis
- `objdump` - a utility that allows you to display information about object files

## Common uses of `objdump` command
### To disassemble the file
```bash
objdump -d <executable file>
```