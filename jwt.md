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

## appsettings.json 
```
"Jwt": {  
    "Key": "ThisismySecretKey",  
    "Issuer": "Test.com"  
  }  
```

## LoginController.cs
```
using System;
using System.Collections.Generic;
using System.IdentityModel.Tokens.Jwt;
using System.Linq;
using System.Security.Claims;
using System.Text;
using System.Threading.Tasks;
using Authentication.Models;
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Configuration;
using Microsoft.IdentityModel.Tokens;

namespace Authentication.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    public class LoginController : ControllerBase
    {

        private IConfiguration _config;  
  
        public LoginController(IConfiguration config)  
        {  
            _config = config;  
        }  


        [HttpGet]
        [Authorize]  
        public ActionResult<IEnumerable<string>> Get()
        {
            return new string[] { "value1", "value2" };
        }

        
        [AllowAnonymous]  
        [HttpPost]  
        public IActionResult Login([FromBody]UserModel login)  
        {  
            IActionResult response = Unauthorized();  
            var user = AuthenticateUser(login);  
  
            if (user != null)  
            {  
                var tokenString = GenerateJSONWebToken(user);  
                response = Ok(new { token = tokenString });  
            }  
  
            return response;  
        }  
  
        private string GenerateJSONWebToken(UserModel userInfo)  
        {  
            var securityKey = new SymmetricSecurityKey(Encoding.UTF8.GetBytes(_config["Jwt:Key"]));  
            var credentials = new SigningCredentials(securityKey, SecurityAlgorithms.HmacSha256);  
  
            var claims = new Claim[]
                            {
                                new Claim("id", "ID"),
                                new Claim("name", "username"),
                                //new Claim(JwtRegisteredClaimNames.Iat, ToUnixEpochDate(now).ToString(), ClaimValueTypes.Integer64)
                            };
            var token = new JwtSecurityToken
                            (
                                _config["Jwt:Issuer"],  
                                _config["Jwt:Issuer"],  
                                claims, 
                                DateTime.UtcNow, 
                                DateTime.UtcNow.AddSeconds(20),  
                                credentials
                            );  
  
            return new JwtSecurityTokenHandler().WriteToken(token); 
        }  
  
        private UserModel AuthenticateUser(UserModel login)  
        {  
            UserModel user = login;  
  
            //Validate the User Credentials  
            //Demo Purpose, I have Passed HardCoded User Information  
            if (user.Username == "Jignesh")  
            {  
                user = new UserModel { Username = "Jignesh Trivedi", EmailAddress = "test.btest@gmail.com" };  
            }  
            return user;  
        }
    }
}
```
