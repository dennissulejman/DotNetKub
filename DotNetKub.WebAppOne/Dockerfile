FROM mcr.microsoft.com/dotnet/aspnet:7.0 AS base
WORKDIR /app
EXPOSE 8080

ENV ASPNETCORE_URLS=http://+:8080

# Creates a non-root user with an explicit UID and adds permission to access the /app folder
# For more info, please refer to https://aka.ms/vscode-docker-dotnet-configure-containers
RUN adduser -u 5678 --disabled-password --gecos "" appuser && chown -R appuser /app
USER appuser

FROM --platform=$BUILDPLATFORM mcr.microsoft.com/dotnet/sdk:7.0 AS build
ARG configuration=Release
WORKDIR /src
COPY ["DotNetKub.WebAppOne/DotNetKub.WebAppOne.csproj", "DotNetKub.WebAppOne/"]
RUN dotnet restore "DotNetKub.WebAppOne/DotNetKub.WebAppOne.csproj"
COPY . .
WORKDIR "/src/DotNetKub.WebAppOne"
RUN dotnet build "DotNetKub.WebAppOne.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "DotNetKub.WebAppOne.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DotNetKub.WebAppOne.dll"]
