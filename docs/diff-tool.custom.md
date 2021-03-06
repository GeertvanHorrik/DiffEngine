<!--
GENERATED FILE - DO NOT EDIT
This file was generated by [MarkdownSnippets](https://github.com/SimonCropp/MarkdownSnippets).
Source File: /docs/mdsource/diff-tool.custom.source.md
To change this file edit the source file and then run MarkdownSnippets.
-->

# Custom Diff Tool

A custom tool can be added by calling `DiffTools.AddTool`

<!-- snippet: AddTool -->
<a id='snippet-addtool'/></a>
```cs
var resolvedTool = DiffTools.AddTool(
    name: "MyCustomDiffTool",
    autoRefresh: true,
    isMdi: false,
    supportsText: true,
    requiresTarget: true,
    arguments: (tempFile, targetFile) => $"\"{tempFile}\" \"{targetFile}\"",
    exePath: diffToolPath,
    binaryExtensions: new[] {"jpg"})!;
```
<sup><a href='/src/DiffEngine.Tests/DiffToolsTest.cs#L23-L33' title='File snippet `addtool` was extracted from'>snippet source</a> | <a href='#snippet-addtool' title='Navigate to start of snippet `addtool`'>anchor</a></sup>
<!-- endsnippet -->

Add a tool based on existing resolved tool:

<!-- snippet: AddToolBasedOn -->
<a id='snippet-addtoolbasedon'/></a>
```cs
var resolvedTool = DiffTools.AddToolBasedOn(
    DiffTool.VisualStudio,
    name: "MyCustomDiffTool",
    arguments: (temp, target) => $"\"custom args {temp}\" \"{target}\"");
```
<sup><a href='/src/DiffEngine.Tests/DiffToolsTest.cs#L62-L67' title='File snippet `addtoolbasedon` was extracted from'>snippet source</a> | <a href='#snippet-addtoolbasedon' title='Navigate to start of snippet `addtoolbasedon`'>anchor</a></sup>
<!-- endsnippet -->


## Resolution order

New tools are added to the top of the order, the last tool added will resolve before any existing tools. So when the following is executeed the last tool that supports the file types will launch:

<!-- snippet: DiffRunnerLaunch -->
<a id='snippet-diffrunnerlaunch'/></a>
```cs
DiffRunner.Launch(tempFile, targetFile);
```
<sup><a href='/src/DiffEngine.Tests/DiffRunnerTests.cs#L18-L20' title='File snippet `diffrunnerlaunch` was extracted from'>snippet source</a> | <a href='#snippet-diffrunnerlaunch' title='Navigate to start of snippet `diffrunnerlaunch`'>anchor</a></sup>
<!-- endsnippet -->

Alternatively the instance  returned from `AddTool*` can be used to explicitly launch that tool.

<!-- snippet: AddToolAndLaunch -->
<a id='snippet-addtoolandlaunch'/></a>
```cs
var resolvedTool = DiffTools.AddToolBasedOn(
    DiffTool.VisualStudio,
    name: "MyCustomDiffTool",
    arguments: (temp, target) => $"\"custom args {temp}\" \"{target}\"");

DiffRunner.Launch(resolvedTool!, "PathToTempFile", "PathToTargetFile");
```
<sup><a href='/src/DiffEngine.Tests/DiffToolsTest.cs#L76-L83' title='File snippet `addtoolandlaunch` was extracted from'>snippet source</a> | <a href='#snippet-addtoolandlaunch' title='Navigate to start of snippet `addtoolandlaunch`'>anchor</a></sup>
<!-- endsnippet -->


## exePath

`exePath` is the path to the executable.

If the file cannot be found `AddTool*` will return null.


### Path conventions

 * [Environment.ExpandEnvironmentVariables](https://docs.microsoft.com/en-us/dotnet/api/system.environment.expandenvironmentvariables) is used to expand environment variables.
 * `*` can be used as a wildcard.
 * In the case where multiple matches are resolved the newest will be used.

So for example `%ProgramFiles(x86)%\Microsoft Visual Studio\*\*\Common7\IDE\devenv.exe` might discover the following:

 * `C:\Program Files (x86)\Microsoft Visual Studio\2019\Professional\Common7\IDE\devenv.exe`
 * `C:\Program Files (x86)\Microsoft Visual Studio\2017\Professional\Common7\IDE\devenv.exe`

Then `C:\Program Files (x86)\Microsoft Visual Studio\2019\Professional\Common7\IDE\devenv.exe` will be used.
