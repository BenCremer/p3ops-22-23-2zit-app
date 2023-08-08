# Variables
ARG repo=mcr.microsoft.com/dotnet
ENV DOTNOT_ENVIRONMENT Production
ENV DOTNET_ConnectionStrings_SQLDatabase server=192.168.56.1;uid=sa;pwd=TemporarySAPass123!;database=DevOps_EP3
FROM $repo/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 8080/tcp

# Build layer
FROM $repo/sdk:6.0 AS build
WORKDIR /src
COPY src/Server/Server.csproj ./Server/
COPY src/Client/Client.csproj ./Client/
COPY src/Shared/Shared.csproj ./Shared/
COPY src/Domain/Domain.csproj ./Domain/
COPY src/Services/Services.csproj ./Services/
COPY src/Persistence/Persistence.csproj ./Persistence/
RUN dotnet restore './Server/Server.csproj'
COPY . .
RUN dotnet build 'src/Server/Server.csproj' -c Release -o /app/build

# Publish layer
FROM build AS publish
RUN dotnet publish 'src/Server/Server.csproj' -c Release -o /app/publish

# Final layer
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

# Entrypoint
ENTRYPOINT ["dotnet", "Server.dll"]
