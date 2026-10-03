# ---------- Build stage ----------
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

COPY payment-service.csproj ./
RUN dotnet restore payment-service.csproj

COPY Program.cs ./
RUN dotnet publish payment-service.csproj \
    -c Release \
    -o /app/publish \
    --no-restore \
    /p:UseAppHost=false

# ---------- Runtime stage ----------
FROM mcr.microsoft.com/dotnet/aspnet:8.0
WORKDIR /app

# curl is used by the container health check.
RUN apt-get update && \
    apt-get install -y --no-install-recommends curl && \
    rm -rf /var/lib/apt/lists/*

COPY --from=build --chown=app:app /app/publish ./

USER app
EXPOSE 8085
ENTRYPOINT ["dotnet", "payment-service.dll"]
