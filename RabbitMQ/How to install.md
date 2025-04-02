1. For Windows, run PowerShell as admin.
2. Run this command: `Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))`
3. Run this command: `choco install rabbitmq`
4. For both your Server and Client applications install NuGet package `RabbitMQ.Client`
5. 