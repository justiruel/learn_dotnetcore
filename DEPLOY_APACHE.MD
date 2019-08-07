# Deploy net core apache linux
#### ref => https://www.youtube.com/watch?v=u3GisIV7l3s

- Jalankan
```
sudo su

wget -q https://packages.microsoft.com/config/ubuntu/16.04/packages-microsoft-prod.deb

dpkg -i packages-microsoft-prod.deb

apt-get install apt-transport-https

apt-get update

apt-get install dotnet-sdk-2.2

cd /var

mkdir webapp

dotnet new webapi --no-https

dotnet build
```

- edit Startup.cs
```
using Microsoft.AspNetCore.HttpOverrides;
....

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }

    app.UseForwardedHeaders(new ForwardedHeadersOptions
    {
        ForwardedHeaders = ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto
    });

    app.UseAuthentication();
    app.UseMvc();
}

```

- Jalankan
```
dotnet build

dotnet run
```
or publish
```
dotnet publish --configuration Release
dotnet webapp.dll
```

- Jalankan
```
apt-get install apache2

cd /etc/apache2/sites-available/

cp 000-default.conf webapp.conf
```

- edit webapp.conf
```
<VirtualHost *:*>
    RequestHeader set "X-Forwarded-Proto" expr=%{REQUEST_SCHEME}
</VirtualHost>

<VirtualHost *:81>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:7000/
        ProxyPassReverse / http://127.0.0.1:7000/

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

<VirtualHost *:80>
        ProxyPreserveHost On
        ProxyPass / http://127.0.0.1:5000/
        ProxyPassReverse / http://127.0.0.1:5000/

        ServerAdmin webmaster@localhost
        DocumentRoot /var/www/html

        ErrorLog ${APACHE_LOG_DIR}/error.log
        CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

- Jalankan
```
a2ensite webapp.conf

a2dissite 000-default.conf

a2enmod headers proxy proxy_http
```

vim /etc/apache2/ports.conf
```
Listen 80
Listen 81

```

- Jalankan
```
systemctl restart apache2
```

- akses http://127.0.0.1/api/values  ----> akan diredirect ke port 5000
- akses http://127.0.0.1:81/api/values  ----> akan diredirect ke port 7000
