# cmake\_minimum\_ required

```
	cmake_minimum_required(VERSION major.minor[.patch[.tweak]])
```

* The VERSION keyword must always be present and the version details provided must have at least the `major.minor` part.
* 

# project

```
	project
	(
	  projectName
	  [VERSION major[.minor[.patch[.tweak]]]]
	  [LANGUAGES languageName ...]
	)
```
* The `projectName` is required and may only contain letters, numbers, underscores (_) and hyphens (-).
* Version details are used by CMake to populate some variables and as default package metadata, but other than that, the version details don’t have any other significance. 
* The optional `LANGUAGES` argument defines the programming languages that should be enabled for the project. Supported values include `C`, `CXX`, `Fortran`, `ASM`, `Java` and others. If no `LANGUAGES` option is provided, CMake will default to `C` and `CXX`.


# add\_executable

```
	add_executable
	(
	  targetName [WIN32] [MACOSX_BUNDLE] [EXCLUDE_FROM_ALL]
	    source1 
	    ... 
	)
```
* Creates an executable which can be referred to within the CMake project as `targetName`. This name may contain letters, numbers, underscores (_) and hyphens (-). When the project is built, an executable will be created in the build directory with a platform-dependent name.
* Multiple executables can also be defined within the one CMakeLists.txt file by calling add_executable() multiple times with different target names.
* If the same target name is used in more than one add_executable() command, CMake will fail and highlight the error.
* **WIN32** When building the executable on a Windows platform, this option instructs CMake to build the executable as a Windows GUI application. In practice, this means it will be created with a WinMain() entry point instead of just main() and it will be linked with the /SUBSYSTEM:WINDOWS option. On all other platforms, the WIN32 option is ignored.
* **MACOSX\_BUNDLE** When present, this option directs CMake to build an app bundle when building on an Apple platform. Contrary to what the option name suggests, it applies not just to macOS, but also to other Apple platforms like iOS as well.
* **EXCLUDE\_FROM\_ALL** Sometimes, a project defines a number of targets, but by default only some of them should be built. When no target is specified at build time, the default ALL target is built (depending on the CMake generator being used, the name may be slightly different, such as ALL\_BUILD for Xcode). If an executable is defined with the EXCLUDE_FROM_ALL option, it will not be included in that default ALL target. A
common situation where it can be useful to exclude a target from ALL is where the executable is a developer tool that is only needed occasionally.


# add\_library

```
	add_library
	(
	  targetName [STATIC | SHARED | MODULE]
	    [EXCLUDE_FROM_ALL]
	    source1
	    ...
	)
```
* This form is analogous to how add_executable() is used to define a simple executable. The `targetName` is used within the CMakeLists.txt file to refer to the library, with the name of the built library on the file system being derived from this name by default.
* **STATIC** Specifies a static library or archive. On Windows, the default library name would be `targetName.lib`, while on Unix-like platforms, it would typically be `libtargetName.a`.
* **SHARED** Specifies a shared or dynamically linked library. On Windows, the default library name would be `targetName.dll`, on Apple platforms it would be `libtargetName.dylib` and on other Unix-like platforms it would typically be `libtargetName.so`. On Apple platforms, shared libraries can also be marked as frameworks.
* **MODULE** Specifies a library that is somewhat like a shared library, but is intended to be loaded dynamically at run-time rather than being linked directly to a library or executable. These are typically plugins or optional components the user may choose to be loaded or not. On Windows platforms, no import library is created for the DLL.
* It is possible to omit the keyword defining what type of library to build. Unless the project specifically requires a particular type of library, the preferred practice is to not specify it and leave the choice up to the developer when building the project. In such cases, the library will be either STATIC or SHARED, with the choice determined by the value of a CMake variable called `BUILD_SHARED_LIBS`. If `BUILD_SHARED_LIBS`has been set to true, the library target will be a shared
library, otherwise it will be static.


# target\_link\_libraries

```
	target_link_libraries
	(
	  targetName
	     <PRIVATE|PUBLIC|INTERFACE> 
	       item1 
	       ...
	     <PRIVATE|PUBLIC|INTERFACE> 
	       item1 
	      ...
	)
```
* When considering the targets that make up a project, developers are typically used to thinking in terms of library A needing library B, so A is linked to B. This is the traditional way of looking at library handling, where the idea of one library needing another is very simplistic.
* **PRIVATE** Private dependencies specify that library A uses library B in its own internal implementation. Anything else that links to library A doesn’t need to know about B because it is an internal implementation detail of A.
* **PUBLIC** Public dependencies specify that not only does library A use library B internally, it also uses B in its interface. This means that A cannot be used without B, so anything that uses A will also have a direct dependency on B. An example of this would be a function defined in library A which has at least one parameter of a type defined and implemented in library B, so code cannot call the function from A without providing a parameter whose type comes from B.
* **INTERFACE** Interface dependencies specify that in order to use library A, parts of library B must also be used. This differs from a public dependency in that library A doesn’t require B internally, it only uses B in its interface. An example of where this is useful is when working with library targets defined using the `INTERFACE` form of `add_library()`, such as when using a target to represent a header-only library’s dependencies.
* In addition to CMake targets, the following things can also be specified as items in a `target_link_libraries()` command:
	* **Full path to a library file** CMake will add the library file to the linker command. If the library file changes, CMake will detect that change and re-link the target. Note that from CMake version 3.3, the linker command always uses the full path specified.
	* **Plain library name** If just the name of the library is given with no path, the linker command will search for that library (e.g. foo becomes -lfoo or foo.lib, depending on the platform). This would be common for libraries provided by the system.
	* **Link flag** As a special case, items starting with a hyphen other than -l or -framework will be treated as flags to be added to the linker command. The CMake documentation warns that these should only be used for `PRIVATE` items, since they would be carried through to other targets if defined as `PUBLIC` or `INTERFACE` and this may not always be safe.
* The `debug`, `optimized` and `general` keywords should be avoided for new projects.

# set

```
	set(varName value... [PARENT_SCOPE])
	set(varName value... CACHE <BOOL|FILEPATH|PATH|STRING|INTERNAL> "docstring" [FORCE])
```

* The most basic way of defining a variable is with the set() command
* The name of the variable, varName, can contain letters, numbers and underscores (_), with letters being **case-sensitive**.

# set\_property

```
	set(trafficLight Green CACHE STRING "Status of something")
	set_property(CACHE trafficLight PROPERTY STRINGS Red Orange Green)
```

* `trafficLight`  cache  variable  will  initially  have  the  value `Green`. When  the  user
attempts to modify trafficLight in cmake-gui, they will be given a combobox containing the three values `Red`, `Orange`, `Green`

# unset

```
	unset(varName)
```

* A variable can be unset either by calling unset() or by calling set() with no value for the named variable. 

# option

```
	option(varName docstring [initialValue])
```
* Equal to `set(varName initialValue CACHE BOOL "docstring")`


# mark\_as\_advanced

```
	mark_as_advanced([CLEAR|FORCE] var1 [var2...])
```
* The  `CLEAR`  keyword  ensures  the  variables  are  not  marked  as  advanced,  while  the  `FORCE`  keyword ensures  the  variables  are  marked  advanced.  Without  either  keyword,  the  variables  will  only  be marked as advanced if they don’t already have an advanced/non-advanced state set.

# message

```
	message([STATUS|WARNING|SEND_ERROR|FATAL_ERROR|DEPRECATION] msg1 [msg2]...)
```

* Print out diagnostic messages and variable values during a CMake run.
* **STATUS** Incidental information. Messages will normally be preceded by two hyphens.
* **WARNING** usually  shown  highlighted  in  red  where  supported  (cmake  command  line console or the cmake-gui log area). Processing will continue.
* **AUTHOR_WARNING** Like WARNING, but only shown if developer warnings are enabled (requires the -Wdev option on the cmake command line).
* **SEND_ERROR** Indicates an error message which will be shown highlighted in red, where supported. Processing will continue until the configure stage completes, but generation will not be performed.
* **FATAL_ERROR** Denotes a hard error. The message will be printed and processing will stop immediately. The log will also normally record the location of the fatal message() command.
* **DEPRECATION** Special  category  used  to  log  a  deprecation  message.  If  the  `CMAKE_ERROR_DEPRECATED`  variable  is defined to a boolean true value, the message will be treated as an error. If `CMAKE_WARN_DEPRECATED` is  defined  to  a  boolean  true,  the  message  will  be  treated  as  a  warning.  If  neither  variable  is defined, the message will not be shown.

# variable\_watch

```
	variable_watch(myVar [command])
```

* All attempts to read or modify it are logged.
 
# string

```
	string(FIND inputString subString outVar [REVERSE])
```

* If `subString` does not appear in `inputString`, then `outVar` will be given the value -1.

```
	string(REPLACE matchString replaceWith outVar input [input...])
```

* Replacing a simple substring

```
	string(REGEX MATCH    regex outVar input [input...])
	string(REGEX MATCHALL regex outVar input [input...])
	string(REGEX REPLACE  regex replaceWith outVar input [input...])
```

* Regex full support

```
	string(SUBSTRING input index length outVar)
```

* Extracting a substring
* If `length` is -1, the returned substring will contain all characters up to the end of the input string. 

```
	string(LENGTH  input outVar)
	string(TOLOWER input outVar)
	string(TOUPPER input outVar)
	string(STRIP   input outVar)
```

# list

```
	list(LENGTH listVar outVar)
	list(GET    listVar index [index...] outList)
	list(APPEND listVar item [item...])
	list(INSERT listVar index item [item...])
	list(FIND myList value outIndex)
	list(REMOVE_ITEM myList value [value...])
	list(REMOVE_AT myList index [index...])
	list(REMOVE_DUPLICATES myList)
	list(REVERSE myList)
	list(SORT myList)
```

* For all list operations which take an `index` as input, the `index` can be negative to indicate counting starts from the end of the list instead of the start. When used this way, the last item in the list has `index` -1, the second last has `index` -2 and so on.

# math

```
	math(EXPR outVar mathExpr)
```

# if

```
	if(expression1)
	    # commands ...
	elseif(expression2)
	    # commands ...
	else()
	    # commands ...
	endif()
```

* If value is an unquoted constant with value 1, ON, YES, TRUE, Y or a non-zero number, it is treated as true. The test is case-insensitive.
* If value is an unquoted constant with value 0, OFF, NO, FALSE, N, IGNORE, NOTFOUND, an empty string or a string that ends in -NOTFOUND, it is treated as false. Again, the test is case-insensitive.

##### Logic operator
```
	# Logical operators
	if(NOT expression)
	if(expression1 AND expression2)
	if(expression1 OR expression2)

	# Example with parentheses
	if(NOT (expression1 AND (expression2 OR expression3)))
```

| Numeric     | String         | Version numbers      |
| ------------|----------------|----------------------|
|LESS         |STRLESS         |VERSION_LESS          |
|GREATER      |STRGREATER      |VERSION_GREATER       |
|EQUAL        |STREQUAL        |VERSION_EQUAL         |
|LESS_EQUAL   |STRLESS_EQUAL   |VERSION_LESS_EQUAL    |
|GREATER_EQUAL|STRGREATER_EQUAL|VERSION_GREATER_EQUAL |

```
	# Valid numeric expressions, all evaluating as true
	if(2 GREATER 1)
	if("23" EQUAL 23)
	set(val 42)
	if(${val} EQUAL 42)
	if("${val}" EQUAL 42)
	
	if(1.2   VERSION_EQUAL   1.2.0)
	if(1.2   VERSION_LESS    1.2.3)
	if(1.2.3 VERSION_GREATER 1.2  )
	if(2.0.1 VERSION_GREATER 1.9.7)
	if(1.8.2 VERSION_LESS    2    )	
```

##### Regex

```
	if(value MATCHES regex)
```

##### File System

```
	if(EXISTS pathToFileOrDir)
	if(IS_DIRECTORY pathToDir)
	if(IS_SYMLINK fileName)
	if(IS_ABSOLUTE path)
	if(file1 IS_NEWER_THAN file2)
```

* `IS_NEWER_THAN` returns true if either file is missing or if both files have the same timestamp. Thus, it would not  be  unusual  to  test  for  the  existence  of  `file1`  and  `file2`  before  performing  the  actual `IS_NEWER_THAN` test,  since  the  result  of  `IS_NEWER_THAN` 
* Full paths should also be given when using `IS_NEWER_THAN`, since the behaviour for relative paths is not well defined.
* None of the file system operators perform any variable/string substitution without `${}`, regardless of any quoting.