# Stwilz.io
The blog and portfolio of Steve Wilcox

## Prerequisites
* [NodeJs](https://nodejs.org/)
* [Metalsmith](http://www.metalsmith.io/)

## Installation
* `npm install`

## Build instructions
  * `metalsmith`

## Git workflow
When committing new feature's please ensure that all commit's only contain the relevant files for the particular feature and that the commit message clearly describes it's purpose.

When develop is ready to publish please bump the version of the package.json file, compile the approved build to `dist` and run the following command to push to `master`.
```
git subtree push --prefix dist origin master
```
When the commit is complete please tag the release in git and add release notes outlining what has been updated.