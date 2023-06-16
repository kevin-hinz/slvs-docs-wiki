# Supported IDEs

See https://www.sonarlint.org/ for information about the currently supported IDEs, and also for details of which languages are supported in which IDEs. One notable supported IDE is `Rider` for C# development.

# Unsupported IDEs

SonarLint is not currently available for `Visual Studio for Mac`. Also, the SonarLint extension for `Visual Studio Code` does not currently support analysing C# code.

However, it is still possible to use the Sonar C# and VB.NET analyzers in those IDEs with a little manual configuration.

## Support for analysing C#/VB.NET in other IDEs
`Visual Studio Code` and `Visual Studio for Mac` all support running Roslyn analyzers that are shipped as NuGet packages. This includes Sonar C# and VB.NET rules.

The NuGet package id for the Sonar C# rules is [SonarAnalyzer.CSharp](https://www.nuget.org/packages/SonarAnalyzer.CSharp/).
The NuGet package id for the Sonar VB.NET rules is [SonarAnalyzer.VisualBasic](https://www.nuget.org/packages/SonarAnalyzer.VisualBasic)

See below for information on setting up analysis in specific IDEs.

### Visual Studio Code
See the [OmniSharp blog post from April 2019](https://www.strathweb.com/2019/04/roslyn-analyzers-in-code-fixes-in-omnisharp-and-vs-code/).

### Visual Studio for Mac
To configure a project to use the Sonar C# analyser, edit the project file to include the following XML:

```
  <!-- Import and configure the SonarC# analyzer -->
  <PropertyGroup>
    <SonarAnalyzerVersion>8.2.0.14119</SonarAnalyzerVersion>
  </PropertyGroup>
  <ItemGroup>
    <PackageReference Include="SonarAnalyzer.CSharp" Version="$(SonarAnalyzerVersion)" />
    <Analyzer Include="$(NuGetPackageRoot)SonarAnalyzer.CSharp/$(SonarAnalyzerVersion)/analyzers/*.dll" Visible="false" />
  </ItemGroup>
```

Use `SonarAnalyzer.VisualBasic` instead of `SonarAnalyzer.CSharp` to configure the Sonar VB.NET analyser.

Note: instead of editing every project file you could create a single [Directory.Build.props](https://docs.microsoft.com/en-us/visualstudio/msbuild/customize-your-build?view=vs-2019#directorybuildprops-and-directorybuildtargets) file at solution level or repo level that will apply to all projects in the solution/repo.

Note: you can add references to NuGet packages through the UI but that will not add the necessary `Analzyer`entries in the project file - you would still have to add them manually by editing the project file. See this [StackOverflow post](https://stackoverflow.com/questions/49693416/visual-studio-for-mac-support-of-roslyn-analyzers) for more information.