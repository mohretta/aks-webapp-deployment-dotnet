# Build (avec Node pour ClientApp)
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Installer NodeJS + npm
RUN apt-get update \
 && apt-get install -y curl ca-certificates gnupg \
 && curl -fsSL https://deb.nodesource.com/setup_20.x | bash - \
 && apt-get install -y nodejs \
 && node -v && npm -v

COPY . .
WORKDIR /src/GC.WebReact
RUN dotnet restore
RUN dotnet publish -c Release -o /app/publish

# Run
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS runtime
WORKDIR /app
COPY --from=build /app/publish .
ENV ASPNETCORE_URLS=http://+:5000
EXPOSE 5000
ENTRYPOINT ["dotnet","GC.WebReact.dll"]