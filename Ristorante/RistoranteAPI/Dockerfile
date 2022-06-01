#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:6.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:6.0 AS build
WORKDIR /src
COPY ["RistoranteAPI/RistoranteAPI.csproj", "RistoranteAPI/"]
COPY ["BL/Logic.csproj", "BL/"]
COPY ["Models/Models.csproj", "Models/"]
COPY ["Accounts/Accounts.csproj", "Accounts/"]
COPY ["DL/RateAppDL.csproj", "DL/"]
RUN dotnet restore "RistoranteAPI/RistoranteAPI.csproj"
COPY . .
WORKDIR "/src/RistoranteAPI"
RUN dotnet build "RistoranteAPI.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "RistoranteAPI.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "RistoranteAPI.dll"]