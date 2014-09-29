# Use Case

You have repository A with remote location rA, and repository B with remote location rB. You want to preserve the commits of both repositories, but replace everything from A with the contents of B, and use rA as your remote location.

**Disclaimer**: I don't know if there is a better way to go about this, but I have done it successfully using the following method.

Before starting, make sure your local and remote repositories are up-to-date with all changes you need.

1. Change the remote origin of B to that of rA:
```
$ git remote rm origin
$ git remote add origin url_to_rA
```
2. Rename the local master branch of B:
```
$ git checkout master
$ git branch -m master-holder
```
3. Pull all the code of A from rA
```
$ git fetch
$ git checkout master
$ git pull origin master
```
4. Delete all the things! i.e, scrap everything from A
```
$ git rm -rf *
$ git commit -m "Delete yo."
```
5. Merge master-holder to the master branch.
```
git merge master-holder
```
`git log` should show all the commits from A, the delete commit, the merge commit, and finally all the commits from B.
6. Push everything to rA
```
git push origin master
```
Now your local copy of B has become a unified repository, which includes all the commits from A and B. rA is used as the remote repo. You no longer need your local copy of A or the remote repo rB (although keeping rB around for a bit longer isn't a bad idea).