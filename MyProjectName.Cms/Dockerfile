FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8080

ENV ASPNETCORE_URLS=http://+:8080

USER app
FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["MyProjectName.Cms/MyProjectName.Cms.csproj", "MyProjectName.Cms/"]
RUN dotnet restore "MyProjectName.Cms/MyProjectName.Cms.csproj"
COPY . .
WORKDIR "/src/MyProjectName.Cms"
RUN dotnet build "MyProjectName.Cms.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "MyProjectName.Cms.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
RUN mkdir -p /app/App_Data && chmod -R 777 /app/App_Data
ENTRYPOINT ["dotnet", "MyProjectName.Cms.dll"]
