# Best pactices

## .1
Ensure  every  CMake  project  has  a  cmake_minimum_required()  command  as  the  first  line  of  its  top level CMakeLists.txt file. When deciding the minimum required version number to specify, keep in mind that the later the version, the more CMake features the project will be able to use. It will also mean the project is likely to be better placed to adapt to new platform or operating system releases, which  inevitably  introduce  new  things  for  build  systems  to  deal  with.  Conversely,  if  creating  a project  intended  to  be  built  and  distributed  as  part  of  the  operating  system  itself  (common  for Linux), the minimum CMake version is likely to be dictated by the version of CMake provided by that same distribution.


## .2
Target names need not be related to the project name. It is common to see tutorials and examples use a variable for the project name and reuse that variable for the name of an executable target like so:

```
		set(projectName MyExample)
		project(${projectName})
		add_executable(${projectName} ...)
```

This only works for the most basic of projects and encourages a number of bad habits. Consider the project name and executable name as being separate, even if initially they start out the same. Set the project name directly rather than via a variable, choose a target name according to what the target does rather than the project it is part of and assume the project will eventually need to define more than one target. This reinforces better habits which will be important when working on more complex multi-target projects.

## .3
When naming targets for libraries, resist the temptation to start or end the name with lib. On many platforms (i.e. just about all except Windows), a leading lib will be prefixed automatically when constructing the actual library name to make it conform to the platform’s usual convention. If the target  name  already  begins  with  lib,  the  resultant  library  file  names  end  up  with  the  form `liblibsomething`, which people often assume to be a mistake.


## .4
Unless there are strong reasons to do so, try to avoid specifying the `STATIC` or `SHARED` keyword for a library until it is known to be needed. This allows greater flexibility in choosing between static or dynamic libraries as an overall project-wide strategy. The `BUILD_SHARED_LIBS` variable can be used to change the default in one place instead of having to modify every call to ``add_library()`.