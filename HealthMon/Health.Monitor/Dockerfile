FROM microsoft/dotnet:2.2-aspnetcore-runtime AS base
# Copy self signed development certificates
WORKDIR /usr/local/share/ca-certificates
COPY ./dev_certs/*.crt .
RUN update-ca-certificates -v
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM microsoft/dotnet:2.2-sdk AS build
WORKDIR /src
COPY ["Health.Monitor/Health.Monitor.csproj", "Health.Monitor/"]
RUN dotnet restore "Health.Monitor/Health.Monitor.csproj"
COPY . .
WORKDIR "/src/Health.Monitor"
RUN dotnet build "Health.Monitor.csproj" -c Release -o /app

FROM build AS publish
RUN dotnet publish "Health.Monitor.csproj" -c Release -o /app

FROM base AS final
WORKDIR /app
COPY --from=publish /app .
ENTRYPOINT ["dotnet", "Health.Monitor.dll"]