# Swagger

## Add Package 
```
dotnet add package NSwag.AspNetCore
```

## Startup.cs
```
public void ConfigureServices(IServiceCollection services)
{
    ....
    // Register the Swagger services
    services.AddSwaggerDocument();
}
```
```
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseSwagger();
    app.UseSwaggerUi3();
}
```
## buka swagger
```
http://localhost:5000/swagger
```
