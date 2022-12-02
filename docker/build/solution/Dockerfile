# escape=`

# This Dockerfile will build the Sitecore solution and save the build artifacts
# for use in other images, such as 'cm' and 'rendering'. It does not produce a
# runnable image itself.

ARG BASE_IMAGE
ARG BUILD_IMAGE

FROM ${BUILD_IMAGE} AS nuget-prep
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Gather only artifacts necessary for NuGet restore, retaining directory structure
COPY *.sln NuGet.config Directory.Build.targets *.props /nuget/
COPY src/ /temp/
RUN Invoke-Expression 'robocopy C:/temp C:/nuget/src /s /ndl /njh /njs *.csproj Packages.props'

FROM ${BUILD_IMAGE} AS builder
ARG BUILD_CONFIGURATION
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Create an empty working directory
WORKDIR /build

# Copy prepped NuGet artifacts, and restore as distinct layer to take better advantage of caching
COPY --from=nuget-prep ./nuget ./

# Restore NuGet packages
RUN nuget restore -Verbosity quiet

# Copy remaining source code
COPY src/ ./src/

# Copy transforms, retaining directory structure
RUN Invoke-Expression 'robocopy C:\build\src C:\out\transforms /s /ndl /njh /njs *.xdt'

# Ensure deploy folders exist
RUN mkdir ./website;

# Build website with file publish
RUN msbuild .\src\Website\website\Headlixbase.Website.csproj /p:Configuration=$env:BUILD_CONFIGURATION /m /p:DeployOnBuild=true /p:PublishProfile=Docker /p:PublishUrl=C:\out\website

FROM ${BASE_IMAGE}
WORKDIR C:\artifacts

# Copy final build artifacts
COPY --from=builder C:\out\website .\website\
COPY --from=builder C:\out\transforms .\transforms\