**Getting started**
1. Make a fork of the original open-source repo
2. Clone fork in to local environment
3. Make a feature branch and develop
4. Commit and push the feature branch to fork's origin
5. If everything looks good in the fork, make a pull request to the original repo from fork's feature branch to open-source repo's main or dev branch

**PR for Github Actions CI/CD Job Itself**
1. Same process as "Getting started" for the feature branch
	1. Ensure that the Github Action Job we're making for the PR will run in the fork's repository by configuring branch settings in the yaml file. We'll make another change later before actually opening the PR
2. Make two test branches
	1. branch meant for passing CI/CD
	2. branch meant for failing CI/CD
3. Make a PR in the fork repository from the test branches to the feature branches and Github Action will run in the fork repository
4. If everything looks good, remove the test branch configurations from the Github Action Job yaml file and only include the main branch
5. Make a PR with the cleaned feature branch and link the two tests for the code reviewers to see