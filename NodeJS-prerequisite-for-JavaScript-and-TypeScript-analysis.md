Sonar’s JavaScript and TypeScript analyzers require specific Node.js versions to be installed on the machine. If no compatible version is found, the analysis will not be performed. See the table below for minimal required Node.js version per SonarLint version:

| SonarLint for Visual Studio Version | Minimal Required Node.js Version |
|-|-|
| v6.9 | Node.js v14.17 and higher |
| v6.8 | Node.js v12.22 and higher |
| v6.7 and earlier | Node.js v10 and higher, excluding Node.js v11 |

## Auto-detection of compatible Node.js versions
SonarLint for Visual will attempt to locate a compatible Node.js version on the machine by searching on the %PATH%.
It will also check whether there is a Node.js installation as part of the current Visual Studio installation.

If SonarLint cannot find a compatible Node.js version, it will show a notification in a gold bar like the following:

[[images/JsTs/SLVS_NoSupportedNodeJs_GoldBar_v6_8.png|alt=JavaScript TypeScript no supported Node version gold bar screenshot]]

The Output Window will contain additional information about the Node.js versions that were located:

[[images/JsTs/SLVS_NoSupportedNodeJs_OutputWindow_v6_8.png|alt=JavaScript TypeScript no supported Node version Output Window screenshot]]

## Manually specifying a custom location of Node.js installation
You can also set the environment variable `SONAR_NODEJS_PATH` to specify a custom location. The value should be the full file path to the node executable  e.g. `c:\custom\node.exe`.
The environment variable takes precedence over the automatic detection. You will need to restart Visual Studio after setting or changing the environment variable.

## Installing Node.js
If the machine does not have a compatible version of Node.js, you can manually install one.
Node.js versions can be downloaded from the official website [nodejs.org](https://nodejs.org/en/download/).
Both the 32-bit and 64-bit versions are supported.
The simplest installation method on Windows is to use the appropriate `.msi`.

## Deprecation of older Node.js versions
From time to time the Sonar JavaScript/TypeScript analyzer will increase the minimum required Node.js version as older versions go out of support.
The dropping of support will be announced in advance in the Community Forum (e.g. [this post](https://community.sonarsource.com/t/drop-of-node-js-10-support-and-deprecation-of-node-js-12-support/59590) regarding the deprecation of Node.js v12 and dropping of support for Node.js vs10).

This page will be updated as the minimum required Node.js version changes.