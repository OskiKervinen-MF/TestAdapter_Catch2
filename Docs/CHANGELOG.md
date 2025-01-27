# Change Log

Changes are relative to v1.0.0

## Changes for v1.8.0

This version introduces support for Catch2 v3, which has a small change in xml-reporting format and has improved support for test discovery.

### New Features

- Add support for Catch2 v3 (issue #52)

### Extended Features

- Enable support for environmental variables when running in debug mode.

## Changes for v1.7.0

### New Features

- Add support for Visual Studio 2022.
- Add support for environmental variables (inspired by issue #37). Environmental variables to be set for the Catch2 executable process can now be configured with the `<Environment>` option (see [Settings documentation](Settings.md#environment) for details).

### Bug fixes

- Issue #46: A problem deleting temporary files, caused the test adapter to fail.

## Changes for v1.6.0

This version introduces some major changes in the way the backend works. Where before for each test case an instance of the test executable was started (still the default behavior), now it is possible to run multiple test cases in a single instance of the test executable, improving test execution time significantly. Related to this there is also a mechanism introduced to force a test case to be run in its own test executable instance, when using the new way of executing test cases.

Furthermore, the test results are no longer written to memory, but to a temporary file before being processed. Though needed to make the above change possible, the positive side effect is that the test explorer now no longer has problems with tests that write information to the terminal in a way that is not intercepted by Catch2. In addition, the backend is better able to handle partial xml test result output, which typically occurs when the test executable instance needs to be killed due to a timeout, or when the test executable is prematurely terminated.

## Changes for v1.5.1

This version fixes a problem installing the Test Adapter for Catch2 on VS2019 RC.

## Changes for v1.5.0

This version contains significant improvements in testcase discovery. No longer do you need a custom discovery mechanism to enable source links to the testcase. Also, the discovery mechanism has been made more robust.

### Extended Features

- Issue #18: Added support for verbose default discovery. This enables creating a source file link to the position of a testcase for easy navigation via the Test Explorer, by using default discovery. This removes the need to add a custom Xml-based discovery mechanism to enable this testcase navigation feature in the Test Explorer.

### Changes to defaults

- The default value for `<DiscoverCommandLine>` has been set to `--verbosity high --list-tests *`, this enables creating a source file link to the position of a testcase for easy navigation via the Test Explorer.

### Bug fixes

- Issue #17: Testcases and tags with long names are not detected correctly using default discovery settings. Mostly fixed, there are a few corner cases that cannot be fixed.

### Documentation

- Updated the [walkthrough](Walkthrough.md) to reflect the availability of verbose default discovery.
- Added a page on the [discovery mechanism](Discovery.md). This includes an updated and improved custom discovery example.

## Changes for v1.4.2

This version adds support for VS2019. It also updates the Microsoft.TestPlatform.ObjectModel to version 15.9.0.

### Bug fixes

- Bug: When tests are run via "Debug Selected Tests" the working directory is set incorrectly. Fixed.

## Changes for v1.4.0

### Extended Features

- Improve Additional info output with respect to section. Is now more like standard Catch2 output.
- Improve how is dealt with problematic test case names. Tests with such names are now marked as skipped, with an error message that suggests a possible solution.  

### Bug fixes

- Bug: Test durations in Test Explorer are wrong for systems setup with a decimal symbol that is not '.'. Fixed.

## Changes for v1.3.0

### Extended Features

- Add option `<StackTracePointReplacement>` to allow replacement of decimal points in StackTrace description with a custom string. This is related to a bug fix, where decimal points in the StackTrace description interferes with the displayed StackTrace link.

- Handle test cases with names differing only in case more gracefully (_e.g._, "TestName", "TESTNAME", and "testname"). Test cases with names that only differ by case are always run together. Now the correct test result and failures are shown for those tests. Shown assertion statistics are of all tests that were run, and a note is added to the test result message to indicate this.

### Bug fixes

- Bug: StackTrace link does not display correctly when decimal points are part of the description (_e.g._, when displaying floating point numbers). Fixed.
- Bug: Test cases with names ending in '\\' generate invalid output. Fixed.

## Changes for v1.2.0

This version contains an important fix that enables stack trace links to source code. This is a significant usability improvement. Please update to this version and stop using the older ones. Documentation has been adapted to assume you are using this version.

### New Features

- Added `<MessageFormat>` option to configure what information is shown in the message part of the test case detail view. The default is to only show the assertion statistics for the test. It is also possible to choose to show additional info in the message, or to not have a message at all.

### Extended Features

- Improve error handling, specifically when a discovery timeout occurs it is now logged as a warning at logging level normal.
- Enable stack trace links in the Test Explorer detail view. With the help of Microsoft I was able to figure out the correct string format to use for the Stacktrace info in order to turn it into a source link. As a result the `<StackTraceFormat>` option was also altered and now has the options `None`and `ShortInfo`, the latter being the default or fall-back value.

### Changes to defaults

- The default value for `<DiscoverCommandLine>` has been set to `--list-tests *`, as having two settings with invalid defaults is not really useful.
- The default value for `<DiscoverTimeout>` has been set to 1000 ms. There were situations where 500 ms turned out to be too short, doubling that hopefully will give less problems.

### Bug fixes

- Bug: CHECK_FALSE and REQUIRE_FALSE failed output expansion shows '!' in front of value results. _E.g._, "CHECK_FALSE( !true )", should be "CHECK_FALSE( true )". Fixed.

## Changes for v1.1.0

### New Features

- Added `disabled` attribute to `<Catch2Adapter>` node for use in the _.runsettings_ file.
- Added `<DebugBreak>` setting that turns on or off Catch2's break on test failure feature (`--break`), when running tests via `Debug Selected Tests`.

### Extended Features

 - Added `Debug` level option to `<Logging>` setting and updated what is logged in the `Verbose` level.
 - Improve error handling, specifically when a duplicate test name is used, the potential resulting error is logged.
 - Add support for reporting Fatal Error Conditions

 ### Bug fixes

 - Bug: Warnings in Sections are not displayed when there are no failures. Fixed.
 - Bug: Get invalid test runner output error when Catch2 xml output produces additional text after xml report. Additional text is now ignored.
