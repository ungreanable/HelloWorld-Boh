#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/core/aspnet:3.1-buster-slim AS base
WORKDIR /app
EXPOSE 8081
EXPOSE 443

FROM mcr.microsoft.com/dotnet/core/sdk:3.1-buster AS build
WORKDIR /src
COPY ["HelloWorld-Boh/HelloWorld-Boh.csproj", "HelloWorld-Boh/"]
RUN dotnet restore "HelloWorld-Boh/HelloWorld-Boh.csproj"
COPY . .
WORKDIR "/src/HelloWorld-Boh"
RUN dotnet build "HelloWorld-Boh.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "HelloWorld-Boh.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
# Expose the port in ASP.Net Core
ENV ASPNETCORE_URLS=http://*:8080
ENTRYPOINT ["dotnet", "HelloWorld-Boh.dll"]
