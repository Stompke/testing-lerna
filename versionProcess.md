# Versioning Process


# ideas
- i think you can publish alpha without creating tags using `lerna publish --canary`

# Branches

DEV -> NEXT -> MAIN

# Step by step
- Checkout to a new branch ( ex: feat/button ) from main
- make changes on branch, w/ conventional commits (feat(button): Did things)
- Create PR from branch into DEV ( This runs tests BUT not versioning)
- Create PR from DEV into NEXT ( This does a lerna pre-release version change )
- Create PR from NEXT into MAIN ( This does a graduate & creates a release )



# Commands

1
## Publish Canary (alpha) build w/o adding git tag or changing package.json
### Publish canary (alpha) to npm
npx lerna publish --conventional-commits --canary

2
## on PR closed/merged into MAIN
### Bump lerna version & create github release tag
GH_TOKEN=MYTOKEN npx lerna version --conventional-commits --create-release github -m "chore(release): publish" --no-private --exact

3
## on PR closed/merged into MAIN
### Publish new stable build to NPM
npx lerna publish from-package --conventional-commits



    




# Other Commands 

# When versioning packages independently, no placeholders are replaced
lerna version -m "chore(release): publish"
# commit message = "chore(release): publish

# Generating initial changelogs
# independent versioning
# (no root changelog)
npx lerna exec --concurrency 1 --stream -- 'conventional-changelog --preset angular --release-count 0 --commit-path $PWD --pkg $PWD/package.json --outfile $PWD/CHANGELOG.md --verbose --lerna-package $LERNA_PACKAGE_NAME'



# Scenarios

## TRY #1 Changes ONLY made to link icon (not dependant & no dependencies)

> Changes to link.js
> git commit -m 'feat(link): Added new text to link.'
> Changes to link.js
> git commit -m 'fix(link): added a comment.'
> npx lerna version --conventional-commits --conventional-prerelease --no-private --exact 

    Changes:
        - link: 1.1.0 => 1.2.0-alpha.0

> GH_TOKEN=MYTOKEN npx lerna version --conventional-commits --conventional-graduate --create-release github -m "chore(release): publish" --no-private --exact

    Changes:
        - link: 1.2.0-alpha.0 => 1.2.0

..... GIT said bad credentials & didnt create release tag, but it still changed versions and created a tag. 

Made the tag a release manually



## TRY #2 Changes ONLY made to link icon (not dependant & no dependencies)

> branch fix/link
> changes to link.js
> commit -m 'fix(link): stuff'
> PR squash merged into master w/ bug label
> back to master and run
> npx lerna version --conventional-commits --conventional-prerelease -m "chore(release): pre-release"  --no-private --exact 
    Changes:
    - link: 1.2.0 => 1.2.1-alpha.0
> GH_TOKEN=MYTOKEN npx lerna version --conventional-commits --conventional-graduate --create-release github -m "chore(release): publish" --no-private --exact
    Changes:
    - link: 1.2.1-alpha.0 => 1.2.1
> Made a release tag w/ these comments in the body (lame)
    1.2.1 (2022-11-17)
    Note: Version bump only for package link
> and change log looks like this
    Change Log
    All notable changes to this project will be documented in this file. See Conventional Commits for commit guidelines.

    1.2.1 (2022-11-17)
    Note: Version bump only for package link

    1.2.1-alpha.0 (2022-11-17)
    Bug Fixes
    link: (#7) (7c8993e)


## TRY #3 Changes ONLY made to link icon (not dependant & no dependencies)  w/ NO ALPHA STUFF (see if changelogs & release tags look better)



> branch feat/link
> git commit -m 'feat(link): added features to link.'
> Squashed feat/link PR w/ enhancement tag
> Squashed fix/link PR w/ bug tag
> GH_TOKEN=MYTOKEN npx lerna version --conventional-commits --create-release github -m "chore(release): publish" --no-private --exact
    Changes:
    - link: 1.2.1 => 1.3.0
> Link changelog looks good 👍
    Change Log
    All notable changes to this project will be documented in this file. See Conventional Commits for commit guidelines.

    1.3.0 (2022-11-17)
    Bug Fixes
    link: Added text to fix a bug. (#9) (e500143)
    Features
    link: Added things to link (#8) (a33a7b7)
> RELEASE TAG looks good 👍
    link@1.3.0 Latest

    1.3.0 (2022-11-17)
    Bug Fixes
    link: Added text to fix a bug. (#9) (e500143)
    Features
    link: Added things to link (#8) (a33a7b7)


## TRY #3 Changes to Link & Button  w/ NO ALPHA STUFF (see if changelogs & release tags look better)

> changes on feat/link-newthings and merged into master
> changes on fix/link-stuff and merged into master
> changes on feat/button-things and merged into master
> Create a release (skipping pre-release)
> GH_TOKEN=MYTOKEN npx lerna version --conventional-commits --create-release github -m "chore(release): publish" --no-private --exact

> version changed on: 
    link
    button
    icon

> Created Good changelogs 👍
> Created good release tags 👍



## TRY #5 using try using pre-releases
> changes, commit and git push origin feat/new-stuff-on-link & merged into master
> 




## try #6 skip pre release changelogs

> feat/button
> merged into master
> npx lerna version --conventional-commits --conventional-prerelease -m "chore(release): pre-release" --no-changelog  --no-private --exact
    Changes:
    - button: 2.2.0 => 2.3.0-alpha.0
    - icon: 2.2.1 => 2.2.2-alpha.0
> git tags were made but no changelogs were modified
> GH_TOKEN=MYTOKEN npx lerna version --conventional-commits --conventional-graduate --create-release github -m "chore(release): publish" --no-private --exact
    Changes:
    - button: 2.3.0-alpha.0 => 2.3.0
    - icon: 2.2.2-alpha.0 => 2.2.2
> versioning worked, but since changelogs were skipped on pre-release (2.3.0-alpha.0), no changes were shown in  changelog files.

## try #7 skip pre release changelogs and git tags

> feat/link-stuff branch merged into master
> lerna publish --canary
    Found 1 package to publish:
    - link => 1.5.1-alpha.3+cb1d9da
> my npm not setup