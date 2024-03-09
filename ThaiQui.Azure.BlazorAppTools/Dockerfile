FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
USER $APP_UID
WORKDIR /app
EXPOSE 80

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
ARG BUILD_CONFIGURATION=Release
WORKDIR /src
COPY ["ThaiQui.Azure.BlazorAppTools/ThaiQui.Azure.BlazorAppTools.csproj", "ThaiQui.Azure.BlazorAppTools/"]
RUN dotnet restore "ThaiQui.Azure.BlazorAppTools/ThaiQui.Azure.BlazorAppTools.csproj"
COPY . .
WORKDIR "/src/ThaiQui.Azure.BlazorAppTools"
RUN dotnet build "ThaiQui.Azure.BlazorAppTools.csproj" -c $BUILD_CONFIGURATION -o /app/build

FROM build AS publish
ARG BUILD_CONFIGURATION=Release
RUN dotnet publish "ThaiQui.Azure.BlazorAppTools.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "ThaiQui.Azure.BlazorAppTools.dll"]
