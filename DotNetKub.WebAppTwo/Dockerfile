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
COPY ["DotNetKub.WebAppTwo/DotNetKub.WebAppTwo.csproj", "DotNetKub.WebAppTwo/"]
RUN dotnet restore "DotNetKub.WebAppTwo/DotNetKub.WebAppTwo.csproj"
COPY . .
WORKDIR "/src/DotNetKub.WebAppTwo"
RUN dotnet build "DotNetKub.WebAppTwo.csproj" -c $configuration -o /app/build

FROM build AS publish
ARG configuration=Release
RUN dotnet publish "DotNetKub.WebAppTwo.csproj" -c $configuration -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DotNetKub.WebAppTwo.dll"]
