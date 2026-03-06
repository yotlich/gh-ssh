# GitHub SSH

## Create config dir

```powershell
ni -f -i dir $env:USERPROFILE\.ssh
```

## Generate auth key

```powershell
ssh-keygen -t ed25519 `
  -f $env:USERPROFILE\.ssh\github-com.key `
  -C "$env:USERNAME@$env:COMPUTERNAME"; `
  cat $env:USERPROFILE\.ssh\github-com.key.pub |
  Set-Clipboard
```

## Enable config

```powershell
Write-Output @'
Host github.com
    User git
    Port 22
    IdentityFile ~/.ssh/github-com.key
    IdentitiesOnly yes
    PreferredAuthentications publickey
    PasswordAuthentication no
'@ | Out-File -a -e ascii $env:USERPROFILE\.ssh\config
```

## Generate sign key

```powershell
ssh-keygen -t ed25519 `
  -f $env:USERPROFILE\.ssh\github-com.sign.key `
  -C "$env:USERNAME@$env:COMPUTERNAME"; `
  cat $env:USERPROFILE\.ssh\github-com.sign.key.pub |
  Set-Clipboard
```

## Setup repository

```powershell
git init
```

```powershell
git config user.name 'yotlich'
git config user.email '149834513+yotlich@users.noreply.github.com'
git config gpg.format ssh
git config user.signingKey ~/.ssh/github-com.sign.key.pub
git config commit.gpgSign true
git config tag.gpgSign true

```

## Add remote

```powershell
git branch -M main
git remote add origin git@github.com:yotlich/
```
