# pb-cpp-program-template
Template project for creating c++ programs

1. Modify CMakeLists.txt change all occurances of pb-cpp-program-template to your project name.
2. Rename pb-cpp-program-template.code-workspace to <project-name>.code-workspace
3. Create build folder.
4. cd to build folder
5. For Windows
        cmake -G "Visual Studio 17 2022" -A x64 ..
   For Linux
        cmake -G "Unix Makefiles" ..
6. cmake --build .
