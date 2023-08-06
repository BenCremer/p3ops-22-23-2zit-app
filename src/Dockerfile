# Variables
ARG repo=mcr.microsoft.com/dotnet

# Base layer
FROM $repo/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 8080/tcp

# Build layer
FROM $repo/sdk:6.0 AS build
WORKDIR /src
COPY Server/Server.csproj ./Server/
COPY Client/Client.csproj ./Client/
COPY Shared/Shared.csproj ./Shared/
COPY Domain/Domain.csproj ./Domain/
COPY Services/Services.csproj ./Services/
COPY Persistence/Persistence.csproj ./Persistence/
RUN dotnet restore 'Server/Server.csproj'
COPY . .
RUN dotnet build 'Server/Server.csproj' -c Release -o /app/build

# Publish layer
FROM build AS publish
RUN dotnet publish 'Server/Server.csproj' -c Release -o /app/publish

# Final layer
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Entrypoint
ENTRYPOINT ["dotnet", "Server.dll"]
