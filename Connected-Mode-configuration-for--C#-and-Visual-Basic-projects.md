# What configuration does SLVS add to my C#/VB.NET projects when binding to a SonarQube/SonarCloud server?

When binding to SonarQube/SonarCloud, SLVS generates configuration files that are needed for Sonar C# and Sonar VB.NET analyzers. 
The following configuration files are generated for each language:
* a `.ruleset` file that contains the rules configuration corresponding to the Quality Profile (See the Microsoft documentation for [Rule sets](https://docs.microsoft.com/en-us/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules?view=vs-2019))
* a `SonarLint.xml` file which contains the rules parameters for Sonar C# and Sonar VB.NET analyzers.

The configuration files are located under the `.sonarlint` folder in your solution directory.

Each non-test project under your solution needs to reference all of these files in order to be considered as “correctly bound”. If one of the configuration files is not referenced, the project is considered as unbound and SLVS will prompt you to bind it.

## How does SLVS recognize that the configuration files are referenced?

The generated ruleset is specified in the `CodeAnalysisRuleSet` property e.g.
```
<PropertyGroup>
  <CodeAnalysisRuleSet>path-to-generated-ruleset.ruleset</CodeAnalysisRuleSet>
</PropertyGroup>
```

The `SonarLint.xml` file is referenced as an `AdditionalFiles` item e.g.
```
<ItemGroup>
  <AdditionalFiles Include="..\.sonarlint\{language}\SonarLint.xml" />
</ItemGroup>
```



# What happens when SLVS binds my C# / VB.NET projects?

If SLVS recognizes that the project does not reference the generated `.ruleset` file, SLVS will reference it using the following logic:

If the project has no `CodeAnalysisRuleSet` properties, SLVS will create one and point to the generated `.ruleset` file. So your project will look like this:
```
<PropertyGroup>
  <CodeAnalysisRuleSet>path-to-generated-ruleset.ruleset</CodeAnalysisRuleSet>
</PropertyGroup>
```
If the project has a `CodeAnalysisRuleSet` property that points to a `.ruleset` file that is located under the project’s directory, SLVS will amend that ruleset to reference the generated `.ruleset` file. So your project’s ruleset file will look like this:
```
<?xml version="1.0" encoding="utf-8"?>
<RuleSet Name="My ruleset" Description="test" ToolsVersion="16.0">
  <Include Path="path-to-generated-ruleset.ruleset" Action="Default" />
  ...
```
 If the project has a `CodeAnalysisRuleSet` property that points to a `.ruleset` file that is not located under the project’s directory, SLVS will create a new `.ruleset` file and place it under your project’s directory. The new ruleset file references your previous ruleset and Sonar’s generated `.ruleset` file. So the new ruleset file will look like this:
```
<?xml version="1.0" encoding="utf-8"?>
<RuleSet Name="SonarQube - Sonar way" ToolsVersion="14.0">
  <Include Path="path-to-generated-ruleset.ruleset" Action="Default" />
  <Include Path="path-to-your-other-ruleset.ruleset" Action="Default" />
</RuleSet>
``` 

# Can I customize how my projects are configured?

Yes. The initial binding described above will correctly configure your projects, but you are free to modify this using the standard capabilities of MSBuild e.g. using a `Directory.Build.props` file, or putting the references in a common targets file that is included in the appropriate projects.

FYI the SonarLint for Visual Studio solution in this repo uses a `Directory.Build.props` file ([see here](https://github.com/SonarSource/sonarlint-visualstudio/blob/master/Directory.Build.props#L3)). It does not contain any project-level rulesets or settings.

See [our documentation](https://github.com/SonarSource/sonarlint-visualstudio/wiki/Choosing-which-C%23-or-VB.NET-rules-to-run-in-Standalone-mode) on how to configure a reference to project rulesets, and [Microsoft’s documentation](https://docs.microsoft.com/en-us/visualstudio/msbuild/customize-your-build?view=vs-2019) for customizing your build.
