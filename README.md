# DFUnit - DataFlex Unit Testing Framework

**DFUnit** is a renewed DataFlex testing framework. It was originally forked from [https://github.com/olaeld/DFUnit](https://github.com/olaeld/DFUnit). Data Access originally forked it to accommodate the framework for multiple reporters like seen in frameworks like doctest. Most importantly support for build-servers was added using a new Console reporter and exit codes. It supports outputting to one of the most used test-data formats **JUnit** which will help for a steady CI flow. The framework is and will stay free.

### Getting Started

The repository is self-sustaining aside from the DataFlex APIs. To use this renewed version you will need DataFlex 20.0+. By simply opening the Sample workspace and compiling it you should be able to automatically see the output of the unit tests.

Now let's walk through the essentials:

#### Project setup

Should you wish to create a new project that is build-server compatible perform the following actions:

1. Disable the following project options in the studio<sup>1:</sup>
    - Build Manifest
    - Embed Manifest
2. Disable suffixes on 64-bit to make it easier for the compiler; **for the current Jenkinsfile example**.

<sup>1</sup>Is needed as studio settings might interfere with the console compiler's settings from the workspace.

#### Project source

Everything within DFUnit is single-header based, which means that you only ever need to include DFUnit.pkg. From then on it works the same as working with a Windows or Web Application.

1. Create a cApplication instance which will cover ghoApplication and; 
    - Opens the workspace.
    - Parses the command line.
2. Create a cDFUnitTestApplication instance which is our root parent. 
    1. You can Include files within the application to nest the individual fixtures and tests.
    2. You can embed a cTestFixture.
    3. You can embed a cTest.
    4. You can declare published procedures like below.

```DataFlex
Use DFUnit.pkg

Object oApplication is a cApplication
End_Object

Object oTestApplication is a cDFUnitTestApplication
	... Use {file}
    ... Create a fixture
    ... Create a test
    ... Create a published procedure as test
    { Published=True }
    Procedure TrueIsTrue
        Send Assert True "True should be True."
    End_Procedure
End_Object
```

#### Using fixtures

Fixtures have different setup and teardown events. These events are all called before a test is executed. Know that they are recursive; as you start nesting fixtures executing a test will recursively call Setup upward and TearDown the same way afterwards.

In-between there are some events which will give you more flexibility which are also called in the following order.;

- BeforeSetupOneTime
- SetupOneTime
- AfterSetupOneTime
- BeforeSetup
- Setup
- AfterSetup
- BeforeTearDown
- TearDown
- AfterTearDown
- BeforeTearDownOneTime
- TearDownOneTime
- AfterTearDownOneTime

The OneTime events are called before and after each fixture itself but not the individual tests.

#### Using tests

All tests should use the assertion functions which are defined for each type.

```DataFlex
Procedure Assert Boolean bCondition String sAssertMessage
Procedure AssertFalse Boolean bCondition String sAssertMessage
Procedure AssertIAreEqual Integer Expected Integer Actual String sAssertMessage
Procedure AssertNAreEqual Number Expected Number Actual String sAssertMessage
Procedure AssertSAreEqual String Expected String Actual String sAssertMessage
Procedure AssertDTAreEqual DateTime Expected DateTime Actual String sAssertMessage
...
```

#### Using tests with Errors

DataFlex works with error numbers and as such we are able to expect them in block-like statements like the following.

```DataFlex
{ Published=True }
Procedure ExpectingAnError
    Send ExpectError DFERR_PROGRAM
        Error DFERR_PROGRAM "An expected error."
    Send UnExpectError DFERR_PROGRAM
End_Procedure
```

### Application options

`psTestFixtureName`, Defines the name of the root fixture (The TestApplication).

`pbAutoRun`, **\[Advanced\]** Should you have custom Run() code that invokes the Test Application you can turn this to false.

`pbAutoRunTests`, If using the UI this will immediately start the unit tests instead of waiting for a button click.

`pbUseUIIfInDebugger`, Should `pbUseUI` be false this could override that property if using the debugger.

`pbUseUI`, Indicates whether the UI or Console should run by default.

### Console options

`--help (-h)`, prints the DFUnit framework information and the options.

`--console (-c)`, runs the tests without the UI (overrides `pbUseUI` to False). The program exits with 0 when
every test passed, and -1 when a test failed or had an unexpected error.

`--gui (-g)`, overrides `pbUseUI` to True.

`--no-autorun (-n)`, overrides `pbAutoRunTests` to False.

`--output (-o) <file>`, also writes the results to the file as **JUnit XML**, the format build servers read
(Jenkins, GitLab, Azure DevOps, the GitHub test-report actions): one `<testsuite>` per fixture, named by its
path, with `tests`, `failures`, `errors` and `time`, and a `<testcase>` with `classname` and `name` per test. A
test with failed assertions counts as one failure, a test with an unexpected error as one error.

`--filter (-f) <text>`, runs only the tests whose full name contains `<text>` (case does not matter, and `_`
matches a space, so a procedure name works too). Several texts are separated by `;`, or the option is given
again. A fixture with no selected test is skipped completely, its one-time setup included. A filter that
selects nothing ends the run with -1, so a typo in a build script never gives a green build.

`--list (-l)`, prints the full names of the tests - or of the tests that `--filter` selects - in the order they
run, and exits without running them. A full name is the path of fixtures and the test, for example
`Leap_Year_Spec / A_year_is_a_leap_year / If it is divisible by 400`; it is what `--filter` matches.

```
RunAllTests.exe -c -o test_results.xml          all tests, JUnit XML for the build server
RunAllTests.exe -c -f "leap_year;divisible"     only the tests with either text in the name
RunAllTests.exe -l -f leap_year                 list them first
```
