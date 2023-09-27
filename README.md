# BMC GitLab Remote Templates

In GitLab using the `include` keyword allows the inclusion of external YAML files. This helps to break down the CI/CD configuration into multiple files and increases readability for long configuration files. It’s also possible to have template files stored in a central repository and projects include their configuration files. This helps avoid duplicated configuration, for example, global default variables for all projects.

`include` requires the external YAML file to have the extensions .yml or .yaml, otherwise the external file won’t be included.

## BMC Remote Templates

The templates in this repository can be used as `remote` template in the GitLab CI yaml.

### How to Use the BMC remote template?

It is very simple to use these templates in your CI yaml. It can be done using `include:remote` in the GitLab CI YAML.
`include:remote` can be used to include a file from a different location, using HTTP/HTTPS, referenced by using the full URL. The remote file must be publicly accessible through a simple GET request as authentication schemas in the remote URL are not supported. 
For example including strobemeasurement using remote template:
```yaml
include:
  - remote: 'https://raw.githubusercontent.com/bmcsoftware/bmc-gitlab-remote-templates/master/templates/job/strobemeasurement.gitlab-ci.yml'
```

We can provide multiple remotes in list format to add more number of remote templates.

### A Sample GitLab CI YAML using strobemeasurement template 

_gitlab-ci.yml_
```yaml
---
include:
  remote: 'https://www.github.com/bmcsoftware/master/templates/strobemeasurement.gitlab-ci.yml'

stages:
  - "strobe"

strobemeasurementjob:
  stage: strobe
  extends: .strobemeasurement
  tags:
    - windowstest
  variables :
    jobstage: strobe
    selectedHostName: 'cw09'
    requestType: 'addQueue'
    system: 'cw09'
    jobName: 'PINXXX'
    tags: 'test'
    profile: 'TEST'
    email: 'youremail'
    duration: '5'
    limit: 'nolimit'
    finalAction: 'stop'
    method: 'GET'
    url: 'https://url'
    returnURL: 'https://returnURL'

```

extends keyword used to inherit jobs defined in template file.Pass stage (eg. build,test etc) in jobstage variable to the Job.


