Git Workflow
============

This is a sample repository to demonstrate a git workflow.

This workflow is based on a agile sprint model, where there will be (bi-)weekly releases to production with a number of features that have been completed & acceptance tested during the sprint.

Primary Branches
----------------

There are two primary branches in the repository:

- `master`
- `development`

The `master` branch will always reflect what's on production. No exceptions. Pushing to the `master` branch will trigger a deploy directly to production.

All completed features will be submitted to the `development` branch via pull requests. It will auto-deploy to the staging server.

Create a Feature or Bug Fix
---------------------------


Branch off: `development`  
Merge Into: `development`  
Naming: `<initials>-<branch name>`

For non-urgent features or bug fixes, branch off the `development` branch and create a feature or bug fix branch:

    git checkout development
    git pull
    git checkout -b <initials>-<branch name>

From within your feature branch, rebase regularly to incorporate the latest changes from `development`:

    git fetch origin
    git rebase origin/development

Resolve conflicts & create commits as you go along. When your feature is ready for merging into `development`, squash all your commits into one using `git rebase -i`.

Share your branch.

    git push origin <branch name>

[Create a pull request] from this branch and request a code review. **Make sure you create a PR that merges into the `development` branch, not the `master` branch**.

Your PR should merge INTO `development` FROM your feature branch.

[Create a pull request]: https://help.github.com/articles/using-pull-requests/

Merging in a Feature
--------------------

Once you've fixed any issues in your PR, make sure to squash commits into one and force push your branch.

When you've gotten final approval to merge, merge the feature branch into `development`

    git checkout development
    git merge --ff-only <initials>-<branch name>
    git push origin development

Once this is done, delete your branch.

    git push origin --delete <initials>-<branch-name>
    git branch --delete <initials>-<branch-name>

End-of-Sprint Release
---------------------

The `development` branch is now full of tested features that are ready for deployment. To release, create a pull request that will be merging the `development` branch into the `master` branch.

Your PR should merge INTO `master` FROM `development`.

Set the title of the PR to be:

    YYYY-MM-DD Release

In the description of the PR, you can add details like:

- Features in the PR
- Trello Cards
- Airbrake Notifications

This can act as the weekly release report. Once the report is complete, merge the branch into master:

    git checkout master
    git merge --ff-only development
    git push origin master


Hotfixes: Urgent Production Issues
----------------------------------

Branch off: `master`  
Merge Into: `master` and `development`  
Naming: `<initials>-hotfix-<branch name>`

For issues that cannot wait until the end of a sprint cycle, create hotfix branch from the `master` branch:

    git checkout master
    git pull
    git checkout -b <initials>-hotfix-<branch name>

Add your fix and rebase from `master` regularly to keep up to date. Once your fix is complete, squash your commits into one and submit another pull request. This time, make sure you are merging into the `master` branch.

Your PR should merge INTO `master` FROM your hotfix branch.

Once you've gotten approval, merge your hotfix branch into the `master` branch.

    git checkout master
    git merge --ff-only <initials>-hotfix-<branch name>
    git push origin master

Merge your hotfix branch into development as well.

    git checkout development
    git merge --ff-only <initials>-hotfix-<branch name>
    git push origin development

Once this is done, delete your branch.

    git push origin --delete <initials>-hotfix-<branch-name>
    git branch --delete <initials>-hotfix-<branch-name>