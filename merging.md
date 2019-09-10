
# Merging with existing fork

**Use case**:  There is a fork of your app repo on BYU-Hydroinformatics Github. You would like to merge all your changes from your local repo to the one that is on BYU-Hydroinformatics github. 

Before starting, make sure your local and remote repositories are up-to-date with all changes you need. 
____
Find the url to the repo that exists on BYU-HydroInformatics, for example: 
`https://github.com/BYU-Hydroinformatics/embalses.git`

Change the remote origin of your local repo to that of the BYU-Hydroinformatics Repo:
```
$ cd path/to/repo
$ git remote rm origin
$ git remote add origin https://github.com/BYU-Hydroinformatics/embalses.git
```

Rename the local master branch of your repo:
```
$ git checkout master
$ git branch -m master-holder
```

Pull all the code of the BYU-Hydroinformatics repo into your local repo.
```
$ git fetch
$ git checkout master
$ git pull origin master
```
Now the master branch of the BYU-Hydroinformatics repo is *master* in your local repo. The old master of your local repo is *master-holder*.

Merge *master-holder* into *master*.
```
git merge master-holder --allow-unrelated-histories
```
`git log` should show all the commits from BYU-Hydroinformatics repo, the delete commit, the merge commit, and finally all the commits from your local repo. 

Push everything to the new repo
```
git push origin master
```

Once you have done this, update the Github URL on the app list : 
https://docs.google.com/spreadsheets/d/1NRS1QHoYUca3jKZgMAO2WAkkRGhP9qTUQ_3MDVgtuAU/edit#gid=2116302117
