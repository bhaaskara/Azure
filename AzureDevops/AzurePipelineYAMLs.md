# Simple pipeline
```yml
pool: 
  vmImage: 'ubuntu-latest'

steps:
- bash: echo "our first pipele"
```

# Pipeline with Jobs
```
pool:
  vmImage: 'ubuntu-latest'

jobs:
- job: first job
  timeoutInMinutes: 10
  steps:
  - bash: echo "the first job"

- job: Second job
  steps:
  - bash: echo "the second job"
```

The specification of a pool can be done at multiple levels in a YAML file.
```yml
jobs:
- job: Linux
  pool:
    vmImage: 'ubuntu-latest'
  steps:
  - script: echo hello from Linux
- job: macOS
  pool:
    vmImage: 'macOS-latest'
  steps:
  - script: echo hello from macOS
- job: Windows
  pool:
    vmImage: 'windows-latest'
  steps:
  - script: echo hello from Windows
```
# Trigger
```
trigger:
- master

pool:
  vmImage: 'ubuntu-latest'

jobs:
- job: first job
  timeoutInMinutes: 10
  steps:
  - bash: echo "the first job"

- job: Second job
  steps:
  - bash: echo "the second job"