# Rules Multirun Windows Issue

This repository demonstrates an issue with running format commands using `rules_lint` and `rules_multirun` on Windows.

## Issue Description

When attempting to run format commands on Windows using:
```bash
bazel run //tools:format
```

The following error occurs:
```
No suitable shell toolchain found:
* if you are running Bazel on Windows, set the BAZEL_SH environment variable to the path of bash.exe
* if you are running Bazel on a non-Windows platform but are targeting Windows, register an sh_toolchain for the //shell:toolchain_type toolchain type
```

## Root Cause

The issue stems from the interaction between:
1. `rules_lint`'s format rules which use `rules_shell`
2. `rules_multirun` which is used to run multiple targets
3. Windows-specific shell handling requirements

The error occurs because the Windows environment is missing the required shell toolchain configuration that `rules_shell` needs to execute shell commands.

## Project Structure

- `tools/BUILD.bazel`: Contains the format target definition using `format_multirun`
- Uses `rules_lint` for formatting
- Uses `buildifier` for Starlark file formatting

## Reproduction Steps

1. Clone this repository
2. Run `bazel run //tools:format`
3. Observe the shell toolchain error

## Environment

- Windows 11
- Bazel 8.2.1
- Using `rules_lint` and `rules_multirun`

## Potential Solutions

1. Set the `BAZEL_SH` environment variable to point to a valid `bash.exe` installation
2. Configure a proper shell toolchain for Windows in the Bazel configuration
3. Modify the rules to better handle Windows environments

## Related Issues

This issue is related to:
- Windows compatibility in Bazel rules
- Shell toolchain configuration
- Cross-platform build tool support
