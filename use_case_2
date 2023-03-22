$condition           = "@@{condition}@@"
$username            = "@@{username}@@"
$groupname           = "@@{groupname}@@"
$emailgroup_checkbox = "@@{emailgroup_checkbox}@@"
$emailgroupname      = "@@{emailgroupname}@@"


## CrÃ©ation d'arguments permanents pour les requÃªtes ultÃ©rieures
$arguments = @{
  Server = 'localhost'
}


switch ( $condition )
{
  Add
  {
    ## Si l'utilisateur n'est pas déjà présent dans le groupe
    if ( !(get-adgroup -Filter 'name -eq $groupname' @arguments | Get-ADGroupMember @arguments | Where-Object {$_.samaccountname -eq $username}) )
    {
      ## Vérification de la présence du groupe et récupération de ses informations
      $group=get-adgroup -Filter 'name -eq $groupname' @arguments
      if ($group)
      {
        try {
          ## Ajout de l'utilisateur dans le groupe
          Add-ADGroupMember -Identity $group.SID -Members $username @arguments
          $log_text="L'utilisateur $username a été ajouté au groupe $groupname avec succès."
          Write-Host "`n$($log_text.ToUpper())" -ForegroundColor "green" -BackgroundColor "black"
        }
        catch {
          ## Envoi d'un message d'erreur
          write-host ""
          $log_text="Échec de l'ajout de l'utilisateur $username dans le groupe $groupname."
          Write-Host "`n$($log_text.ToUpper())`n$($Error[0].Exception.Message | Out-String)" -ForegroundColor "red" -BackgroundColor "black"
        }
      }
      else
      {
        ## Envoi d'un message d'erreur
        write-host ""
        $log_text="Le groupe $groupname n'existe pas."
        Write-Host "`n$($log_text.ToUpper())" -ForegroundColor "red" -BackgroundColor "black"
      }
    }
    else
    {
      ## Envoi d'un message d'erreur
      write-host ""
      $log_text="L'utilisateur $username se trouve déjà dans le groupe $groupname."
      Write-Host "`n$($log_text.ToUpper())" -ForegroundColor "red" -BackgroundColor "black"
    }

    ## Si ajout à un groupe de diffusion
    if ( $emailgroup_checkbox -eq $True )
    {
      if ( !([string]::IsNullOrWhiteSpace($emailgroupname)) )
      {
        if ( !(get-adgroup -Filter 'name -eq $emailgroupname' @arguments | Get-ADGroupMember @arguments | Where-Object {$_.samaccountname -eq $username}) )
        {
          ## Vérification de la présence du groupe de diffusion et récupération de ses informations
          $group=get-adgroup -Filter 'name -eq $emailgroupname' @arguments
          if ($group)
          {
            try {
              ## Ajout de l'utilisateur dans le groupe de diffusion
              Add-ADGroupMember -Identity $group.SID -Members $username @arguments
              $log_text="L'utilisateur $username a été ajouté au groupe de diffusion $emailgroupname avec succès."
              Write-Host "`n$($log_text.ToUpper())" -ForegroundColor "green" -BackgroundColor "black"
            }
            catch {
              ## Envoi d'un message d'erreur
              write-host ""
              $log_text="Échec de l'ajout de l'utilisateur $username dans le groupe de diffusion $emailgroupname."
              Write-Host "`n$($log_text.ToUpper())`n$($Error[0].Exception.Message | Out-String)" -ForegroundColor "red" -BackgroundColor "black"
            }
          }
          else
          {
            ## Envoi d'un message d'erreur
            write-host ""
            $log_text="Le groupe de diffusion $emailgroupname n'existe pas."
            Write-Host "`n$($log_text.ToUpper())" -ForegroundColor "red" -BackgroundColor "black"
          }
        }
        else
        {
          ## Envoi d'un message d'erreur
          write-host ""
          $log_text="L'utilisateur $username se trouve déjà dans le groupe de diffusion $emailgroupname."
          Write-Host "`n$($log_text.ToUpper())" -ForegroundColor "red" -BackgroundColor "black"
        }
      }
      else
      {
        ## Envoi d'un message d'erreur
        write-host ""
        $log_text="Vous avez choisi l'option d'ajout d'un utilisateur à un groupe de diffusion, mais ce dernier n'a pas été renseigné."
        Write-Host "`n$($log_text.ToUpper())" -ForegroundColor "red" -BackgroundColor "black"
      }
    }
  }
  Delete
  {
    ## Si l'utilisateur est bien présent dans le groupe
    if ( (get-adgroup -Filter 'name -eq $groupname' @arguments | Get-ADGroupMember @arguments | Where-Object {$_.samaccountname -eq $username}) )
    {
      ## Vérification de la présence du groupe et récupération de ses informations
      $group=get-adgroup -Filter 'name -eq $groupname' @arguments
      if ($group)
      {
        try {
          ## Suppression de l'utilisateur du groupe
          Remove-ADGroupMember -Identity $group.SID -Members $username -Confirm:$False @arguments
          $log_text="L'utilisateur $username a été supprimé du groupe $groupname avec succès."
          Write-Host "`n$($log_text.ToUpper())" -ForegroundColor "green" -BackgroundColor "black"
        }
        catch {
          ## Envoi d'un message d'erreur
          write-host ""
          $log_text="Échec de la suppression de l'utilisateur $username du groupe $groupname."
          Write-Host "`n$($log_text.ToUpper())`n$($Error[0].Exception.Message | Out-String)" -ForegroundColor "red" -BackgroundColor "black"
        }
      }
      else
      {
        ## Envoi d'un message d'erreur
        write-host ""
        $log_text="Le groupe $groupname n'existe pas."
        Write-Host "`n$($log_text.ToUpper())" -ForegroundColor "red" -BackgroundColor "black"
      }
    }
    else
    {
      ## Envoi d'un message d'erreur
      write-host ""
      $log_text="L'utilisateur $username ne se trouve pas dans le groupe $groupname."
      Write-Host "`n$($log_text.ToUpper())" -ForegroundColor "red" -BackgroundColor "black"
    }

    ## Si suppression d'un groupe de diffusion
    if ( $emailgroup_checkbox -eq $True )
    {
      if ( !([string]::IsNullOrWhiteSpace($emailgroupname)) )
      {
        if ( (get-adgroup -Filter 'name -eq $emailgroupname' @arguments | Get-ADGroupMember @arguments | Where-Object {$_.samaccountname -eq $username}) )
        {
          ## Vérification de la présence du groupe de diffusion et récupération de ses informations groupe
          $group=get-adgroup -Filter 'name -eq $emailgroupname' @arguments
          if ($group)
          {
            try {
              ## Suppression de l'utilisateur du groupe de diffusion
              Remove-ADGroupMember -Identity $group.SID -Members $username -Confirm:$False @arguments
              $log_text="L'utilisateur $username a été supprimé du groupe de diffusion $emailgroupname avec succès."
              Write-Host "`n$($log_text.ToUpper())" -ForegroundColor "green" -BackgroundColor "black"
            }
            catch {
              ## Envoi d'un message d'erreur
              write-host ""
              $log_text="Échec de la suppression de l'utilisateur $username du groupe de diffusion $emailgroupname."
              Write-Host "`n$($log_text.ToUpper())`n$($Error[0].Exception.Message | Out-String)" -ForegroundColor "red" -BackgroundColor "black"
            }
          }
          else
          {
            ## Envoi d'un message d'erreur
            write-host ""
            $log_text="Le groupe de diffusion $emailgroupname n'existe pas."
            Write-Host "`n$($log_text.ToUpper())" -ForegroundColor "red" -BackgroundColor "black"
          }
        }
        else
        {
          ## Envoi d'un message d'erreur
          write-host ""
          $log_text="L'utilisateur $username ne se trouve pas dans le groupe de diffusion $emailgroupname."
          Write-Host "`n$($log_text.ToUpper())" -ForegroundColor "red" -BackgroundColor "black"
        }
      }
      else
      {
        ## Envoi d'un message d'erreur
        write-host ""
        $log_text="Vous avez choisi l'option de suppression d'un utilisateur d'un groupe de diffusion, mais ce dernier n'a pas été renseigné."
        Write-Host "`n$($log_text.ToUpper())" -ForegroundColor "red" -BackgroundColor "black"
      }
    }
  }
}
