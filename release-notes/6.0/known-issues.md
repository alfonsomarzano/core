# .NET 6.0 Known Issues

You may encounter the following known issues, which may include workarounds, mitigations or expected resolution timeframes.

## .NET SDK

1. WPF App may fail to build or publish

**Publish in Visual Studio**

When publishing a WPF project in Visual Studio with a target runtime specified, the publish operation may fail and display a message similar to the following in the output window:

> Assets file ‘c:\git\repro\WPFSelfContained\obj\project.assets.json’ doesn’t have a target for ‘net5.0-windows/win-x64’. Ensure that restore has run and that you have included ‘net5.0-windows’ in the TargetFrameworks for your project. You may also need to include ‘win-x64’ in your project’s RuntimeIdentifiers.

**Customized output paths**

When building a project that redirects the intermediate and/or output paths to a folder using the `MSBuildProjectName` MSBuild property, for example via the following in a `Directory.Build.props` file:

```xml
<PropertyGroup>
    <RepoRoot>$([System.IO.Path]::GetFullPath('$(MSBuildThisFileDirectory)'))</RepoRoot>
    <BaseOutputPath>$(RepoRoot)artifacts\bin\$(MSBuildProjectName)\</BaseOutputPath>
    <BaseIntermediateOutputPath>$(RepoRoot)artifacts\obj\$(MSBuildProjectName)\</BaseIntermediateOutputPath>
  </PropertyGroup>
```
An error similar to the following may be generated:

> error NETSDK1004: Assets file ‘c:\git\repro\wpf\artifacts\obj\wpf_gzmmtwnk_wpftmp\project.assets.json’ not found. Run a NuGet package restore to generate this file.

**Workaround**

You can workaround these issues by setting the IncludePackageReferencesDuringMarkupCompilation property to false in the project file:

`<PropertyGroup>
    <IncludePackageReferencesDuringMarkupCompilation>false</IncludePackageReferencesDuringMarkupCompilation>
</PropertyGroup>`

## .NET Runtime
1. Issue in `ReadyToRun` feature for .NET Preview 6.0 Preview 3 as described in [dotnet/runtime #50472](https://github.com/dotnet/runtime/issues/50472)

**Workaround**

You can workaround this issue by setting `COMPlus_ReadyToRun=0` environment variable.


## Windows Forms

* `PropertyGrid` values are rendered at incorrect location.

     The issue is tracked in [dotnet/winforms#4593](https://github.com/dotnet/winforms/issues/4593) and is expected to be fixed in 6.0 Preview3.
     
## ASP.NET Core

**Running Blazor WebAssembly using IIS Express in Development**

As of .NET 6 Preview 1, there is an ongoing issue with running Blazor WebAssembly applications using an IIS Express server during development on Visual Studio. As a workaround, we recommend using Kestrel during development.

**Incremental builds in VS not working for apps with Razor views**

As of .NET 6 Preview 3, changes to Razor views will not be updated during incremental builds. As a workaround, you can:

- Build from the command line
- Configure VS to always call MSBuild when building projects
- Clean and build projects to pick up changes
