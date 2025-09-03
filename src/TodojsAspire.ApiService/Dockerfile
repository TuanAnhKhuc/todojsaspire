# === Giai đoạn 1: RESTORE ===
# Tầng này chỉ để restore các nuget package
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS restore
WORKDIR /src

# Copy tất cả các file .csproj và .sln
COPY src/*.sln .
COPY src/TodojsAspire.ApiService/*.csproj ./TodojsAspire.ApiService/
COPY src/TodojsAspire.ServiceDefaults/*.csproj ./TodojsAspire.ServiceDefaults/
COPY src/TodojsAspire.AppHost/*.csproj ./TodojsAspire.AppHost/

# Chạy restore cho toàn bộ solution
RUN dotnet restore

# === Giai đoạn 2: PUBLISH ===
# Tầng này kế thừa từ tầng restore và thực hiện build/publish
FROM mcr.microsoft.com/dotnet/sdk:9.0 AS publish
WORKDIR /src

# Copy các thư mục obj đã được restore từ tầng trước
COPY --from=restore /src .

# Copy toàn bộ source code
COPY src/TodojsAspire.ApiService ./TodojsAspire.ApiService
COPY src/TodojsAspire.ServiceDefaults ./TodojsAspire.ServiceDefaults

# Publish project ApiService
RUN dotnet publish "TodojsAspire.ApiService/TodojsAspire.ApiService.csproj" -c Release -o /app/publish

# === Giai đoạn 3: FINAL ===
# Tầng cuối cùng, chỉ chứa runtime và kết quả publish, rất nhẹ
FROM mcr.microsoft.com/dotnet/aspnet:9.0
WORKDIR /app
COPY --from=publish /app/publish .
EXPOSE 8080
ENTRYPOINT ["dotnet", "TodojsAspire.ApiService.dll"]