# .NET-Deployment-on-Linux-with-NGINX
# Setup Linux Server with .NET
https://learn.microsoft.com/en-us/dotnet/core/install/linux-debian#troubleshooting

[SOLVED] I get an error when trying to check for updates....
Comment out or delete the lines pertaining to the cdrom in /etc/apt/sources.list. You can also accomplish this from the software sources or synaptic under the administration menu in Gnome (provided you have these installed). If not you can this to edit the sources.list (from root terminal)
nano /etc/apt/sources.list
After you comment out the cdrom lines with # press ctrl+x followed by y to save. Then run apt-get update

# Create the projet on dev machine
dotnet new webapi -n Test_Demo

# Publish the project
dotnet publish -c Release .\Test_Demo.csproj
dotnet run .\bin\Release\net7.0\publish\Test_Demo.dll --environment Development
dotnet .\bin\Release\net7.0\publish\Test_Demo.dll --environment Development

Swagger is available Only for --environment Development
http://localhost:5000/swagger/index.html

Look at this piece of code in the Program.cs
// Configure the HTTP request pipeline.
if (app.Environment.IsDevelopment())
{
  app.UseSwagger();
  app.UseSwaggerUI();
}

You can test the EndPoint in --environment Development and Production
Without the --environment it starts as Production

dotnet run .\bin\Release\net7.0\publish\Test_Demo.dll
dotnet .\bin\Release\net7.0\publish\Test_Demo.dll 

curl 'http://localhost:5000/WeatherForecast'

# Intall NGINX
https://learn.microsoft.com/en-us/aspnet/core/host-and-deploy/linux-nginx?view=aspnetcore-7.0&tabs=linux-ubuntu
sudo apt update
sudo apt install nginx
sudo systemctl start nginx

sudo nano /etc/systemd/system/apponlinux.service
systemctl start apponlinux.service
systemctl status apponlinux.service
journalctl -u apponlinux.service

# Setup NGINX
nano /etc/nginx/sites-enabled/apponlinux
systemctl restart nginx.service
systemctl status nginx.service
