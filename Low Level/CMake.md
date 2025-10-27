## Reasons for the existence of CMake #CMake
- Manual Makefiles --> unscalable, unportable and error-prone
- Strong need for a portable, flexible, and IDE-friendly build system
- CMake offered a way to write once, build anywhere - reliably and cleanly

-  CMake uses a configuration file called `CMakeLists.txt`
	1. Define your project in CMakeLists.txt
	2. Run CMake to create the Makefile

## What does  `find_package()` do? #CMakeCases
- `find_package` = Cmake command -> locate and configure an external library or package
	- sets up variables and import targets so you can use the library in your project
- Example: `find_package(SFML 2.5 COMPONENTS graphics window system REQUIRED)`
	- `SFML` - name of the package
	- `2.5` - minimum required version of the package
	- `COMPONENTS graphics window system` - tells CMake which modules of SFML your project needs
	- `REQUIRED` - tells CMake that the command must succeed. If the package with required components are not found, CMake will stop with an error
### Normally after `find_package()` there's always a follow of `target_link_libraries()`. What does it do?
- `target_link_libraries()` - tells CMake which libraries your target (program or library) should be linked with during compilation
- Example: `target_link_libraries(side_project_calculator PRIVATE sfml-graphics sfml-window sfml-system)`
	- `side_project_calculator` - name of your target (your executable or library), which you defined earlier in `add_executable()`
	- `PRIVATE` - an **access scope** = "only this target needs to know about these libraries"
		> There are other scopes:
		> `PUBLIC` - target and anything that links to it need these libraries
		> `INTERFACE` - only things linking to this target need the libraries
	- `sfml-graphics sfml-window sfml-system` - actual library targets you are linking against

⚠️***This is useful when want to run debugger on CLion***
