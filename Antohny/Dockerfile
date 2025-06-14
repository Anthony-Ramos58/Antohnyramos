# Imagen base de runtime para producción
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

# Imagen base para build
FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src

# Copia el archivo del proyecto y restaura dependencias
COPY ["Antohny/Antohny.csproj", "Antohny/"]
RUN dotnet restore "Antohny/Antohny.csproj"

# Copia el resto del código
COPY . .

# Establece el directorio y compila
WORKDIR "/src/Antohny"
RUN dotnet build "Antohny.csproj" -c Release -o /app/build

# Publicación del proyecto
FROM build AS publish
RUN dotnet publish "Antohny.csproj" -c Release -o /app/publish

# Imagen final
FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .

ENTRYPOINT ["dotnet", "Antohny.dll"]
