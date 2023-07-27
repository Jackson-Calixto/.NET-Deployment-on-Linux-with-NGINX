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

# configure linux service to start automatically
https://www.digitalocean.com/community/tutorials/how-to-configure-a-linux-service-to-start-automatically-after-a-crash-or-reboot-part-2-reference

# Setup NGINX
nano /etc/nginx/sites-enabled/apponlinux
systemctl restart nginx.service
systemctl status nginx.service

# Using Free Let’s Encrypt SSL/TLS Certificates with NGINX
https://www.nginx.com/blog/using-free-ssltls-certificates-from-lets-encrypt-with-nginx/

# Tunneling
# with localtunnel
lt --port 5000 --subdomain api-realize
https://api-realize.loca.lt/swagger/index.html
# with ngrok
https://dashboard.ngrok.com/tunnels/agents

# with cloudflare
https://one.dash.cloudflare.com/36d83243c1aaa8ac8d0c6e53886f3552/access/tunnels

# Run a Shell Script as Systemd Service in Linux to open localtunnel
https://www.funoracleapps.com/2022/02/run-shell-script-as-systemd-service-in.html

nano /usr/bin/lt.apponlinux.sh
tmux new -d 'lt --port 5000 --subdomain api-realize'
chmod +x /usr/bin/lt.apponlinux.sh

nano /etc/systemd/system/lt.apponlinux.service

# Map SSH Driver
http://makerlab.cs.hku.hk/index.php/en/mapping-network-drive-over-ssh-in-windows

# Publishing to SSH folder directly
dotnet publish -c Release -o R:\apponlinux\ .\Test_Demo.csproj