
SonarLint now provides ability to fix select issues with quick fixes. 

## Supported languages
Click on the language to see the list of supported rules.
* [VB.NET](https://rules.sonarsource.com/vbnet/quickfix) , [C#](https://rules.sonarsource.com/csharp/quickfix/) 
* [C](https://rules.sonarsource.com/c/quickfix/), [C++](https://rules.sonarsource.com/cpp/quickfix/) (SonarLint v5.5 or higher)


## Feature overview

When a file is open and there are issues with quick fixes then a lightbulb will appear on the line of the issue.

[[images/QuickFixes/SLVS_QuickFixes_Lightbulb_v5_5.png|alt=LightBulb on line with QuickFixes]]

If lightbulb is clicked a menu with possible quick fixes will be opened. 

[[images/QuickFixes/SLVS_QuickFixes_Fixes_v5_5.png|alt=Menu with applicable quick fixes]]

If one of the quick fixes are selected the fix will be applied automatically. 

[[images/QuickFixes/SLVS_QuickFixes_Fixed_v5_5.png|alt=Fixed applied]]

## Known Limitations

* Quick fixes for the whole line are always shown [#2878](https://github.com/SonarSource/sonarlint-visualstudio/issues/2878)
* When an edit elsewhere invalidates an issue quick fixes are visible till a new analysis is run. 