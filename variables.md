# Variables

* The most basic way to defining a variable is with the set() command.
* The name of the variable, varName, can contain letters, numbers and underscores (_), with letters being **case-sensitive**.
* In CMake, a variable has a particular scope, much like how variables in other languages have scope limited to a particular function, file, etc. A variable cannot be read or modified outside of its scope.
* CMake treats all variables as strings. 
* When setting a variable’s value, CMake doesn’t require those values to be quoted unless the value contains spaces.
*  If multiple values are given, the values will be joined together with a semicolon separating each value: the resultant string is how CMake represents lists. The following should help to demonstrate the behavior.

```
	set(myVar a b c)   # myVar = "a;b;c"
	set(myVar a;b;c)   # myVar = "a;b;c"
	set(myVar "a b c") # myVar = "a b c"
	set(myVar a b;c)   # myVar = "a;b;c"
	set(myVar a "b c") # myVar = "a;b c"
```

+ The value of a variable is obtained with ${myVar}, which can be used anywhere as string or as pointer (by name) to a variable.
* CMake doesn’t require variables to be defined before using them. Default value is empty string.
* Strings can contain embedded newline characters or quotes, which require escaping with backslashes.

```
	set(foo ab)               # foo  = "ab"
	set(bar ${foo}cd)         # bar  = "abcd"
	set(baz ${foo} cd)        # baz  = "ab;cd"
	set(myVar ba)             # myVar = "ba"
	set(big "${${myVar}r}ef") # big  = "${bar}ef" = "abcdef"
	set(${foo} xyz)           # ab  = "xyz"
	set(bar ${notSetVar})     # bar  = ""
	
	set(myVar "goes here")
	set(multiLine "First line ${myVar}
	Second line with a \"quoted\" word")
```

* Alternative to quotes is to use the lua-inspired bracket syntax where the start of the content is marked by **\[=\[** and the end with **\]=\]**. Any number of **=** characters can appear between the square brackets, including none at all, but the same number of **=** characters must be used at the start and the end.
* If the opening brackets are immediately followed by a
newline character, that first newline is ignored.

```
	# Simple multi-line content with bracket syntax,
	# no = needed between the square bracket markers
	set(multiLine [[
	First line
	Second line
	]])

	# Bracket syntax prevents unwanted substitution
	set(shellScript [=[
	#!/bin/bash
	[[ -n "${USER}" ]] && echo "Have USER"
	]=])

	# Equivalent code without bracket syntax
	set(shellScript
	"#!/bin/bash
	[[ -n \"\${USER}\" ]] && echo \"Have USER\"
	")
```

# Environment Variables

```
	$ENV{varName} 
```

* Setting an environment variable can be done in the same way as an ordinary variable, except with **ENV{varName}** instead of just varName as the variable to set.
* As soon as the CMake run is finished, the change to the environment variable is lost.

# Cache Variables

```
	set(varName value... CACHE <BOOL|FILEPATH|PATH|INTERNAL> "docstring" [FORCE])
```

* **BOOL** The cache variable is a boolean on/off value. GUI tools use a checkbox or similar to represent the variable. (ON/OFF, TRUE/FALSE, 1/0).
* **FILEPATH** The cache variable represents a path to a file on disk. GUI tools present a file dialog to the user for modifying the variable’s value.
* **PATH** Like FILEPATH, but GUI tools present a dialog that selects a directory rather than a file.
* **STRING** The variable is treated as an arbitrary string. By default, GUI tools use a single-line text edit.
* **INTERNAL** The variable is not intended to be made available to the user. Internal cache variables are sometimes used to persistently record internal information by the project, such as caching the result of an intensive query or computation. GUI tools do not show INTERNAL variables.
* **docstring** is a description string used from GUI tools to describe variable
* set() command will only overwrite a cache variable if the FORCE keyword is present, unlike normal variables where the set() command will always overwrite a pre-existing value. The main reason for this is that cache variables are primarily intended as a customization point for developers.
* A point that is often not well understood is that normal and cache variables are two separate things. It is possible to have a normal variable and a cache variable with the same name but holding different values. 



