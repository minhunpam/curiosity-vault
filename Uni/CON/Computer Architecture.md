## x86_64 (x64 or AMD64)
- a 64-bit version of the x86 instruction set architecture
- Why 64-bit
	- has 64-bit wide registers
	- allows larger data types, more registers -> improve performance
### Checking your system architecture:
```bash
uname -m
```

### Virtual Addresses
- virtual addresses on x86_64 processors only have 48 meaningful bits -> not all 64-bit patterns correspond to meaningful virtual addresses
- Bit patterns that are valid addresses are called **canonical addresses**
	- The architecture divides canonical addresses into 2 groups, low and high
	

|                     | Lower Half (User Space)                            | Higher Half (Kernel Space)                         |
| ------------------- | -------------------------------------------------- | -------------------------------------------------- |
| Range               | `0x0000'0000'0000'0000` to `0x0000'7FFF'FFFF'FFFF` | `0xFFFF'8000'0000'0000` to `0xFFFF'FFFF'FFFF'FFFF` |
| Sign-extension bits | 47 = 48-63 = 0                                     | 47 = 48-63 = 1                                     |
⚠️ If bits from 48 - 63 are not equal to bit 47, then the address is **non-canonical**
