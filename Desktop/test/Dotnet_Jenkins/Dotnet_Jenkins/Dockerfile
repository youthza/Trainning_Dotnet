FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 8088

ENV ASPNETCORE_URLS=http://+:8088

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["Dotnet_Jenkins/Dotnet_Jenkins.csproj", "Dotnet_Jenkins/"]
RUN dotnet restore "Dotnet_Jenkins\Dotnet_Jenkins.csproj"
COPY . .
WORKDIR "/src/Dotnet_Jenkins"
RUN dotnet build "Dotnet_Jenkins.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "Dotnet_Jenkins.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "Dotnet_Jenkins.dll"]
