# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

# Changed

## [Unreleased]

## [1.1.0] - 2026-10-22
- Added support for compiling with the native (cross-platform) toolchain. Windows-only features (UI runner, `-c`/`-g`/`-o` options, TimeSpan array assertions) are excluded on cross-platform builds.
- Message call recording (`EnableCallRecording.pkg`, `CallRecording.pkg`) is now only included when `DFUNIT_ENABLE_MESSAGE_CALL_TESTING` is defined.
- Fixed command line argument parsing skipping the last argument and reading the program name as an argument.
- Added `pbConsoleWait` to `cDFUnitTestApplication` so the console waits for a key before a native console run exits. The console also waits when a debugger is attached.
- Added `cWebDfUnitTestApplication` that can be used under the WebAssembly runtime.
- Added `cDFUnitJunitReporter` that writes JUnit XML to the console or to a file. `cDFUnitXMLReporter` is now a subclass of it and is also available on cross-platform builds.
- Added `--junit` (JUnit XML to the console) and `--junit-file <file>` (console output plus JUnit XML to a file) command line arguments.
- Added named reporters to the reporter manager: `ReporterCount`, `AddReporterByName`, `ReporterByIndex`, `ReporterByName` and `RemoveReporterByName`. The default console reporter is only added when no reporter has been registered.
- Line-based reporters now print full fixture and test names (`Fixture.SubFixture.Test`).
- Added assertions: `AssertPAreEqual`, `AssertPNAreEqual`, `AssertHAreEqual`, `AssertHNAreEqual`, `AssertUIAreEqual`, `AssertUBIAreEqual`, `AssertDNAreEqual`, `AssertDNAreNotEqual`, `AssertRAreEqual`, `AssertRAreNotEqual`, `AssertTAreEqual`, `AssertTAreNotEqual` and `AssertTSAreLess`.
- A test now fails when an error registered with `ExpectError` did not occur.
- Fixed the JUnit/XML reporter carrying the `Fatal` marker over to subsequent errors of the same test.
- Fixed all subsequent tests failing after one test failed, because the current test's error count was never reset.
- Fixed casing of `Use` statements so packages resolve on case-sensitive file systems.
- Documented `pbConsoleWait`, `--junit` and `--junit-file` in the README.
