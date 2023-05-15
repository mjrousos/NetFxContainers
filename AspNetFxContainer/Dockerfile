# Build stage
# Use .NET Framework SDK as the base build image
FROM mcr.microsoft.com/dotnet/framework/sdk:4.8-windowsservercore-ltsc2022 AS build

WORKDIR /src
COPY ["AspNetFxContainer.sln", "AspNetFxContainer.sln"]
COPY ["./WebFormsApp/*.csproj", "./WebFormsApp/"]
COPY ["./WebFormsApp/*.config", "./WebFormsApp/"]
RUN msbuild /t:restore /p:RestorePackagesConfig=true AspNetFxContainer.sln
COPY . .
RUN msbuild /p:DeployOnBuild=true /p:PublishProfile=FolderProfile

FROM mcr.microsoft.com/dotnet/framework/aspnet:4.8-windowsservercore-ltsc2022

## Enable URL rewrite and Applicaiton Request Routing modules
## requestRouter can be downloaded from https://www.iis.net/downloads/microsoft/application-request-routing
## rewrite can be downloaded from https://iis-umbraco.azurewebsites.net/downloads/microsoft/url-rewrite
#RUN     mkdir /temp
#COPY    requestRouter_amd64.msi /temp/
#COPY    rewrite_amd64.msi /temp/
#
#RUN     msiexec.exe /i "C:\temp\rewrite_amd64.msi" /qn
#RUN     msiexec.exe /i "C:\temp\requestRouter_amd64.msi" /qn
#RUN     rmdir /Q \temp
#RUN     powershell -NoProfile -Command \
#        Set-WebConfigurationProperty -PSPath 'MACHINE/WEBROOT/APPHOST' -Name 'enabled' -Filter 'system.webServer/proxy' -Value 'True

WORKDIR /inetpub/wwwroot
COPY --from=build /src/WebFormsApp/bin/app.publish .
