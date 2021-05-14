# sonarscanner-buildkite-plugin

SonarQube scanner plugin for buildkite using their publically available Dockerhub CLI Image. The plugin will automatically detect if the running build is a Github PR, Feature branch or your default branch (main/master) and report relevant findings back to your configured host.

For additional information around how to utilize/configure sonarscanner refer to [sonarscanner](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/)

## Getting Started

There are two environment variables that have to be set in order for the plugin to function. These variables are listed below:

- `SONARSCANNER_HOST` - The place where the sonarscan results should be sent
- `SONARSCANNER_LOGIN` - This is the Token used to authenticate back to the sonar host

You have two options on how to configure these variables:

1. Pipeline configuration - See [environment-variables](https://buildkite.com/docs/pipelines/environment-variables) for additional information
2. AWS Parameter Store (assume buildkite agent has enough permissions for the following paths: `/buildkite/SONARSCANNER_HOST` and `/buildkite/SONARSCANNER_LOGIN`)

Once this is done, then you are able to use the plugin.

### Example: Basic

The below example can be used to run the sonarscanner plugin whenever your pipeline is triggered. If you want to skip specific branches etc, then use buildkite agnostic syntax to not run the plugin.

```yaml
steps:
  - label: ":sonarqube: Running sonarscanner"
    plugins:
      - jack1902/sonarscanner#v0.0.1:
          project_key: "PLACEHOLDER"
    agents:
      queue: default
```

### Example: Additional Flags

```yaml
steps:
  - label: ":sonarqube: Running sonarscanner"
    plugins:
      - jack1902/sonarscanner#v0.0.1:
          project_key: "PLACEHOLDER"
          additional_flags:
            - "-Dsonar.coverage.jacoco.xmlReportPaths=coverage.xml"
            - "-Dsonar.tests='app/tests'"
    agents:
      queue: default
```

## Configuration

| Option           | Required |      Type      | Default | Description                                                                                                                                                                   |
| ---------------- | :------: | :------------: | :-----: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| project_key      |   Yes    |    `string`    |         | The project key used inside of sonarqube                                                                                                                                      |
| additional_flags |    No    | `list(string)` |         | Additional flags to pass directly to the sonarscan, see the documentation for an extensive list [sonarscanner](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/) |
| debug            |    No    |    `boolean`    |         | Run the plugin in debug mode, useful for validating given flags that are being passed to the docker container                                                                 |
| artifacts        |    No    | `list(string)` |         | use `buildkite-agent artifact download` for each of the given paths. Helpful when wanting test-coverage output within SonarQube                                               |

## Contributing

Feel free to open `issues` and open Pull Requests to fix any bugs or extend the feature set of this plugin for your `terraform` use-case.

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/jack1902/tf-plan-apply-buildkite-plugin/tags).

## Authors

- **Jack** - *Initial work* - [jack1902](https://github.com/jack1902)

See also the list of [contributors](https://github.com/jack1902/sonarscanner-buildkite-plugin/contributors) who participated in this project.

## License

This project is licensed under the Mozilla Public License Version 2.0 - see the [LICENSE](LICENSE) file for details
