# escape=`
FROM microsoft/aspnet:windowsservercore
SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

# configure IIS to write a global log file:
RUN Set-WebConfigurationProperty -p 'MACHINE/WEBROOT/APPHOST' -fi 'system.applicationHost/log' -n 'centralLogFileMode' -v 'CentralW3C'; `
    Set-WebConfigurationProperty -p 'MACHINE/WEBROOT/APPHOST' -fi 'system.applicationHost/log/centralW3CLogFile' -n 'truncateSize' -v 4294967295; `
    Set-WebConfigurationProperty -p 'MACHINE/WEBROOT/APPHOST' -fi 'system.applicationHost/log/centralW3CLogFile' -n 'period' -v 'MaxSize'; `
    Set-WebConfigurationProperty -p 'MACHINE/WEBROOT/APPHOST' -fi 'system.applicationHost/log/centralW3CLogFile' -n 'directory' -v 'c:\iislog'

WORKDIR C:\iis-env
RUN Remove-Website -Name 'Default Web Site';`
    New-Website -Name 'iis-env' -Port 80 -PhysicalPath 'C:\iis-env'

ENV A01_KEY A01 value
ENV A02_KEY="A02 value" `
    A03_KEY="A03 value"

COPY bootstrap.ps1 C:\
ENTRYPOINT ["powershell", "C:\\bootstrap.ps1"]

COPY default.aspx .

## docker build -t dockeronwindows/ch03-iis-environment-variables .
## docker container run -d -P --name iis-env dockeronwindows/ch03-iis-environment-variables
##
## Example on usign environment variables
## docker container run  --env ENV_01='Hello' --env ENV_02='World'  microsoft/nanoserver powershell 'Write-Output $env:ENV_01 $env:ENV_02'
##
## IIS reads variables at machine level and docker writes environment variables
## at the process level, so docker needs to promote environment variables from process level
## to machine level. Use bootstrap.ps1 for this.