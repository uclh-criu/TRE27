# Workflow Documentation

## Rationale
This Repository represents the UCLH implementation of the [Microsoft Azure TRE](https://github.com/microsoft/AzureTRE.git). The repository should allow new features from the Microsoft Azure TRE to be incorporate, whilst preserving any local customisations.


# Branching Policy
All code is developed using a [GitFlow Branching Policy](https://datasift.github.io/gitflow/IntroducingGitFlow.html).
The main features of this workflow are:
1. New features are built in ***feature branches***
2. ***Feature branches*** are branched out of the ***develop branch***
3. When features are finished they are merged back to the ***develop branch***
4. When it is time to make a release, a ***release branch*** is created off of the ***develop branch***
5. The ***release branch*** is deployed to a suitable test environment and tested. Any required fixes are deployed directly in the ***release branch***
6. When the ***release branch*** is finished it is merged into the ***main and develop branches***
7. The ***main branch*** tracks released code only. The only commits to ***main*** are from the ***release branches*** and ***hotfix branches***
8. ***Hotfix branches*** are used to create emergency fixes. They are branched directly from tagged releases in the ***main branch***
9. When finished ***hotfix branches*** are merged directly into both ***main and develop branches***

# Local Customisations
The ***main*** branch represents the code used to build the production environment. New features are added as follows:
1. create a **develop** branch from ***main***:
```
git checkout main
git checkout -b develop
```
2. Create a new branch from develop for the feature:
```
git checkout -b feature123
```
3. Add some code and push the changes
```
git push -u origin feature123
```
4. Run the CI pipeline (DEV environment)
5. If all tests pass these changes can be pushed to develop:
```
git checkout develop
git merge feature123
git push -u origin develop
```
6. Run the CI pipeline (TEST environment)
7. If all tests pass, these changes can be pushed to the release branch:
```
git checkout -b release
```
8. When your ready to release the feature, push the changes to main:
```
git checkout main
git merge release
git push -u origin main
```
9. Before starting the next release, delete **develop**, **feature123** and **release**
```
git branch -d develop
git push --delete origin develop
git branch -d feature123
git push --delete origin feature123
git branch -d release
git push --delete origin release
```

# Incorporating Changes from the Microsoft TRE

1. Create a new develop branch from main
```
git checkout main
git checkout -b develop
```
2. Create a new branch for the Microsoft Updates
```
git checkout -b ms_main
```
3. Fetch the latest commits from the Microsoft Azure TRE project
```
git fetch msr main
```
4. Rebase ms_main to msr/main
```
git rebase msr/main
```
5. Run CI/CD pipeline (DEV environment)
```
git push -u origin ms_main
```
6. If testing passes push changes to develop
```
git checkout develop
git merge ms_main
git push -u origin develop
```
7. Run CI/CD pipeline (TEST environment)
8. If testing passes, push the changes to main
```
git checkout main
git pull
git merge develop
git push -u origin main
```
9. Before starting the next release, delete **develop**, **ms_main**
```
git branch -d develop
git push --delete origin develop
git branch -d ms_main
git push --delete origin ms_main