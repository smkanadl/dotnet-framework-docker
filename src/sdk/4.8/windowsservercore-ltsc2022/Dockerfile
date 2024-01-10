# escape=`

# https://github.com/microsoft/dotnet-framework-docker/blob/main/src/runtime/4.8/windowsservercore-ltsc2022/Dockerfile
# https://github.com/microsoft/dotnet-framework-docker/blob/main/src/sdk/4.8/windowsservercore-ltsc2022/Dockerfile

# Instead of installing VS build tools, we install latest donet sdk and use this to run net48 unit tests
# We also do not use the optimized runtime image as base as this seems to be part of causing additional startup time

FROM mcr.microsoft.com/windows/servercore:ltsc2022-amd64

LABEL org.opencontainers.image.source="https://github.com/smkanadl/dotnet-framework-docker"

ENV `
    # .NET Runtime version
    DOTNET_VERSION=8.0.1 `
    # Do not generate certificate
    DOTNET_GENERATE_ASPNET_CERTIFICATE=false `
    # Do not show first run text
    DOTNET_NOLOGO=true `
    # SDK version
    DOTNET_SDK_VERSION=8.0.101 `
    # Enable correct mode for dotnet watch (only mode supported in a container)
    DOTNET_USE_POLLING_FILE_WATCHER=true `
    # Skip extraction of XML docs - generally not useful within an image/container - helps performance
    NUGET_XMLDOC_MODE=skip

RUN `
    powershell -Command `
        $ErrorActionPreference = 'Stop'; `
        $ProgressPreference = 'SilentlyContinue'; `
        Invoke-WebRequest `
            -UseBasicParsing `
            -Uri https://dot.net/v1/dotnet-install.ps1 `
            -OutFile dotnet-install.ps1; `
        ./dotnet-install.ps1 `
            -InstallDir '\Program Files\dotnet' `
            -Architecture x64 `
            -Channel 8.0.1xx; `
        Remove-Item -Force dotnet-install.ps1 `
    && powershell -Command `
        $ErrorActionPreference = 'Stop'; `
        $ProgressPreference = 'SilentlyContinue'; `
        Invoke-WebRequest `
            -UseBasicParsing `
            -Uri https://dot.net/v1/dotnet-install.ps1 `
            -OutFile dotnet-install.ps1; `
        ./dotnet-install.ps1 `
            -InstallDir '\Program Files (x86)\dotnet' `
            -Architecture x86 `
            -Channel 8.0.1xx; `
        Remove-Item -Force dotnet-install.ps1

# In order to set system PATH, ContainerAdministrator must be used
USER ContainerAdministrator
RUN setx /M PATH "%PATH%;C:\Program Files\dotnet;C:\Program Files (x86)\dotnet"
USER ContainerUser
