ARG SDK_VERSION=9.0

FROM mcr.microsoft.com/dotnet/sdk:8.0.412@sha256:4763fe4aeb0c5f0dec72fe29678329113f9ee5d4f092a86a5a5044c0bb18e974 AS dotnet-sdk-8.0
FROM mcr.microsoft.com/dotnet/sdk:9.0.303@sha256:86fe223b90220ec8607652914b1d7dc56fc8ff422ca1240bb81e54c4b06509e6 AS dotnet-sdk-9.0

FROM dotnet-sdk-${SDK_VERSION} AS build
ARG PUBLISH_CONFIGURATION=Release
ARG PUBLISH_FRAMEWORK=net9.0
WORKDIR /app
COPY . ./
RUN dotnet publish ./examples/MicroserviceExample/WorkerService -c "${PUBLISH_CONFIGURATION}" -f "${PUBLISH_FRAMEWORK}" -o /out -p:IntegrationBuild=true

FROM mcr.microsoft.com/dotnet/aspnet:8.0.18@sha256:fbc9110dff9359545a6e1e67cc8295a3e001fe3c04c677abedb6e764d4277b3a AS dotnet-aspnet-8.0
FROM mcr.microsoft.com/dotnet/aspnet:9.0.5@sha256:d5507d8292fb2469ec0b9913f3cd4bb8d5a014bd6dc00c33fd4c4e7310229f07 AS dotnet-aspnet-9.0

FROM dotnet-aspnet-${SDK_VERSION} AS runtime
WORKDIR /app
COPY --from=build /out ./
ENTRYPOINT ["dotnet", "WorkerService.dll"]
