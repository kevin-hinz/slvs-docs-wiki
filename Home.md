# SonarLint for Visual Studio

SonarLint is an IDE extension that helps you detect and fix quality issues as you write code. Like a spell checker, SonarLint squiggles flaws so they can be fixed before committing code. You can get it directly from the VS Marketplace and it will then detect new bugs and quality issues as you code (for C#, VB.NET, JavaScript, TypeScript, C and C++).

SonarLint is also available for VS Code, Eclipse and IntelliJ (the languages supported vary from IDE to IDE).

## How it works

Simply open a project containing C#, VB, C++, JS or TS files.

For C# and VB.Net, new issues will be reported as you type. You do not have to select 'Run Code Analysis' from the 'Analyze' menu - the rules are run automatically.
Note: by default, VS is configured to only run Roslyn analyzers on files that are currently open. You can choose to have the analysis run on the entire solution as described in the [Microsoft docs](https://docs.microsoft.com/en-us/visualstudio/code-quality/how-to-enable-and-disable-full-solution-analysis-for-managed-code?view=vs-2019), although this is obviously more processor-intensive.


For C, C++, JavaScript and TypeScript, new issues will be reported when you open or save a file. Issues are highlighted in your code, and also listed in the 'Error List'.

You can access the detailed rule description directly from the issue in the Error List, using the **Show Error help** option on the contextual menu.

## Rules

Check the rules to see what SonarLint for Visual Studio can do for you:

- [C# rules](https://rules.sonarsource.com/csharp)
- [VB.NET rules](https://rules.sonarsource.com/vbnet)
- [JavaScript rules](https://rules.sonarsource.com/javascript)
- [TypeScript rules](https://rules.sonarsource.com/typescript)
- [C](https://rules.sonarsource.com/c) and [C++ rules](https://rules.sonarsource.com/cpp) 
- [CSS rules](https://rules.sonarsource.com/css) (from SonarLint for Visual Studio v6.16)
- [Secrets detection rules](https://rules.sonarsource.com/secrets)

You will benefit from the following code analyzers: [SonarC#](https://redirect.sonarsource.com/plugins/csharp.html), [SonarVB](https://redirect.sonarsource.com/plugins/vbnet.html), [SonarCFamily for C/C++](https://redirect.sonarsource.com/plugins/cpp.html) and [SonarJS](https://redirect.sonarsource.com/plugins/javascript.html).

## Sonar Rule Help

From SonarLint for Visual Studio v6.14+, users are able to visualize descriptive and educational content associated with each issue. Simply select the issue’s rule as shown below to open the **Sonar Rule Help** view.

[[images/Home/slvs-sonar-rule-help.gif|Sonar Rule Help]]

The Sonar Rule Help view brings rule descriptions and patch instructions relevant to the library or framework you’re using, directly into the IDE.

## Requirements

SonarLint for Visual Studio (SLVS) is available for Visual Studio version 2019.3+ and 2022; SLVS is available for Visual Studio 2017 but is no longer supported. The only thing you need to install is the VSIX which is available on the [Visual Studio Marketplace](https://marketplace.visualstudio.com/search?term=sonarlint&target=VS&category=All%20categories&vsVersion=&sortBy=Relevance).

In versions prior to v4.34, additional steps were required for some languages; see [this page for more details](https://github.com/SonarSource/sonarlint-visualstudio/wiki/%5BObsolete,-for-versions-prior-to-v4.34-only%5D-Support-for-Additional-Languages).

## Installation

SonarLint is available as [an extension for Visual Studio](https://learn.microsoft.com/en-us/visualstudio/ide/finding-and-using-visual-studio-extensions?view=vs-2022). 

To install SonarLint from within Visual Studio:
1. From Visual Studio, go to **Extensions** > **Manage Extensions** and search **SonarLint for Visual Studio**. 
2. Select **SonarLint for Visual Studio** and click **Download**.

The extension will be installed after all instances of Visual Studio have been closed.


## Offline installation

To install SonarLint offline, you must first download the SonarLint for Visual Studio executable file from the Visual Studio Marketplace. Administrators can download the plugin and distribute the installer internally if needed.

1. Go to the [Visual Studio Marketplace](https://marketplace.visualstudio.com/search?term=sonarlint&target=VS&category=All%20categories&vsVersion=&sortBy=Relevance) and search for sonarlint. 
2. Choose the SonarLint for Visual Studio version [2019](https://marketplace.visualstudio.com/items?itemName=SonarSource.SonarLintforVisualStudio2019) or [2022](https://marketplace.visualstudio.com/items?itemName=SonarSource.SonarLintforVisualStudio2022), that corresponds to your Visual Studio version.
3. Click **Download**.
4. Once the file is on your disk, double-click it to launch the installation.

The extension will be installed after all instances of Visual Studio have been closed.


### Standalone mode
By default SonarLint runs in standalone mode i.e. completely independent of SonarQube/SonarCloud.

#### Choosing which C#/VB.NET rules to run in Standalone mode
The SonarC# and SonarVB rules are implemented as [Roslyn VSIX analyzers](https://docs.microsoft.com/en-us/visualstudio/code-quality/install-roslyn-analyzers?view=vs-2019), and you can configure which rules are executed using the normal [ruleset](https://docs.microsoft.com/en-us/visualstudio/code-quality/use-roslyn-analyzers?view=vs-2019#rule-sets) mechanism in VS.

#### Choosing which C/C++/JavaScript/TypeScript/Secrets detection rules to run in Standalone mode
See [Choosing which C, C++, JavaScript, TypeScript or Secrets detection rules to run in Standalone mode](https://github.com/SonarSource/sonarlint-visualstudio/wiki/Choosing-which-C,-Cpp,-JavaScript,-TypeScript-or-Secrets-detection-rules-to-run-in-Standalone-mode)

## Connected Mode
In Connected Mode, the solution is linked to a project in SonarQube/SonarCloud. See [Connected Mode](https://github.com/SonarSource/sonarlint-visualstudio/wiki/Connected-Mode) for more information. Connected Mode is supported for all currently supported languages.

## Rule severities
The rule [severities](https://docs.microsoft.com/en-us/visualstudio/code-quality/use-roslyn-analyzers?view=vs-2019#rule-severity) defined by Visual Studio are different from the rule [severities](https://docs.sonarqube.org/latest/user-guide/issues/) defined by SonarQube/SonarCloud. The mapping from Sonar to VS severities is as follows:

| SonarQube/SonarCloud | Visual Studio |
|---------------|----------------------|
|Info | Info |
|Minor | Info |
|Major | Warning |
|Critical | Warning |
|Blocker | Warning |

By default Sonar `Critical` and `Blocker` issues are not mapped to Visual Studio `Error` as this would cause IDE builds to fail.
You can change that by enabling `Treat warnings as errors` in your project properties in Visual Studio.
If you are using Connected Mode, the rule severities defined in the Quality Profile will be used.

## Contributions

If you would like to see a new feature, please create a new thread in the forum ["Suggest new features"](https://community.sonarsource.com/c/suggestions/features).

Please be aware that we are not actively looking for feature contributions. The truth is that it's extremely difficult for someone outside SonarSource to comply with our roadmap and expectations. Therefore, we typically only accept minor cosmetic changes and typo fixes.

With that in mind, if you would like to submit a code contribution, please create a pull request for this repository. Please explain your motives to contribute this change: what problem you are trying to fix, what improvement you are trying to make.

Make sure that you follow our [code style](https://github.com/SonarSource/sonar-developer-toolset#code-style) and all tests are passing.

## Have Question or Feedback?

For SonarLint support questions ("How do I?", "I got this error, why?", ...), please first read the [FAQ](https://community.sonarsource.com/t/frequently-asked-questions/7204) and then head to the [SonarSource forum](https://community.sonarsource.com/c/help/sl). There are chances that a question similar to yours has already been answered. 

Be aware that this forum is a community, so the standard pleasantries ("Hi", "Thanks", ...) are expected. And if you don't get an answer to your thread, you should sit on your hands for at least three days before bumping it. Operators are not standing by. :-)

## License

Copyright 2017-2023 SonarSource.

Licensed under the [GNU Lesser General Public License, Version 3.0](http://www.gnu.org/licenses/lgpl.txt)
