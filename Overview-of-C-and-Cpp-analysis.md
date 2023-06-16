### Supported project types

#### 1. vcxproj projects
SonarLint can analyse _.vcxproj_ projects that use the standard Visual Studio compilation tools.
Projects that use third-party tools (such as Unity projects) are not supported.

#### 2. CMake projects - supported in v4.38+
Visual Studio 2017 and later support opening CMake projects in the IDE (see the [Microsoft documents](https://docs.microsoft.com/en-us/cpp/build/cmake-projects-in-visual-studio?view=msvc-160) for more information).

Starting with v4.38, SonarLint can analyse a subset of CMake project types that meet the following requirements:
* Visual Studio 2017 Update 3 or later is used
* Visual Studio is configured to use the Ninja generator (the default), and
* the _CMakeLists.txt_ file contains the following command: `set(CMAKE_EXPORT_COMPILE_COMMANDS ON)`.

See the [Getting Started with analyzing CMake projects in Visual Studio](https://github.com/SonarSource/sonarlint-visualstudio/wiki/Getting-Started-with-analyzing-CMake-projects-in-Visual-Studio) for more information.

### Triggering analysis

C/C++ analysis is triggered when a file is opened or saved. Any existing issues for the document will immediately be removed from the _Error List_. 
New issues will be added to the _Error List_ incrementally as they are detected.

The status bar will be updated to show the file is being analysed...

[[images/CFamily/CFamily_statusbar_inprogress_v4_23.png|alt=Screenshot of status bar showing analysis is in progress]]

... and again when analysis has finished:

[[images/CFamily/CFamily_statusbar_finished_v4_23.png|alt=Screenshot of status bar showing analysis has finished]]

Additional detailed information will be shown in the SonarLint pane of the _Output Window_.

_Note: SonarLint only displays status information in the status bar, as shown in the the above screen shots. Some versions of Visual Studio provide other C++ analysis rules from Microsoft that display progress information in the background tasks pop-up window. These messages do not relate to SonarLint analysis. See [here](https://docs.microsoft.com/en-us/cpp/code-quality/code-analysis-for-c-cpp-overview?view=vs-2019) or more information on the Microsoft C++ rules._

### C/C++ analysis performance

SonarLint has [450+ rules](https://rules.sonarsource.com/cpp) for C++ and [280+ rules](https://rules.sonarsource.com/c) for C. Most run very quickly and will return issues in under a second. However, some of the more advanced rules that involve semantic analysis can take more than ten seconds to run.

Analysis is performed in the background in a separate process. It should not have a noticeable impact on the user experience in Visual Studio.
If the analysis is taking too long for a particular file then the analysis process will be terminated. If an analysis is being re-triggered (e.g. via a re-save of the file), the previous analysis will be cancelled and a new analysis will start.

Prior to v4.22, issues were only displayed in the Error List after all rules had finished processing. As of v4.23, issues are displayed in the _Error List_ as soon as they are available. This means that most issues will appear within a few seconds. However, the status bar may still continue to display the `analyzing xxx.cpp` message for some time until all of the rules have finished processing.


### Analysis timeout
By default, an analysis that has not completed within one minute will be stopped and a message logged in the _Output Window_.
We expect such timeouts to be very rare, and probably indicates a problem in one of our analysis rules. If you do encounter timeouts when analysing files, please open an issue in the _SonarLint_ section of the [Community Forum](https://community.sonarsource.com/c/help/sl/11) and tag it with `cfamily`.