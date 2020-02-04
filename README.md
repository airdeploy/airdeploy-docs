
### Description

This is the Airship Documentation website project.

It's built on [Docusaurus](https://docusaurus.io) framework

`Master` branch is for the source files

`gh-pages` branch is for github pages branch. This branch is autogenerated by deploy command and must not be touched 
manually. It consists of html & css & js code

### Contribution 

Create a PR to a `master` branch with the desired changes and request a review.

### Developing

`cd website` 

There is a README.md file which describes on how to run docusaurus locally 

### Deploy

Right now publishing new version is done manually only via bash command using github pages

You must publish only `master` branch 

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