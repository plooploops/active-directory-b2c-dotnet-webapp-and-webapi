### This is for the docker multilayer build ###
### docker build -t myrepo.azurecr.io/aad-b2c-sample-task-service:windowsservercore-1709 .

# escape=`
FROM jsturtevant/4.7-windowsservercore-1709-builder:latest as build-agent
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# Build files
WORKDIR C:\src
COPY packages.config .
RUN nuget restore packages.config -PackagesDirectory ..\packages

COPY . C:\src
RUN msbuild TaskService.csproj /p:OutputPath=C:\out /p:DeployOnBuild=true 

## final image
FROM microsoft/aspnet:4.7.1-windowsservercore-1709
WORKDIR /inetpub/wwwroot
COPY --from=build-agent C:\out\_PublishedWebsites\TaskService .


### This should be used for regular docker calls with docker compose ###
#
#FROM microsoft/aspnet:4.7.1-windowsservercore-1709
#ARG source
#WORKDIR /inetpub/wwwroot
#COPY ${source:-obj/Docker/publish} .
#
