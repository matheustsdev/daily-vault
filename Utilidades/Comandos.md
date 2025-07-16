## Build .Net

### Topcon.Integrator

```bash
sudo dotnet publish Topcon.Integrator/Topcon.Integrator.csproj --configuration Release --runtime win-x64 --output /home/matheus/dev/Topcon_INTEGRATOR/build && cd /home/matheus/dev/Topcon_INTEGRATOR
/build && sudo rm -rf appsettings.* && zip ../../Topcon.Integrator.zip * && cd .. && sudo rm -rf /home/matheus/dev/Topcon_INTEGRATOR/build && ls -la
```
### Topcon.Integrator.Automation

```bash
sudo dotnet publish Topcon.Integrator.Automation/Topcon.Integrator.Automation.csproj --configuration Release --runtime win-x64 --output /home/matheus/dev/Topcon_INTEGRATOR/build && cd /home/matheus/dev/Topcon_INTEGRATOR/build && sudo rm -rf appsettings.* && zip ../../Topcon.Integrator.Automation.zip * && cd .. && sudo rm -rf /home/matheus/dev/Topcon_INTEGRATOR/build && ls -la
```
