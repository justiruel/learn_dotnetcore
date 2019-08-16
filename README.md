# Perintah dasar

## Help
```
dotnet --help
```

## Create new solution (group of proyek)
```
dotnet "new" "sln" "-n" "Testing" "-o" "d:\PROJECT\RESEARCH\dotnetcore\Testing"
```

## Create new API project
``` 
dotnet new webapi -o TodoApi
```

## Add proyek ke solution
```
dotnet "sln" "d:\PROJECT\RESEARCH\dotnetcore\Testing\Testing.sln" "add" "d:\PROJECT\RESEARCH\dotnetcore\Testing\TodoApi\TodoApi.csproj"
```

## Build Project
```
dotnet build
```

## Jalankan project
```
dotnet run
```

## Add package nugget
```
dotnet add package Pomelo.EntityFrameworkCore.MySql --version 2.0.1
```

## dotnet restore (sama dengan npm install, dipergunakan jika library sudah tercatat di .csproj)
```
dotnet restore
```

## publish
```
dotnet publish -c Release -o ./publish
```
selanjutnya copy ke IIS


## run published project
```
dotnet <nama_project>.dll
```

## Migration
```
dotnet ef migrations add addNIP -c TodoContext -o Migrations\TodoDB
dotnet ef database update
```
