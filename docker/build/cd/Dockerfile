# escape=`

ARG BASE_IMAGE
ARG SXA_IMAGE
ARG TOOLING_IMAGE
ARG SOLUTION_IMAGE
ARG HEADLESS_SERVICES_IMAGE

FROM ${HEADLESS_SERVICES_IMAGE} as headless_services_image
FROM ${SOLUTION_IMAGE} as solution
FROM ${TOOLING_IMAGE} as tooling
FROM ${SXA_IMAGE} as sxa
FROM ${BASE_IMAGE}

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Copy development tools and entrypoint
COPY --from=tooling \tools\ \tools\

WORKDIR C:\inetpub\wwwroot

# Add SXA module
COPY --from=sxa \module\cd\content .\
COPY --from=sxa \module\tools \module\tools
RUN C:\module\tools\Initialize-Content.ps1 -TargetPath .\; `
    Remove-Item -Path C:\module -Recurse -Force;
	
# Add HEADLESS module
COPY --from=headless_services_image C:\module\cd\content C:\inetpub\wwwroot
COPY --from=headless_services_image C:\module\tools C:\module\tools
RUN C:\module\tools\Initialize-Content.ps1 -TargetPath C:\inetpub\wwwroot; `
  Remove-Item -Path C:\module -Recurse -Force;

# Copy solution website files
COPY --from=solution \artifacts\website\ .\

# Copy solution transforms
COPY --from=solution \artifacts\transforms\ \transforms\solution\

# Perform solution transforms
# RUN C:\tools\scripts\Invoke-XdtTransform.ps1 -Path .\ -XdtPath C:\transforms\solution\Headlixbase.Website