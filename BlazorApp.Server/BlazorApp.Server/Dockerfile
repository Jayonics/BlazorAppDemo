FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 8080
EXPOSE 8081

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["BlazorApp.Server/BlazorApp.Server/BlazorApp.Server.csproj", "BlazorApp.Server/BlazorApp.Server/"]
COPY ["BlazorApp.Server/BlazorApp.Server.Client/BlazorApp.Server.Client.csproj", "BlazorApp.Server/BlazorApp.Server.Client/"]
RUN dotnet restore "BlazorApp.Server/BlazorApp.Server/BlazorApp.Server.csproj"
COPY . .
WORKDIR "/src/BlazorApp.Server/BlazorApp.Server"
RUN dotnet build "BlazorApp.Server.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "BlazorApp.Server.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "BlazorApp.Server.dll"]
