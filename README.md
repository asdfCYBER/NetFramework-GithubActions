# Github action for building an application based on .NET Framework.

This action uses [microsoft/setup-msbuild@v1.0.2](https://github.com/microsoft/setup-msbuild) and [NuGet/setup-nuget@v1.0.5](https://github.com/NuGet/setup-nuget), and uploads all files created by the build process.
To make this work for your own solution, replace all occurences of 'NetFramework-GithubActions.sln' with your own .sln file. You can find the action file at <code>.github/workflows/netframeworkbuild.yaml</code>, the rest of this repo is just something to test the action on.

Action file contents:

```yaml
name: .NET Framework Build

on: [push]

jobs:
  build:

    runs-on: windows-latest

    steps:
    - uses: actions/checkout@v2
      name: Checkout source

    - name: Setup MSBuild path
      uses: microsoft/setup-msbuild@v1.0.2

    - name: Setup NuGet
      uses: NuGet/setup-nuget@v1.0.5

    - name: Get NuGet packages
      run: nuget restore NetFramework-GithubActions.sln

    - name: Build .exe
      run: msbuild NetFramework-GithubActions.sln /p:Configuration=Release

    - name: Upload Artifacts
      uses: actions/upload-artifact@v2
      with:
        name: Build files
        path: bin\Release\
```
