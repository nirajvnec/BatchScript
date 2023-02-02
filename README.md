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


@echo off
setlocal enabledelayedexpansion

set profiles=CreditErcDev Dummy-Sit ProdParallelDEV FRTBDev FRTB03 FRTB01 FRTB02

for %%i in (%profiles%) do (
  MSBuild.exe .\\%1 /nologo /p:PublishProfile=JenkinProfile /p:Configuration=%%i /p:Platform="Any CPU" /p:DeployOnBuild=true /p:DeleteExistingFiles=true /p:TransformConfigFiles=true

  PowerShell Copy-Item Config\WebApi.Config\publish\Web.config Config\WebApi.Config\TransformedConfig\%%i.config
)

