# BatchScript
Batch Script Iteration
@echo off
setlocal enabledelayedexpansion

set configurations=CreditErcDev Dummy-Sit ProdParallelDEV FRTBDev

for %%c in (%configurations%) do (
  MSBuild.exe .\\%1 /nologo /p:PublishProfile=JenkinProfile /p:Configuration=%%c /p:Platform="Any CPU" /p:DeployOnBuild=true /p:DeleteExistingFiles=true /p:TransformConfigFiles=true

  PowerShell Copy-Item Config\WebApi.Config\publish\Web.config Config\WebApi.Config\TransformedConfig\%%c.config
)

endlocal
