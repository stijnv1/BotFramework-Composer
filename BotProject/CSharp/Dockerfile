FROM mcr.microsoft.com/dotnet/core/sdk:2.1-alpine AS build

WORKDIR /src/botproject/csharp

COPY *.sln .
COPY *.csproj .
COPY Tests/*.csproj Tests/
COPY NuGet.Config .

# run restore to create a distinct layer
RUN dotnet restore

COPY . .
RUN dotnet publish -o out

FROM mcr.microsoft.com/dotnet/core/aspnet:2.1-alpine AS runtime
RUN addgroup -g 1000 dotnetcoregroup \
    && adduser -u 1000 -G dotnetcoregroup -s /bin/sh dotnetcoreuser
USER 1000
EXPOSE 5000
ENV ASPNETCORE_URLS=http://*:5000
WORKDIR /app/botproject/csharp
COPY --from=build /src/botproject/csharp/ComposerDialogs ./ComposerDialogs
COPY --from=build /src/botproject/csharp/out .
CMD ["dotnet", "BotProject.dll"]
