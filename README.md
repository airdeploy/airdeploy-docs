
### Description

This is the Airdeploy Documentation website project.

It's built on [Docusaurus](https://docusaurus.io) framework

`Master` branch is for the source files

`gh-pages` branch is for github pages branch. This branch is autogenerated by deploy command and must not be touched 
manually. It consists of html & css & js code

### Contribution

You should edit files in `docs` folder. Docusaurus uses them to generate new versions of the documentation. If you want, 
you can manually change previous versions from the `website/versioned_docs` folder.

#### Adding a new section

1. Create a file with .md extension and put it to the docs sub-folder you need. File header pattern:

```markdown

---
id: filename_without_extesion
title: title
sidebar_label: sidebar_label  
---
```

2. Update `website/sidebars.json` file with a reference to your new file 
3. Restart/`yarn start` from `website` dir to verify your changes, because you change the structure. You don't need to restart server when you are just editing a file
4. Create a PR to a `master` branch with the desired changes and request a review

### Developing

`cd website` 

There is a README.md file which describes on how to run docusaurus locally 

### Deploy

#### Version update

Before publishing, make sure you updated the version
Do the following:

```commandline
yarn run version x.x.x
```

where "x.x.x" is desired **new** version
The command above generates new version in `website/versioned_docs` folder  

#### Changing the current version
Make sure you always make changes in `docs/` folder, not in `versioned_docs` folder. 
`docs` is the source and versioned is auto-generated files

Let's say the current version is `1.0.0` 

It's not recommended changing the current version. If you still want to do this you need to:
 - delete the current version from a folder `website/versioned_docs/version-1.0.0`
 - edit `website/versioned_sidebars/version-1.0.0-sidebars.json` and remove "1.0.0" from the array
 - generate the current version: `yarn run version 1.0.0`
 
#### Changing the previous versions

It's not recommended, but if you need to make change in previous version:
- open a directory with the version you want to change `website/versioned_docs/version-x.x.x`
- Make a change
- Make sure a change is reflected to all versions above  

#### Publishing 

> You must publish only `master` branch 

```commandline
cd website

GIT_USER=<GIT_USER> \
  CURRENT_BRANCH=master \
  USE_SSH=true \
  yarn run publish-gh-pages
```

This command will build html,css,js and push it to the `gh-pages` branch

More info:
https://docusaurus.io/docs/en/publishing#using-github-pages