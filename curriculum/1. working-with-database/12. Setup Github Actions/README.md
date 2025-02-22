### Workflow

- Is an automated procedule
- Made up of 1+ jobs
- Triggered by events, schedules, or manually
- Add .yml file to repository

```yaml
push:
  branches: [master]
schedule:
  - cron: "*/15 * * * *"
```

### Runner

- Is a server to run the jobs
- Run 1 job at a time
- github hosted or self hosted
- report progress, logs & result to github

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
```

### Job

- Is a set of steps execute on the same runner
- Normal jobs run in parallel
- Dependent jobs run serially

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checout@v2
      - name: Build server
        run: ./build_server.sh
  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - run: ./test_server.sh
```

### Step

- Is an individual task
- Run serially within a job
- Contain 1+ actions

### Action

- Is a standalone command
- Run serially within a step
- Can be reused

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Check out code
        uses: actions/checkout@v2

      - name: build server
        run: ./build_server.sh
```

### Set it up

1. Go to github repo for this project
2. Go to github actions
3. Configure go workflow for this project

```yaml
# This workflow will build a golang project
# For more information see: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-go

name: Go

on:
  push:
    branches: ["main"]
  pull_request:
    branches: ["main"]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Set up Go
        uses: actions/setup-go@v4
        with:
          go-version: "1.20"

      - name: Build
        run: go build -v ./...

      - name: Test
        run: go test -v ./...
```
