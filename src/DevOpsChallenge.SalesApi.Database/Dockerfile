FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
EXPOSE 50000

ENV ASPNETCORE_URLS=http://+:50000

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["src/DevOpsChallenge.SalesApi.Database/DevOpsChallenge.SalesApi.Database.csproj", "src/DevOpsChallenge.SalesApi.Database/"]
RUN dotnet restore "src\DevOpsChallenge.SalesApi.Database\DevOpsChallenge.SalesApi.Database.csproj"
COPY . .
WORKDIR "/src/src/DevOpsChallenge.SalesApi.Database"
RUN dotnet build "DevOpsChallenge.SalesApi.Database.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "DevOpsChallenge.SalesApi.Database.csproj" -c Release -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "DevOpsChallenge.SalesApi.Database.dll"]
