## Запушить последние нугеты

```powershell
$NugetSourcePushTarget = "Gitlab"
$RiderBuildLogDirectory = "C:\Users\Admin\AppData\Local\JetBrains\Rider2021.1\log\SolutionBuilder"
$LastBuildLogFile = Get-ChildItem $RiderBuildLogDirectory | Sort-Object LastWriteTime | Select-Object -last 1
$FullPathToBuildLogFile = "$RiderBuildLogDirectory\$LastBuildLogFile"


nuget source Add -Name "GitLab" -Source "https://git.<your-company-name>.com/api/v4/projects/127/packages/nuget/index.json" -UserName <your-user-name> -Password <your-password>

Select-String -Path $FullPathToBuildLogFile -Pattern '"[A-Za-z0-9\-.:\\]+\.nupkg"' | Select-Object -Expand Matches | Select-Object -Expand Value | 

ForEach-Object {$_ -replace '"',''} | ForEach-Object -Process {nuget push $_ -Source $NugetSourcePushTarget}
```
