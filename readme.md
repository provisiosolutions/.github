# Provisio .github template

The files in this repository serve as defaults for the organization if files of the same 
name/role are not present in a given repository.

Each file is written as generically as possible, so that its text can apply to any project 
with minimal editing.

## How to set up a Drupal project

To begin a Provisio Drupal project:

  1. Follow the usual procedure for making a site in Pantheon
  2. Clone the Pantheon repo to your local
  3. Come here to the Provisio github org and click the green button to make a new repo
  4. Make the repo to match the name of the project in Pantheon
  5. Set up your own local `.git/config` for the repo to match the template below
  6. Update the repo's readme.md to match the template below
  7. Push the code changes to master on the `origin` remote, which should push to both Pantheon and GitHub

### `.git/config` template

The two things that need to be placed here are the uuid assigned by Pantheon and the project 
name you assign the repo in github:

```
[core]
  repositoryformatversion = 0
  filemode = false
  bare = false
  logallrefupdates = true
[remote "origin"]
  url = ssh://codeserver.dev.uuiduuid-0000-uuid-uuid-uuiduuiduuid@codeserver.dev.uuiduuid-0000-uuid-uuid-uuiduuiduuid.drush.in:2222/~/repository.git
  url = git@github.com:provisiosolutions/name-of-project.git
  fetch = +refs/heads/*:refs/remotes/origin/*
[remote "pantheon"]
  url = ssh://codeserver.dev.uuiduuid-0000-uuid-uuid-uuiduuiduuid@codeserver.dev.uuiduuid-0000-uuid-uuid-uuiduuiduuid.drush.in:2222/~/repository.git
  fetch = +refs/heads/*:refs/remotes/pantheon/*
[remote "github"]
  url = git@github.com:provisiosolutions/name-of-project.git
  fetch = +refs/heads/*:refs/remotes/github/*
  ```

  The main idea behind this template, and something that's difficult (but not necessarily 
  impossible) using `git remote` is that origin pushes/pulls to both github and pantheon, 
  with separate remotes for pantheon and github so you can also deal with them separately.

  ### A Drupal readme template

  The following template can be used and extended to give new teammates instructions to onboard 
  quickly, and existing team members quick links and friendly reminders:

  ```
  # name-of-project site

This is a project for name-of-project using the standard Provisio Drupal installation.

## Project Links

These are links that may be useful to you, a developer on the project. If you'd like to add 
a frequently-used link, you absolutely should!

* [Existing site](https://example.com/)
* [Github repo](https://github.com/provisiosolutions/name-of-project)
* [Pantheon application](https://dashboard.pantheon.io/sites/uuiduuid-0000-uuid-uuid-uuiduuiduuid)
* [Teamwork PM](https://provisio.teamwork.com/app/projects/123456/tasks/list)
* [Wireframes](https://balsamiq.cloud/project-specific-path-here)
* [Designs](https://www.figma.com/file/project-specific-path-here)

## Installation and usage

Clone this repository from the primary github/pantheon remote, and `cd name-of-project` into it.

Follow these steps to get a lando-based local set up. A similar process can be used for ddev and other localhosts:

1. `lando start`
2. `lando composer install`
3. @todo

If this does not work, `lando info` should provide the required core name and so forth to make a successful connection.

### Streamlined git config

To connect to the two project remotes and fetch/push in a way that ensures the remotes do not fall out of sync, update
the repository's `.git/config` file to resemble the following:

\`\`\`
[core]
  repositoryformatversion = 0
  filemode = false
  bare = false
  logallrefupdates = true
[remote "origin"]
  url = ssh://codeserver.dev.uuiduuid-0000-uuid-uuid-uuiduuiduuid@codeserver.dev.uuiduuid-0000-uuid-uuid-uuiduuiduuid.drush.in:2222/~/repository.git
  url = git@github.com:provisiosolutions/name-of-project.git
  fetch = +refs/heads/*:refs/remotes/origin/*
[remote "pantheon"]
  url = ssh://codeserver.dev.uuiduuid-0000-uuid-uuid-uuiduuiduuid@codeserver.dev.uuiduuid-0000-uuid-uuid-uuiduuiduuid.drush.in:2222/~/repository.git
  fetch = +refs/heads/*:refs/remotes/pantheon/*
[remote "github"]
  url = git@github.com:provisiosolutions/name-of-project.git
  fetch = +refs/heads/*:refs/remotes/github/*

\`\`\`

With that config in place, and after a healthy `git fetch --all --tags`, the majority of push/pull work can be done on
the `origin` remote.

## Deploying changes to Pantheon

Once a commit has been made (or a PR has been merged from GitHub), deploying to Pantheon is straightforward:

  1. Either via terminus or the [Pantheon UI](https://dashboard.pantheon.io/sites/uuiduuid-0000-uuid-uuid-uuiduuiduuid#test/backups/backup-log), back up the environment(s) to which you'll be deploying
  2. `git pull github master` to your local to get all PRs and other changes on GitHub
  3. `git push origin master` to begin the deployment to dev, and then click the "Deploy Code from..." button in the yellow box of any additional environment's 'Deploys' tab when appropriate
  4. Click the 'Workflows' button in the upper-right-hand corner and wait for "Sync code on dev" or "Deployed code to..." to finish with black text. If the text is red, cast the necessary spells to un-red it next time
  5. Once the deployment is complete as reported by the Pantheon Workflows UI, `terminus drush name-of-site.ENV -- updatedb`
  6. `terminus drush name-of-site.ENV -- cr`
  7. `terminus drush name-of-site.ENV -- cim -y`
  8. `terminus drush name-of-site.ENV -- advagg-caf`
  9. `terminus drush name-of-site.ENV -- cr`
  10. Go to the site and load some pages and begin testing the deployed changes

## Development notes

In this section, add any notes related to local development setup, persistent known unfixable bugs, and other things to
ease onboarding and handoff.

* (replace with dev notes)

  ```

## Managing the project

Once the Pantheon environments are in place and the GitHub repo is set up, the team can 
start developing.

Each teamwork ticket, as long as it's big enough to justify its own ticket and not too big 
that it would be impractical to accomplish in a two-week span, should be its own feature branch: 
`git checkout -b t1234567-two-word-description`

Once work is completed on the feature branch, the developer should push the branch to github (
though in practice pushing to origin is good, with Pantheon warning you that feature branch 
names don't work for multidevs) and open a Pull Request against it.

The dev lead on the project then:

  1. Reviews the PR from the GitHub UI
  2. More often than not approves the PR, but could tentatively request changes or reject the PR
  3. Merges the approved PR into `master`
  4. Does a `git pull github master` locally
  5. If small formatting changes are warranted, performs those fixes locally
  6. Does a `git push origin master` to send the synced repo to both Pantheon and Github
  7. Does the deployment steps to the `.dev` environment (and perhaps more) per the repo readme
