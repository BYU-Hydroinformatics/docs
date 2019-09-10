
# Updating to use the new fork

**Use case**:  There is a fork of your app repo on BYU-Hydroinformatics Github. You would like to use this new fork locally. 

Before starting, make sure your local and remote repositories are up-to-date with all changes you need. 
____
Find the url to the fork that your created on BYU-HydroInformatics, for example: 
`https://github.com/BYU-Hydroinformatics/embalses.git`

Change the remote origin of your local repo to that of the BYU-Hydroinformatics Repo:
```
$ cd path/to/repo
$ git remote rm origin
$ git remote add origin https://github.com/BYU-Hydroinformatics/embalses.git
```

Pull all the code of the BYU-Hydroinformatics repo into your local repo.
```
$ git fetch
$ git checkout master
$ git pull origin master
```
That's it. Your future changes will be pushed to the correct repo. You are free to delete your existing repo from github or keep it around just in case. But please do not make any future updates to that repo. All updates should be sent to the repo on the BYU-Hydroinformatics Group. 


Once you have done this, update the Github URL on the app list : 
https://docs.google.com/spreadsheets/d/1NRS1QHoYUca3jKZgMAO2WAkkRGhP9qTUQ_3MDVgtuAU/edit#gid=2116302117
