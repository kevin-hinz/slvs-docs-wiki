This page contains an overview of how to get started with analyzing CMake projects in VS2017.3 and later versions, along with information about known issues. This feature is available starting with SLVS v4.38.

If you need more help, please open a ticket in the [SonarLint - Get Help](https://community.sonarsource.com/c/help/sl/11) section of the community forum.


## Getting started - VS2019 and later

1. Open / create a new CMake project in Visual Studio
2. Add the command `set(CMAKE_EXPORT_COMPILE_COMMANDS ON)` to the top-level `CMakeLists.txt` file:

[[images/CMake/SLVS_CMake_VS2019_CMakeLists_SetCommand.png|alt=Adding Set command to CMakeLists.txt]]

3. Save the file.
4. Check that VS has generated the compilation database.

Source and header files will now be analyzed on opening/saving. The SonarLint _Output Window_ will contain more information about the progress of the analysis.

_Note: if you are creating a new CMake project in VS2019 there appears to be an intermittent issue with VS not recognizing the project as a CMake project until the folder is closed and re-opened. See ticket [#2656](https://github.com/SonarSource/sonarlint-visualstudio/issues/2656) for more information._


## Getting started - VS2017.3
Follow the same getting started steps as for VS2019 and later above. In addition, to successfully analyze C/C++files in VS2017 your project will need to use a non-default _CMakeSettings.json_ file. This is because the default _CMakeSettings.json_ file uses the `workspaceHash` or `projectHash` macros, which are not supported in by SonarLint in VS2017.

1. If you do not already have a _CMakeSettings.json_ file on disc, Visual Studio will create one if you click on `Manage configurations option` and select a compilation target:

[[images/CMake/SLVS_CMake_VS2017_ManageConfig.png|alt=Selecting manage config dropdown in VS2017]]

Visual Studio will then create a default _CMakeSettings.json_ file:

[[images/CMake/SLVS_CMake_VS2017_DefaultCMakeSettingsFile.png|alt=Default CMakeSettings.json file]]

2. Edit the _CMakeSettings.json_ file to change the `buildRoot` property to one that does not use either `workspaceHash` or `projectHash`.

Note: the defaults used by Visual Studio for `buildRoot` and `installRoot` in VS2019 have changed to:
```
      "buildRoot": "${projectDir}\\out\\build\\${name}",
      "installRoot": "${projectDir}\\out\\install\\${name}",
```
Using those values in VS2017 will allow SonarLint to correctly analyze files in the CMake project.




## CMake - known limitations

#### VS2017: workspaceHash and projectHash macros are not supported in VS2017
VS2017 used a different hashing algorithm from VS2019, which is not supported. See issue [#2632](https://github.com/SonarSource/sonarlint-visualstudio/issues/2632).


This means that SonarLint will not be able to analyse files if the `buildRoot` property uses either of these macros.

#### Environment variable overrides in CMakeSettings.json are not supported
SonarLint will not process environment variable overrides in _CMakeSettings.json_ files.

The SonarLint analysis results may be incorrect if the overridden environment variables affect the CMake compilation e.g. changing the paths that are searched for header files.

#### CMake presets are not currently supported by SonarLint
See the [Microsoft documentation](https://devblogs.microsoft.com/cppblog/cmake-presets-integration-in-visual-studio-and-visual-studio-code/) for more information about support for CMake presets in Visual Studio.

#### Only the Ninja generator is supported
SonarLint uses the generated compilation database (`compile_commands.json`) to fetch the compiler options to use during analysis.
The `Ninja` generator creates a compilation - the other VS generators do not. Visual Studio introduce support for the `Ninja` generator in [VS2017 Update 3 (v15.3)](https://docs.microsoft.com/en-us/visualstudio/releasenotes/vs2017-relnotes-v15.3#open-folder-and---cmake-tools). Newer versions of VS use the `Ninja` generator by default. You can also configure VS to use the `Ninja` generator by providing an appropriate `CMakeSettings.json` file e.g.

```
{
  "configurations": [
    {
      ... 
      "name": "x64-Release",
      "generator": "Ninja",
      ...
      "buildRoot": "${projectDir}\\out\\build\\${name}\\",
      "installRoot": "${projectDir}\\out\\install\\${name}",
      ...
    }
  ]
}
```