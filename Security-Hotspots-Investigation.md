Starting with version 4.29, SonarLint now provides a way to investigate [Security Hotspots](https://docs.sonarqube.org/latest/user-guide/security-hotspots/) found on your [SonarQube](https://www.sonarqube.org/) server. This is an integration feature: when viewing a hotspot on your SonarQube server in your browser, you will notice a new button named "Open in IDE"; clicking on the button whilst your Visual Studio is open will open the hotspot's code file in your IDE. 

## Feature requirements

* SonarQube version 8.6 or higher.
* SonarLint version 4.29 or higher.
* The correct solution must be open in Visual Studio and it must be in [Connected Mode](https://github.com/SonarSource/sonarlint-visualstudio/wiki/Connected-Mode). SonarQube will not open Visual Studio if it is closed.

## Feature overview

When an “Open in IDE” request is received from the browser, SonarLint will verify that the correct solution is open in Connected Mode. If not, a gold bar will be displayed with additional information being logged in the Output Window:

[[images/Hotspots/SLVS_HostpotsGoldbar_v4_29.PNG|alt=Request gold bar]]
[[images/Hotspots/SLVS_HotspotsOutputWindowError_v4_29.PNG|alt=Request error details in the output window]]

If the correct solution is open and the hotpot's code location can be found in the solution, SonarLint will open the file and navigate to the relevant code. In addition, the hotspot will be added to a new tool window, `SonarLint Security Hotspots`, where you can see additional information:

[[images/Hotspots/SLVS_NavigableHotspot_v4_29.PNG|alt=Hotspot code in the IDE]]

However, it is possible that the code in your SonarQube server does not match your local code version, e.g. if code changes have been made since the last analysis or if the relevant code project is not included in the solution. In this case, SonarLint will not be able to locate the hotspot and it will be added to the list with an indication that it is not navigable:

[[images/Hotspots/SLVS_NotNavigableHotspot_v4_29.PNG|alt=Hotspot code cannot be found in the IDE]]

### Security Hotspots list functionality

Once a hotspot has been added to the list, you can navigate to it using a double-click or the Enter key.
In order to remove a hotspot from the list, use the right-click context menu or the Del key. This will only remove the hotspot from the list - it will not have any effect on the hotspot in your SonarQube server.

[[images/Hotspots/SLVS_RemoveHotspot_v4_29.PNG|alt=Remove a hotspot from the list]]

## Implementation notes

When Visual Studio starts, SonarLint will start listening in the background for “Open in IDE” requests originating from your local browser. This listener does not require a lot of resources and should not affect your machine's performance and memory consumption in any way, nor should it interfere with your work. 
SonarLint will try to find an available port in the range 64120-64130 inclusive. Information about the port selection will be logged in the SonarLint pane in the Output Window. If a port cannot be found then “Open in IDE” will not be handled. The port range is not configurable.

