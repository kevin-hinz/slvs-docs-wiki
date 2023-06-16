Starting with version 4.21, it is possible to create a "reproducer" file for C/C++ analysis. A reproducer file contains diagnostic information to help the developers at SonarSource investigate problems that occur during C/C++ analysis.

**Note:**
Starting with version 4.27, a second "reproducer" file is created with additional debug information.

If you encounter a problem with C/C++ analysis, please contact us in the [community forum](https://community.sonarsource.com/) and include the reproducer file(s). 

## Creating a reproducer file for the Active Document

To create a reproducer:
* open the C/C++ file that is not being analysed correctly
* open Visual Studio's [Command Window](https://docs.microsoft.com/en-us/visualstudio/ide/reference/command-window?view=vs-2019), found under `View` -> `Other Windows` -> `Command Window`.
* type in the command window "Analyze.SonarLint.CFamily.CreateReproducer":

[[images/SLVS_CreateReproducer_v4_21.PNG|alt=Create reproducer command]]

You can then view the location of the created reproducer file(s) in the SonarLint Output pane:

Reproducer file in SonarLint 4.21-4.26
[[images/SLVS_CreateReproducer_OutputPane_v4_21.png|alt=Reproducer information in the SonarLint output pane versions 4.21-4.26.]]

Reproducer files in SonarLint 4.27
[[images/SLVS_CreateReproducer_OutputPane_v4_27.png|alt=Reproducer information in the SonarLint output pane version 4.27.]]

**Notes:**
* the reproducer file(s) are regenerated each time the command is invoked. Invoking the command will overwrite the previous reproducer file(s).
* running the reproducer will only produce the reproducer file(s). It will not report any issues for the active C/C++ file.