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
* COMING SOON: Scaffolding helix modules with `dotnet new`
* COMING SOON: Support for custom `dotnet new` templates

## (TODO) Setup Instructions
1. Install Visual Studio 2022
2. Clone this repo
3. Install the headlixbase template
4. dotnet new headlixbase -n {YourSolutionName} -at {YourName} -cn {YourCompanyName}
5. cd {YourSolutionName}
6. .\init.ps1
7. docker-compose build
8. docker-compose up -d


#### (TODO) Using Headlixbase:



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