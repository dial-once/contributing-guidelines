# contributing-guidelines

## Project Structure
Please refer to the sample project adapted to the project developed (list to be completed):
  - node microservice: https://github.com/dial-once/sample-node-microservice

## Coding rules
### node.js

  - Version your packages using semver
  - Wrap your code under 100 characters / line and avoid too large files
  - 2 spaces indentation (no tabulation)

#### Code style

```js
var aVariable;
function aFunctionName();
var Object = () => {}; //avoid defining complex objects like this until we are ES6
```

Instead of complex inheritance hierarchies, we prefer simple objects. We use prototypal inheritance only when absolutely necessary.

Use the following .jshintrc for your project, it should cover most cases (tests, node, etc.) :
```json
{
  "node": true,
  "mocha": true,
  "curly": false,
  "quotmark": "single",
  "maxcomplexity": 8,
  "unused": true,
  "globals": {
	"after": true,
	"assert": true,
	"before": true,
	"describe": true,
	"expect": true,
	"it": true
  }
}
```

One-liners are allowed when they provides a better readability:
```js
if (testCase) return true;
```

#### File naming convention
```js
my-file.js //yes!
myFile.js //no!
myfile.js //no!
MyFile.js //no!
my_file.js //no!
```

## Code quality
All projects are analysed by SonarQube, requires A-grade technical debt,  and at least 80% of functional coverage
All external faced projects must have up to date dependencies and maintainer should subscribe to security notification for his project Snyk

## Documentation
Provide one or multiple use case examples in example/ folder
Provide some basic info in README.md about usage, things to know, maintainer, and…
ADD BADGEEEES (so we can track deps, version, code quality, build status in a single check) in the README.md

## Badges
Node.js: Code quality, coverage, build status, npmjs, snyk
Docker: tags, size

## Dependencies
Usage of libs not included for a set of tools described below is forbidden (ie: in node.js, usage of bunyan to log, or usage of bluebird for promise support).

## Tools
### node.js
tests: mocha, istanbul for coverage 

logs: winston 

Array / Object manipulation: lodash or native 

Bug report: bugsnag 

Docker: base image is dialonce/nodejs:latest

## Note about @todos
Just don’t do it. Work on a branch and create a PR once the work is completely done.
Worst case scenario: Add an issue if you really need to keep track of something you don’t have time to do. 

## Git usage guidelines
### Commit messages
Commits must be atomic and commit message must be short and specific. Detailed comments goes after a line break.
```
Fixes the logs sent to LogEntries on device Register

This commit fix the logs sent when the device does not provide his phone number and registers since more than a week, the register date was not provided.
```

### Commit strategy
Each commit represent a fix, a feature, an improvement, dependency upgrade, etc. You can’t have two fix, two feature, two something into a single commit (see previous section: atomic and specific). 
Please note that a fix on a fix is not a second fix. A fix on a fresh feature is also not a fix: it is part of the feature. So please use git rebase (edit, squash, skip will help you a lot).

Example of bad commits for a feature:
```
 - Add the new device registration method
 - Fix lint issue
 - Fix typo in comment for device registration
```

While it should be:
```
  - Add the new device registration method
```

Why:

The lint issue commit should never have been there: squash it into the previous commit. Same thing for the comment: squash. In this example there should be only 1 commit. If you already pushed, that’s not a problem: fix it and force push. If you are on develop: throw a cookie jar and ask to everyone working on the project if it’s ok if you force push (if less than 1 minute you can do it discretely :D).

### Branch names
Prefix your branch name with the type of modification you are doing.

```
feature/my-feature-name
bugfix/bug-description
test/my-test-case
refactor/description
perf/my-perf-improvement
```

### Submitting a Pull Request
Create a branch to base your feature on, `git checkout -b feature/my-branch`.

Make your changes respecting our coding rules, execute tests and when your feature is ready, create your PR on Github on the develop branch.

Dial Once interns create branches on the main repo, contributors must fork the project.

### Deployment pull request (develop > master)
`develop > master` pull requests are deployment pull requests: they should be named using semver and the last commit on the branch before merge should be tagged with the version name (use `git tag -a`).

Example or Pull Request commits, PR name is v0.2.0:
```
 - Fix bad typo on comment for device register method
 - Add a new route for phone number validation (reserved to India)
 - Fix weird behaviour on something
 - v0.2.0 (Tag here)
```


