FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["Dotnet8ContainerBuild.csproj", "Dotnet8ContainerBuild/"]
RUN dotnet restore "Dotnet8ContainerBuild/Dotnet8ContainerBuild.csproj"
WORKDIR "/src/Dotnet8ContainerBuild"
COPY . .
RUN dotnet build "Dotnet8ContainerBuild.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "Dotnet8ContainerBuild.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Dotnet8ContainerBuild.dll"]
