## Startup.cs
```
public void ConfigureServices(IServiceCollection services)
{
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)  
    .AddJwtBearer(options =>  
    {  
        options.TokenValidationParameters = new TokenValidationParameters  
        {  
            ValidateIssuer = true,  
            ValidateAudience = true,  
            ValidateLifetime = true,  
            ValidateIssuerSigningKey = true,  
            ValidIssuer = Configuration["Jwt:Issuer"],  
            ValidAudience = Configuration["Jwt:Issuer"],  
            IssuerSigningKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(Configuration["Jwt:Key"]))  
        };  
    });  
    ....
}
```
```
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    ....
    app.UseAuthentication();
}
```
