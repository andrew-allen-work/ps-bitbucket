Powershell BitBucket module
==========================

PS.BitBucket is a PowerShell module that provides cmdlets to intract with BitBucket instance. It uses most of the REST API reference availabe here [REST API][bitbucketAPI]   and few from other places like [Branch permission][branchpermission].

This allows fast access to [BitBucket][BitBucket] UI functions in automated way.

The module handles only authenticated method, it supports multiple funcationlity of BitBucket in group manner (useful for Administrator to GET stats), Here are few example (Detailed below) supported:
:`Project`, `Repo`, `Branch`, `BranchPermission`, `Size`, `ListOfRepos`, `CreateRepo`, `CreateBranch`, `CurrentVersion`

## Getting Started

PS.BitBucket is available on [PowerShell Gallery ][powershellgallery], You can install package if you have `PowerShellGet` installed already, otherwise you can follow the instruction to install from [PowerShell Gallery ][powershellgallery] here https://www.powershellgallery.com/.

## Install module

```powershell
Install-Module -Name PS.BitBucket
```

## To start with

Set BitBucket server URL and login Credentails, This will be used for all the command you run. In most cases, to get full information, its good to have `admininstrator` level access.

### Set server URL

```powershell
Set-BitBucketServer -Url "http://example:7990"
```

### Set Credential

```powershell
Set-BitBucketCredential -Credential admin
```

### Verify

```powershell
Get-BitBucketServer
Get-BitBucketInfo
```

Note: All these command output depends on permission you have. Any information are shown by these cmdlets are similar to what you see on UI.

### Get all projects

```powershell
Get-BitBucketProject -All
```

### Get all Repositories under given Project Name

```powershell
Get-BitBucketRepository -ProjectName "TES"
```

### Get all repository hosted on Bitbucket

```powershell
Get-BitBucketRepository -All
```

### Create a new empty repository

```powershell
New-BitBucketRepo -Project "TES" -Repository "ABC"
```

(There are mutliple optional param available for `New-BitBucketRepo`, All of these works if pass with `-WithGitFlowBranch`)

### Set the user full name and email (This required in order to get user real name when pushing new branches)

```powershell
Set-UserFullNameAndEmail -FullName "admin" -EmailId "itsmeshankar1@gmail.com"
```

### Create a new repository with gitflow branch (develop/master)

```powershell
New-BitBucketRepo -Project "TES" -Repository "ABC" -WithGitFlowBranch
```

Note: Above command also looks for a `.gitignore` file under `C:\Git\` folder.

### Create a new repository with gitflow branch and set `develop` as default branch (Git `push` marks `master` as default)

```powershell
New-BitBucketRepo -Project "TES" -Repository "ABC" -WithGitFlowBranch -SetDefaultBranch
```

You can also change default branch at later stage as

```powershell
Set-DefaultBranch -Project "TES" -Repository "ABC" -Branch "develop"
```

### Create a new repository with gitflow branch, Set branch permission as attached [Screenshot][branchpermissionimage]

```powershell
New-BitBucketRepo -Project "TES" -Repository "ABC" -WithGitFlowBranch -SetBranchPermission
```

Note: I have added a standard branch permission, every org has different set of branch permission, to use your own org standard branch permission, write a file similar to `public\BranchPermission.json`. There are two ways to set custom branch permission

1. Do while creating a new repo
```powershell
New-BitBucketRepo -Project "TES" -Repository "ABC" -WithGitFlowBranch -SetBranchPermission -BranchPermissionJson "C:\BranchPer.json"
```
2. Change at later stage for existing repo
```powershell
Set-BranchPermission -Project "TES" -Repository "ABC" --BranchPermissionJson "C:\BranchPer.json"
```
OR to set branch permission as defined in this module for existing repo:
```powershell
Set-BranchPermission -Project "TES" -Repository "ABC"
```

Below cmdlet will help to get size for repositories under given projects or all the repository hosted on BitBucket. These are requred sometime for audit purpose. (Limitation: These command will break if a repository name has space, I am still working on it)
### Get all repository Approximate size (in KBs) for given Projects

```powershell
Get-BitBucketRepository -Project "TES" | Select Name, SizeInKB
```

### Get all repository Approximate size (in KBs)

```powershell
Get-BitBucketRepository -All | Select Name, SizeInKB
```

### Create new branch

```powershell
New-CreateBranch -Project "TES" -Repository "TEST1" -SourceBranch "refs/heads/develop" -NewBranch "release/1.0"
```


## Disclaimer/Issues

This is an open source project which is NOT supported by the BitBucket team. All questions/bugs about this module should be entered on this [github][issues] project or feel free to contribute to this project if you have a possible fix for it. Feel free to request for new feature/cmdlet. Your suggestion/request will help shape up the module according to needs of the community. All these current implementation are based on my experience with BitBucket day to day usage.

## Authors

Created and maintained by [Shankar](<itsmeshankar1@gmail.com>).
Forked and updated by [CleverTwain](<clevertwain@gmail.com>).

## License

Apache License, Version 2.0 (see [LICENSE][LICENSE])

[powershellgallery]: https://www.powershellgallery.com/packages/PS.BitBucket
[download]: https://github.com/i9shankar/ps-bitbucket/archive/master.zip
[repository]: https://github.com/i9shankar/ps-bitbucket
[wiki]: https://github.com/i9shankar/ps-bitbucket/blob/master/README.md
[issues]: https://github.com/i9shankar/ps-bitbucket/issues
[bitbucket]: https://www.atlassian.com/software/bitbucket/download
[bitbucketapi]: https://developer.atlassian.com/static/rest/stash/latest/stash-rest.html
[branchpermission]: https://developer.atlassian.com/static/rest/bitbucket-server/latest/bitbucket-ref-restriction-rest.html
[license]: LICENSE.MD
[branchpermissionimage]: https://github.com/i9shankar/ps-bitbucket/blob/master/images/BranchPermission.png