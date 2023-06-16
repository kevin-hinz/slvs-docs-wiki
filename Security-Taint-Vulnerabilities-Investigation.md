SonarLint now provides a way to investigate [Taint Vulnerabilities](https://docs.sonarqube.org/latest/user-guide/security-rules/) found on [SonarCloud](https://sonarcloud.io/) or your [SonarQube](https://www.sonarqube.org/) server. 

## Feature requirements

* SonarLint version 4.31 or higher.
* The correct solution must be open in Visual Studio and it must be in [Connected Mode](https://github.com/SonarSource/sonarlint-visualstudio/wiki/Connected-Mode) to SonarCloud or SonarQube version 8.6 or higher.

## Feature overview

When a solution connected to SonarCloud or SonarQube is open in Visual Studio, SonarLint will fetch the vulnerabilities from the configured server. If any vulnerabilities exist, a new tool window will be displayed in a new tab next to the Error List:

[[images/Taint/SLVS_TaintTab_v4_31.PNG|alt=Taint vulnerabilities tool window]]

The tool window will appear automatically if your server has any taint vulnerabilities in your project. If you are not in Connected Mode, or if your server has no taint vulnerabilities, the window will not appear.

### Taint Vulnerabilities list

The taint list is filtered to display remote vulnerabilities found in the currently open code file. When a file containing issues is opened, the caption of the tool window will update to reflect the number of remote vulnerabilities found in the file:

[[images/Taint/SLVS_TaintHasIssues_v4_31.PNG|alt=Number of remote taint vulnerabilities found in the current file]]

[[images/Taint/SLVS_TaintList_v4_31.PNG|alt=Remote Taint Vulnerabilities found in the current file]]

The header of the list will display information about the analysis in which these issues were found:

[[images/Taint/SLVS_TaintAnalysisInfo_v4_31.PNG|alt=Analysis Information]]

**Note**: Currently SonarLint does not detect Taint Vulnerabilities during live analysis in the IDE. The issues appearing in the Taint Vulnerabilities list are the issues reported on your SonarQube or SonarCloud server.

### Investigating Taint Vulnerabilities

You can investigate a vulnerability by using a double-click or the Enter key. This will take you to the relevant code location and open the [SonarLint Issue Visualization](https://github.com/SonarSource/sonarlint-visualstudio/wiki/Visualizing-issues-with-secondary-locations) panel with visualization of your code flow.

[[images/Taint/SLVS_TaintSecondaryLocations_v4_31.PNG|alt=Taint Vulnerabilities Flow]]

**Note**: if you do not see the Issue Visualization panel, click on Extensions → SonarLint →  Show Issue Visualization. See [SonarLint Issue Visualization](https://github.com/SonarSource/sonarlint-visualstudio/wiki/Visualizing-issues-with-secondary-locations) for more information.

#### Non-navigable code locations

Since taint vulnerabilities are fetched from your configured server, it is possible that the code on your server does not match your local code version, e.g. if code changes have been made since the last analysis. In this case, non-navigable locations will be displayed with an indication that they are not navigable:

[[images/Taint/SLVS_TaintNonNavigable_v4_31.PNG|alt=Non navigable locations]]

#### Manually re-opening SonarLint Taint Vulnerabilities tool window

If you manually close the tool window, it will no longer appear and disappear automatically when a solution is opened. You can show the window again by clicking on Extensions → SonarLint → Connected Mode →  View Taint Vulnerabilities:

[[images/Taint/Taint_ViewButton.png|alt=Taint vulnerabilities tool window command]]