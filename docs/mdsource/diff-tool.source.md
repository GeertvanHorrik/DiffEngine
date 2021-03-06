# Diff Tools


## Initial difference behavior

Behavior when an input is verified for the first time.

Behavior depends on if an [EmptyFiles](https://github.com/SimonCropp/EmptyFiles) can be found matching the current extension.

 * If an EmptyFiles can be found matching the current extension, then the tool will be launch to compare the input to that empty file.
 * If no EmptyFiles can be found no tool will be launched.


## Detected difference behavior

Behavior when a difference is detected between the input an existing current verified file.


### Not Running

If no tool is running for the comparison of the current verification (per test), a new tool instance will be launched.


### Is Running

If a tool is running for the comparison of the current verification (per test), and a new verification fails, the following logic will be applied:

| Auto Refresh | Mdi   | Behavior |
|--------------|-------|----------|
| true         | true  | No action. Current instance will refresh |
| true         | false | No action. Current instance will refresh |
| false        | true  | Open new instance. Previous instance must be manually closed |
| false        | false | Kill current and open new instance |

include: diffToolCleanup


## MaxInstancesToLaunch

By default a maximum of 5 tool instances will be launched. This prevents a change that break many test from causing too much load on a machine.

This value can be changed:

snippet: MaxInstancesToLaunch


## Successful verification behavior

If a tool is running for the comparison of the current verification (per test), and a new verification passes, the following logic will be applied:

| Mdi   | Behavior |
|-------|----------|
| true  | No action taken. Previous instance must be manually closed |
| false | Kill current instance |

include: diffToolCleanup


## Supported Tools:

include: diffTools
