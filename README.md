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

SET PATH=%PATH%;"C:\Program Files (x86)\Microsoft Visual Studio\2019\Community\MSBuild\Current\Bin\MSBuild.exe"

set configurations[0]=CreditErcDev

set configurations[1]=Dummy-Sit

set configurations[2]=ProdParallelDEV

set configurations[3]=FRTBDev

set configurations[4]=FRTB03

set configurations[5]=FRTB01

set configurations[6]=FRTB02

set configurations[7]=FRTB04

set configurations[8]=FRTB05

for /L %%i in (0,1,8) do (

  set config=%configurations[%%i]%
  
  MSBuild.exe .\\%1 /nologo /p:PublishProfile=JenkinProfile /p:Configuration=%config% /p:Platform="Any CPU" /p:DeployOnBuild=true /p:DeleteExistingFiles=true /p:TransformConfigFiles=true
  
  PowerShell Copy-Item Config\WebApi.Config\publish\Web.config Config\WebApi.Config\TransformedConfig\%config%.config
)



