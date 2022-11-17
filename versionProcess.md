# Versioning Process

# Branches

DEV -> NEXT -> MAIN

# Step by step
- Checkout to a new branch ( ex: feat/button ) from main
- make changes on branch, w/ conventional commits (feat(button): Did things)
- Create PR from branch into DEV ( This runs tests BUT not versioning)
- Create PR from DEV into NEXT ( This does a lerna pre-release version change )
- Create PR from NEXT into MAIN ( This does a graduate & creates a release )



# Commands

## On PR closed/merged into NEXT branch
npx lerna version --conventional-commits --conventional-prerelease  --no-private --exact 

## on PR closed/merged into MAIN
GH_TOKEN=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 npx lerna version --conventional-commits --conventional-graduate --create-release github -m "chore(release): publish" --no-private --exact


# Other Commands 

# When versioning packages independently, no placeholders are replaced
lerna version -m "chore(release): publish"
# commit message = "chore(release): publish

# Generating initial changelogs
# independent versioning
# (no root changelog)
npx lerna exec --concurrency 1 --stream -- 'conventional-changelog --preset angular --release-count 0 --commit-path $PWD --pkg $PWD/package.json --outfile $PWD/CHANGELOG.md --verbose --lerna-package $LERNA_PACKAGE_NAME'