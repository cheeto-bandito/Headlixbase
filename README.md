<img src="https://chetcheeto.files.wordpress.com/2022/02/headless.png" height="154px" width="200px" /><br />
<a href="https://www.flaticon.com/free-icons/headless" title="headless icons">Headless icons created by max.icons - Flaticon</a><br />
A Headless Sitecore Helix (Headlix) solution template which can be used for Greenfield projects. Tackles some common problems when working with the platform.

#### Features include:

* Docker Ready!
* Sitecore Content Serialization (SCS)
* Sitecore 10.2.0
* Version trimming rules engine - Items limited to 10 versions by default
* Search Templates computed index field - find all items from an index by any templates they implement
* Non admin Item Unlock
* Auto unlocks items when a user is deleted
* Integration with [helix-publishing-pipeline](https://github.com/richardszalay/helix-publishing-pipeline)
* Fast ([see benchmark](https://github.com/richardszalay/Helixbase-HPP/tree/benchmarks#benchmarks)) publish-on-build (when building inside Visual Studio)
* [_Show Title When Blank_](https://jammykam.wordpress.com/2017/09/20/show-title-when-blank/) patch, the forgotten Sitecore feature!
* Scaffolding helix modules with `dotnet new`
* COMING SOON: Support for custom `dotnet new` templates

## Getting Started
This code is open sourced in order to facilitate contributions from the community.  However, you do not need to clone this repository in order to take advantage of these `dotnet new` templates.

### Install the headlixbase Templates

1. Open Powershell with administrator privileges
2. Run the following command:

```
dotnet new -i Headlixbase.DevEx.Templates --nuget-source https://nuget.pkg.github.com/cheeto-bandito/headlixbase/index.json
```

### Scaffold a Helix solution

```
dotnet new headlixbase -n {YourSolutionName} -at {YourName} -cn {YourCompanyName}
```

### Scaffold a Helix module

```
dotnet new headlixbase.module -n {YourModuleName} -cn {YourCompanyName} -sn {YourSolutionName} -hl {HelixLayer[Feature, Foundation, Project]}
```

```
dotnet sln add -s "{HelixLayer}/{YourModuleName}" "src/{HelixLayer}/{YourModuleName}/website/{YourModuleName}.csproj"
```

### Initializing Docker

```
.\init.ps1 -LicenseXmlPath {PathToYourLicenseFile}
```

```
docker-compose build
```

```
docker-compose up -d
```


## Contributing

### Packaging the Templates

```
nuget pack ".\Headlixbase.DevEx.Templates.nuspec" -OutputDirectory "C:\nuget\LocalFeed" -NoDefaultExcludes
```

### Publishing the Package

https://docs.github.com/en/packages/learn-github-packages/publishing-a-package

```
nuget source add -Name "github" -Source "https://nuget.pkg.github.com/cheeto-bandito/index.json"
```

```
dotnet nuget push "Headlixbase.DevEx.Templates.1.0.x.nupkg" --api-key YOUR_GITHUB_PAT --source "github"
```




## Build

Headlix Base uses [helix-publishing-pipeline](https://github.com/richardszalay/helix-publishing-pipeline) and pre-configures a number of features.

* Content files from all modules are included in the publish
* Sitecore assemblies are excluded from publish, reducing the package filesize

Local publishing:

* Fast publish-on-build of the Local publish profile. This only adds a few seconds and won't recycle your app pool unless you change code. It even runs your debug Web.config transform!
* Old assemblies (Headlixbase.*.dll) are automatically removed

CI/CD publishing:

* Serialization files are automatically included into App_Data\serialization using the 'package' publish profile.


## Extensions
COMING SOON: A nuget feed for Headlixbase modules