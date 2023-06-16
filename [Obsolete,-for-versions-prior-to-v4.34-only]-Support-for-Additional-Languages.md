# WARNING
This page describes the behavior of SonarLint versions 4.34 or older. Javascript analysis is enabled by default version 4.34+.

## Overview
Out of the box, SLVS will analyse C#, VB.NET and C/C++. Support for additional languages requires the download of another component, the SonarLint daemon. This simply requires clicking a button in the `Tools`, `Options`, `SonarLint` tab as described below.

The only additional language currently supported is JavaScript, although it is likely this set of supported languages will increase in the future.

### Enabling analysis of additional languages
To enable analysis of additional languages, select Tools, Options, SonarLint and click on the "Activate" button then clicking "OK". The SonarLint daemon binaries will be downloaded in the background, with the download progress being shown in the Visual Studio status bar.

Once the download has completed, JavaScript files will be analysed whenever you save them.

### Specifying an alternative download location (SLVS v4.11 or later)
By default the additional binaries will be downloaded from the public site https://binaries.sonarsource.com/Distribution/sonarlint-daemon/.
If you are working in an environment in which developer machines do not have access to the public internet, you can host the required binaries yourself and set the environment variable `SONARLINT_DAEMON_DOWNLOAD_URL` to tell SLVS where to find the binaries. 

There are few restrictions:
* the environment variable SONARLINT_DAEMON_DOWNLOAD_URL must be set before VS starts.
* the URL must be absolute.
* the daemon zip file must not be renamed i.e. it must in the format `sonarlint-daemon-{version}-windows.zip`.

### Changes to C/C++ analysis in SLVS v4.12
* Prior to SLVS v4.12 C++ was an additional language so C/C++ analysis would only occur if support for additional languages was enabled. Since v4.12 the C/C++ analysis binaries have been included in the SLVS VSIX so the SonarLint daemon is not required. If you were using SLVS to analyse C/C++ prior to v4.12, you no longer need to have support for additional languages enabled unless you also want JavaScript files to be analysed. 


### The SonarLint daemon
Most of the Sonar analyzers are written in Java and so need to be run in a Java process. The SonarLint daemon is a Java component that runs in the background and hosts the supported Sonar analyzers.  There is no need to have Java installed separately on your development machine - the SonarLint daemon download includes the necessary Java runtime files.

SLVS handles communication with the daemon behind the scenes. Informational messages about the daemon will appear in the `SonarLint` pane of the Visual Studio `Output` tool window. Otherwise, the existence of the daemon should be completely transparent to the end user: SLVS will take care of starting and stopping the daemon, passing files for analysis, processing the results and displaying them in the Visual Studio `Error List`.

The daemon will only be started when you open a file that can be analysed by the daemon, and it will be closed automatically when you close the solution containing the file (note that this was not the case prior to SLVS v4.12; in older versions of SLVS the daemon would start when VS started).

You can use the Task Manager to check whether the daemon is running:

| Field | Value|
| --- | ---|
| Process name | java.exe |
| Process description | OpenJDK Platform binary |
| File location | %LOCALAPPDATA%\SonarLint for Visual Studio\sonarlint-daemon-{version}-windows\jre\bin |
