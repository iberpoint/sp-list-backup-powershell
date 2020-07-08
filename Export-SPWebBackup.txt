Add-PSSnapin Microsoft.Sharepoint.Powershell

$site = Get-SPSite
$path = "C:\Backup2015\ListBackups"
$serverurlpath =""
$listurlpath=""
$listurlwithoutformat=""
$dosyapath =""
$itemURLNEW=""

foreach($web in $site.AllWebs){
    $serverurlpath=$web.Url 
    $serverlistsurlpath=$web.ServerRelativeUrl -replace "/","\" 
    $serverurlport = ([System.Uri]$serverurlpath).Port
    $serverurlhost = ([System.Uri]$serverurlpath).Host.Split('.-') -join "_"   
    
    if($serverurlport -eq "80")
    { 
     if(!(Test-Path -Path ($path + "\" + $serverurlport)))
     {
      New-Item -Path ($path + "\" + $serverurlport) -Type Directory      
     }    

     if(!(Test-Path -Path (($path + "\" + $serverurlport + "\" + $serverurlhost))))
     {
       New-Item -Path ($path + "\" + $serverurlport + "\" + $serverurlhost) -Type Directory
     }

     foreach($list in $web.lists)
     {      
            
        If (!(Test-Path ($path + "\" + $serverurlport + "\" + $serverurlhost + $serverlistsurlpath))) {        
              New-Item -Path ($path + "\" + $serverurlport + "\" + $serverurlhost + $serverlistsurlpath) -Type Directory 
        }            
       
        $itemURLNEW= "$($list.RootFolder.ServerRelativeUrl)"       
        if($serverlistsurlpath -eq "\")
        {           
          $dosyapath = "$path\$($serverurlport)\$($serverurlhost)$($serverlistsurlpath)$($list -replace "/","\").cmp"           
        }

        else
        {           
          $dosyapath = "$path\$($serverurlport)\$($serverurlhost)$($serverlistsurlpath)\$($list-replace "/","\").cmp"
        }   
        
        if(!(Test-Path ($dosyapath)) -and $list.Hidden -eq $false)
        {       
           export-spweb $web.URL -ItemUrl $itemURLNEW -IncludeUserSecurity -IncludeVersions All -path "$dosyapath" -nologfile         
        }
     }
    }   

    if($serverurlport -eq "1111")
    { 
     if(!(Test-Path -Path ($path + "\" + $serverurlport)))
     {
      New-Item -Path ($path + "\" + $serverurlport) -Type Directory      
     }    

     if(!(Test-Path -Path (($path + "\" + $serverurlport + "\" + $serverurlhost))))
     {
       New-Item -Path ($path + "\" + $serverurlport + "\" + $serverurlhost) -Type Directory
     }

     foreach($list in $web.lists)
     {   
        
        If (!(Test-Path ($path + "\" + $serverurlport + "\" + $serverurlhost + $serverlistsurlpath))) {        
              New-Item -Path ($path + "\" + $serverurlport + "\" + $serverurlhost + $serverlistsurlpath) -Type Directory 
        }            
       
        $itemURLNEW= "$($list.RootFolder.ServerRelativeUrl)" 
       
        if($serverlistsurlpath -eq "\")
        {         
           $dosyapath = "$path\$($serverurlport)\$($serverurlhost)$($serverlistsurlpath)$($list -replace "/","\").cmp"                    
        }

        else
        {          
           $dosyapath = "$path\$($serverurlport)\$($serverurlhost)$($serverlistsurlpath)\$($list -replace "/","\").cmp"            
        }       
        
        if(!(Test-Path ($dosyapath)) -and $list.Hidden -eq $false)
        {            
           export-spweb $web.URL -ItemUrl $itemURLNEW -IncludeUserSecurity -IncludeVersions All -path "$dosyapath" -nologfile
        }
     }             
    }          
} 