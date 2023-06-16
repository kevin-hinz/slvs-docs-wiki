In standalone mode, it is possible to enable or disable rules, configure rule parameters, and change the reported rule severity.
However, not all options are supported for all languages. See the table below for more information:

| Language | From version | Enable/disable | Change severity | Configure rule parameters |
|-|-|-|-|-|
| C | 4.13 | Supported | Supported | Supported |
| C++ | 4.13 | Supported | Supported | Supported |
| JavaScript | 4.35 | Supported | Not supported. See [#2399](https://github.com/SonarSource/sonarlint-visualstudio/issues/2399). | Not supported. See [#2400](https://github.com/SonarSource/sonarlint-visualstudio/issues/2400). |
| TypeScript | 4.35 | Supported | Not supported. See [#2399](https://github.com/SonarSource/sonarlint-visualstudio/issues/2399). | Not supported. See [#2400](https://github.com/SonarSource/sonarlint-visualstudio/issues/2400). |
| Secrets detection | 6.4 | Supported | Not Supported | Not applicable |


## Disabling a rule
To disable a rule, select an instance of the rule in the Error List and click the `Disable SonarLint rule` command on the context menu:

[[images/SLVS_ErrorList_DisableCFamily_v5.png|alt=Disable SonarLint rule menu command]]

You can view and change the current rule settings by using `Extensions → SonarLint → Options` menu:

[[images/SLVS_ToolsOptions_Edit_Rules_v4_35.png|alt=Edit rules settings button on the SonarLint Tools Option page.]]

When the rule settings are changed (either by using the `Disable SonarLint rule` command or by directly editing and saving the settings.json file), SonarLint will automatically re-analyse all open documents.

The SonarLint Output window tab contains text output describing the processing that has taken place e.g.:

[[images/SLVS_ReanalysisOutputWindow_v4_13.png|alt=SonarLint output window example text]]

### settings.json file format and location
The `settings.json` file is stored in user's roaming profile `%APPDATA%\SonarLint for Visual Studio`. It applies to all supported versions of Visual Studio. If the machine is domain-joined, then the settings file will be automatically copied to any other machine in the domain that the user logs on to.

The format of the settings file is as follows:
```
{
  "sonarlint.rules": {
    "c:S1135": {
      "level": "On",
      "severity": "Info"
    },
    "cpp:S1199": {
      "level": "Off",
      "severity": "Minor"
    },
    "cpp:SingleGotoOrBreakPerIteration": {
      "level": "On",
      "parameters": {
        "maxNumberOfTerminationStatements": "1"
      },
      "severity": "Major"
    },
    "javascript:S1135": {
      "level": "Off"
    },
    "typescript:S1854": {
      "level": "On"
    }
  }
}
```


The format of the rule key in the file is `[c|cpp|javascript|typescript]:[rule id]`. The full list of rules is available at https://rules.sonarsource.com/.

Valid severity values are `Blocker`, `Critical`, `Major`, `Minor` and `Info`. Changing the severity of a rule only affects the label in the `Category` column in the Error List.

If the settings file does not contain an entry for a rule then the default setting for the rule in the SonarWay Quality Profile will be used.

**Note:** if the solution is in [Connected Mode](https://github.com/SonarSource/sonarlint-visualstudio/wiki/Connected-Mode) then the settings file is ignored. Connected Mode is not currently for JavaScript or TypeScript - see [#770](https://github.com/SonarSource/sonarlint-visualstudio/issues/770). It is also not available for Secrets detection rules, which are only run in the IDE and so cannot be configured on the server.