# How can I configure the rules C#/VB.NET rules to use?

SonarLint itself does not have any special behaviour in relation to configuring the set of Sonar C#/VB.NET to rules. The Sonar C# and VB.NET rules are implemented as Roslyn analyzers and use the standard Visual Studio features to enable and disable rules. 

For VS2015 and VS2017 this means [ruleset files](https://docs.microsoft.com/en-us/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules?view=vs-2019). VS2019+ users can use [.editorconfig files](https://docs.microsoft.com/en-us/visualstudio/code-quality/use-roslyn-analyzers?view=vs-2019#set-rule-severity-in-an-editorconfig-file) instead.

The simplest approach is just to reference the ruleset directly from the project and check the ruleset file in to source control. This won't have any adverse impact on machines that don't have SonarLint installed; Visual Studio will load the ruleset file, but it won't have any impact on build or in the IDE because the analyzers are not available.

# How can I configure the C#/VB.NET rules to use without changing the project file or checking files in to source control?
 
There are a number of ways to achieve this, depending on the version of VS you are using. Some of the possible solutions are as listed below. There may be others - see the Microsoft documentation [Customize your build](https://docs.microsoft.com/en-us/visualstudio/msbuild/customize-your-build?view=vs-2019) for more ideas.

### 1) Configure the rules in a .editorconfig file that is not under source control
The Visual Studio will search up the directory tree from the solution folder to locate a .editorconfig file. Create a .editorconfig file containing the required settings and drop it in a folder above the solution that is not under source control.
See [Set rule severity in an EditorConfig file](https://docs.microsoft.com/en-us/visualstudio/code-quality/use-roslyn-analyzers?view=vs-2019#set-rule-severity-in-an-editorconfig-file) in the Microsoft documentation for more information (setting a severity to "none" will disable a rule).

#### Limitations:
* this will only work in VS2019 or later
* it won't work if your solution already references a .editorconfig file

### 2) Add a Directory.Build.props file that references the ruleset file
The [Directory.Build.props file](https://docs.microsoft.com/en-us/visualstudio/msbuild/customize-your-build?view=vs-2017) will automatically be imported by MSBuild (and by extension Visual Studio). The file can be in the project directory or any parent directory so it should be possible to put it in a folder above the project/solution that is not under source code control.

Example:
```
<!-- Sample Directory.Build.props content -->
<Project>
  <PropertyGroup>
    <CodeAnalysisRuleSet>$(MSBuildThisFileDirectory)Example.ruleset</CodeAnalysisRuleSet>
  </PropertyGroup>
</Project>
```

#### Limitations:
* this will only work on VS2017 or later.
* it won't work if your solution has a Directory.Build.props file checked in.
* it won't work if your solution already references a ruleset file.

### 3) Conditionally reference the ruleset file from the project, but don't check the ruleset file in to source control. The ruleset can be placed anywhere on your machine.

Example:
```
<!-- Snippet to conditionally set the ruleset to file that might not exist on all dev machines -->
<PropertyGroup>
  <CodeAnalysisRuleset Condition="Exists('$(LocalAppData)\Example.ruleset')">$(LocalAppData)\Example.ruleset</CodeAnalysisRuleset>
</PropertyGroup>
```

#### Limitations:
* this does involve changing the project file(s), but only to add a single property.

### 4) Drop a `props` file that imports the ruleset file into the MSBuild ImportBefore folders
Project files that use the standard Microsoft build targets will automatically import any targets files that are exist in a well-known location. You can use this feature to conditionally import a ruleset.

This is similar to option (2) above but works with all versions of MSBuild.

Example:
```
<?xml version="1.0" encoding="utf-8"?>
<!-- Example props file to drop in %LocalAppData%\Microsoft\MSBuild\[VERSION]\Microsoft.Common.targets\ImportBefore -->
<Project xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <CodeAnalysisRuleset Condition="Exists('$(LocalAppData)\Example.ruleset')">$(LocalAppData)\Example.ruleset</CodeAnalysisRuleset>
  </PropertyGroup>
</Project>
```

Notes:
* the location of the `ImportBefore` folder varies depending on the version of MSBuild, based on the following standard pattern:
`%LocalAppData%\Microsoft\MSBuild\[VERSION]\Microsoft.Common.targets\ImportBefore`.
The `[VERSION]` values for MSBuild 14 and 15 are `14.0` and `15.0` respectively. MSBuild 16 and latter use `Current`.
* the `ImportBefore` folder does not exist by default so you might have to create it.

#### Limitations:
* it won't work if your project already references a ruleset file.
* the location of the ImportBefore folder varies according to the version of MSBuild, so if you are using multiple versions of Visual Studio you will need to drop the same file in multiple locations.
