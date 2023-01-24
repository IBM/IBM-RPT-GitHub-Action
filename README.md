# Rational Performance Tester GitHub Action
Rational Test provides software testing tools to support a DevOps approach: API testing, functional testing, UI testing, performance testing and service virtualization. It helps you automate and run tests earlier and more frequently to discover errors sooner - when they are less costly to fix.

This action enables the ability to run performance tests and schedules from Rational Performance Tester projects on self hosted runners.

## Usage

### Prerequisites

Configure a self hosted runner on a suitable machine
1. The host machine will need a licensed copy of Rational Performance Tester and the Performance projects containing the tests you wish to run.
2. [Add a self-hosted runner](https://docs.github.com/en/actions/hosting-your-own-runners/adding-self-hosted-runners) at an appropriate level, ensure you have configured network access.
3. If you are using more than one runner [assign it a suitable label](https://docs.github.com/en/actions/hosting-your-own-runners/using-labels-with-self-hosted-runners) and note this for later.

### Setup
In the repository you want to apply the action to
1. Create a folder named ".github/workflows" in the root.
2. Create a .yml file with any name, inside the ".github/workflows" folder based on the following example content:


```yaml
name: Rational Performance Tester

on: workflow_dispatch

jobs:
    Performance-Action:
        name: Rational Performance Tester
        runs-on: self-hosted
        steps:
         - name: Rational Performance Tester
           uses: IBM/devopsauto-perftest-actions@main
           with:
            workspace: <C:\Data\SampleWorkspace>
            project: <MyProject>
            suite: <MySuite>
```

3. Update the parameterized items to refer to your project and tests (see parameter details below).
4. If you have more than one runner configured, then use its label as the argument for **runs-on**.
5. Push your updated yml file to the repository.
6. Go to the Actions section in the repository and select the workflow.
7. Click the Run workflow dropdown and the list of input boxes get displayed.

As an alternative to having all of the test projects pre-configured on the runner machine, users could leverage other actions within the workflow to pull down the latest copies. 

### Required Parameters

- **workspace** Complete path to Eclipse workspace, required if configFile is not specified.
- **project** The name of the project containing the test, required if configFile is not specified.
- **suite** The relative path from the project to the test including the file name of the test. A test can be a Performance test, Schedule, or Compound test and required if configFile is not specified.

### Optional Parameters

- **imShared** Complete path to IBMIMShared location, if it is not at default location.
- **configFile** The complete path to a file that contains the parameters for a test or schedule run. If Config file is specified then no other fields will be required.
- **duration** You can use this option to specify the duration of the stages in the Rate Schedule.
- **exportLog** You can use this option to specify the file directory path to store the exported HTTP test log. You can provide multiple parameter entries when running multiple tests. You must use a colon to separate the parameter entries. For example: c:/logexport.txt:c:/secondlogexport.txt
- **exportStatReportList** You can use this option to specify a comma-separated list of report IDs along with exportstats or exportstatshtml to list the reports that you want to export in place of the default reports, or the reports selected under Preferences. To retrieve the report IDs, navigate to Window > Preferences > Test > Performance Test Reports > Export Reports from Rational Performance Tester and under Select reports to export, select the required reports, and click Copy ID to clipboard.
- **exportStats** Use this option to provide the complete path to a directory that you can use to store the exported report in a comma-separated values (CSV) format.
- **exportStatsFormat** Use this option to enter one or more formats for the reports that you want to export by using a comma as a separator. The options are simple.csv, full.csv, simple.json, full.json, csv, and json. When you want to export both simple and full reports in json or csv format, you can specify json or csv as the options. The reports are saved to the location specified in the exportStats field. This field must be used in conjunction with exportStats field.
- **exportStatsHtml** Use this option to provide the complete path to a directory that you can use to export web analytic results. You can analyze the results on a web browser without using Rational Performance Tester.
- **labels** Use this option to add labels to test results. To add multiple labels to a test result, you must separate each label by using a comma.
- **overrideRmLabels** Use this option to enable the Resource Monitoring from Service option for a performance schedule if the Resource Monitoring from Service option is not enabled from the schedule editor in Rational Performance Tester, ignore Resource Monitoring sources that were set in the performance schedule and to change for a label matching mode, replace an existing set of Resource Monitoring labels that were set in the performance schedule and run the schedule with a new set of Resource Monitoring labels.
- **overwrite** Determines whether a result file with the same name is overwritten. The default value, false, indicates that the new result file is created. If the value is true, the file is overwritten and retains the same file name.
- **publish** You can use this option to publish test results to the Server. The format is: serverURL#project.name=projectName&teamspace.name=teamspaceName. If the name of the project or team space contains a special character, then you must replace it with %<Hex_value_of_special_character>.
- **publishFor** You can use this option to publish the test results based on the completion status of the tests, supported values are FAIL,PASS,INCONCLUSIVE,ERROR,ALL.
- **publishReports** You can use this option to specify the reports to be published to Rational Test Automation Server. The supported values are STATS,TESTLOG.
- **rate** Use this option to specify a rate that you want to achieve for a workload in the Rate Runner group. For example, "Rate Runner Group 1=1/s, 3/m", where, Rate Runner Group1 is the name of the rate runner group that has two stages. The desired rate for the first stage is one iteration per second and the rate for the second stage is three iterations per minute.
- **results** Use this option to specify the name of the results file. The default name of the result file is the test or schedule name with a timestamp appended. You must specify a folder name that is relative to the project to store the test results.
- **reportHistory** Use this option when you want to view a record of all events that occurred during a test or schedule run. Supported values are jaeger, testlog, null.
- **swapDatasets** Use this option to replace dataset values during a test or schedule run. You must ensure that both original and new datasets are in the same workspace and have the same column names. You must also include the path to the dataset. For example, /project_name/ds_path/ds_filename.csv:/project_name/ds_path/new_ds_filename.csv
- **userComments** Use this option to add text that you want to display in the user comments row of the report.
- **users** Overrides the default number of virtual users in the run. For a schedule, the default is the number of users specified in the schedule editor. For a test, the default is one user.
- **varFile** Use this option to specify the complete path to the XML file that contains the variable initialization.
- **vmArgs** Use this option to specify the Java maximum heap size for the Java process that controls the playback. For example, when you input the value as -Xmx4096m, it specifies the maximum heap size as 4GB.

## Troubleshooting
- **No runner available:** Check that the runner still exists in GitHub, if the local agent has not connected to GitHub for a period of 14 days then it will be automatically removed.