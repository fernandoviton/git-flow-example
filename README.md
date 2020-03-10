# git-flow-example
This repo is an example for using [gitflow](https://www.atlassian.com/git/tutorials/comparing-workflows/gitflow-workflow) as a workflow for a team project.

This summary from the link above describes a summary of gitflow. The overall flow of Gitflow is:

    1. A develop branch is created from master
    2. A release branch is created from develop
    3. Feature branches are created from develop
    4. When a feature is complete it is merged into the develop branch
    5. When the release branch is done it is merged into develop and master
    6. If an issue in master is detected a hotfix branch is created from master
    7. Once the hotfix is complete it is merged to both develop and master


## Experimenting with similar workflow
Here we will take aspects of gitflow and adjust to minimize steps and number of branches needed.

Most importantly, master will be where releases happen from.  External to this flow we can actually choose which build out of master we will release.  So while master needs to stay clean, it doesn't not immediately get released.

### Feature branches
We use feature branches the same a gitflow.
```
# In develop, at commit 76b65dc4d4b6747aa02459a45447bec780c4be13

# Make a branch and make changes in it
git checkout -b feature1
echo "a sample added in feature1" > sample-feature.txt
git commit -am "Commit from feature1"

# Go to develop and merge the changes from above (or does this through a PR)
# Optionally do a squash commit to remove feature branch history, this can be done as part of PR
git checkout develop
git merge feature1
git push origin head
```

### Merge develop in to master
```
# In master, at commit 76b65dc4d4b6747aa02459a45447bec780c4be13
git merge develop
git push origin head
```

### Hotfix branches
We will use hotfix branches as the only way to service master.

Per gitflow, when a change is done in a hotfix branch it is merged back to both master and develop.

```
# In master, at commit 46ee7036c25bb78a7db6a9d7ac1227780fffd27e

# Make a branch and make changes in it
git checkout -b hotfix1
echo "a sample added in hotfix1" > sample-hotfix.txt
git add .
git commit -am "Commit from hotfix1"
git push origin head

# Merge this to master
git checkout master
git merge hotfix1
git push origin head

# Merge this to develop
git checkout develop
git merge hotfix1
git push origin head
```

## Merge develop to master after a hotfix
Since now we have the same change applied to master and develop, let's see how subsequent changes on top this change are handled at merge time

```
# In develop, at commit 5bf519acf25aa87272776991ff89c4f0d6980cff

# make a change to the same line as was changed in the hotfix example - 27e87c4bf49263d43eab28edebe3762aa278b530 is the example that was commited for this example
git checkout master
git merge develop
git push origin head
```
This completes without conflict.

## Merge develop to master after a squashed hotfix
We will simulate sqush by making the same hotfix change in both develop and master but without using merge to bring it in.

# 27e87c4bf49263d43eab28edebe3762aa278b530


