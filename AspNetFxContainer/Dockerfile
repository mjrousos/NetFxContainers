# Build stage
# Use .NET Framework SDK as the base build image
FROM mcr.microsoft.com/dotnet/framework/sdk:4.8-windowsservercore-ltsc2022 AS build

WORKDIR /src
COPY ["AspNetFxContainer.sln", "AspNetFxContainer.sln"]
COPY ["./WebFormsApp/*.csproj", "./WebFormsApp/"]
COPY ["./WebFormsApp/*.config", "./WebFormsApp/"]
RUN msbuild /t:restore
COPY . .
RUN msbuild /p:DeployOnBuild=true /p:PublishProfile=FolderProfile

FROM mcr.microsoft.com/dotnet/framework/aspnet:4.8-windowsservercore-ltsc2019
WORKDIR /inetpub/wwwroot
COPY --from=build /src/WebFormsApp/bin/app.publish .
