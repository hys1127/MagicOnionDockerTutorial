#See https://aka.ms/containerfastmode to understand how Visual Studio uses this Dockerfile to build your images for faster debugging.

FROM mcr.microsoft.com/dotnet/aspnet:5.0 AS base
WORKDIR /app
#ENV ASPNETCORE_URLS="http//*:12345"
#EXPOSE 80
#EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:5.0 AS build
WORKDIR /src
COPY ["./MyApp-Server/", "./MyApp-Server/"]
COPY ["./MyApp-Shared/", ".MyApp-Shared/"]
RUN dotnet restore "./MyApp-Server/MyApp-Server.csproj"
COPY . .
WORKDIR "/src/MyApp-Server"
RUN dotnet build "MyApp-Server.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "MyApp-Server.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
RUN apt-get update \ 
    && apt-get install -y --allow-unauthenticated \ 
       libgdiplus \ 
       libc6-dev \ 
       libx11-dev \
    && rm -rf /var/lib/apt/lists/*
ENTRYPOINT ["dotnet", "MyApp-Server.dll"]
EXPOSE 12345
