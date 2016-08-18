# contributing-guidelines

## Project Structure
Please refer to the sample project adapted to the project developed (list to be completed):
  - node microservice: https://github.com/dial-once/sample-node-microservice

## Coding rules
`node.js`: https://github.com/dial-once/javascript

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
Tests: mocha, istanbul for coverage 

Logs: winston 

Array / Object manipulation: native 

Bug report: bugsnag 

Docker: base image is `dialonce/nodejs:latest`. If you use dynamic loaded c++ node modules, you can use `dialonce/nodejs:dynamic`.

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

### Branches
We follow Vincent Driessen method: http://nvie.com/posts/a-successful-git-branching-model/ (gitflow)  
Prefix your branch name with the type of modification you are doing.

```
feature/my-feature-name
bugfix/bug-description
test/my-test-case
refactor/description
perf/my-perf-improvement
```

### When to rebase
**Rebase your branch before merge, and before merge only** to clean things up. Keep your commit history, it will be useful and you will maybe need to rollback/delete things while your feature is not ready.
Then you can clean things up by using squash/fixup, you can have multiple commits when merged but it have to be working state commits.

### Pull Requests
Create a branch to base your feature on, `git checkout -b feature/my-branch`.

Make your changes respecting our coding rules, execute tests and when your feature is ready, create your PR on Github on the develop branch.

Dial Once interns create branches on the main repo, contributors must fork the project.

#### Pull Requests - dev workflow
 - Work on a branch
 - Do your commits the way you want - atomic commits are advised
 - Open a PR if you want to have a discussion or submit your code for review
 - Assign yourself on the PR if the PR is not done yet and require some intervention of your part (cleanup before merge, etc.)
 - Assign the persons that you want to get a review from 
 - Assignees remove themselves once reviewed and LGTM'd
 - Once branch ready to merge (LGTM green or explicit comment from a reviewer) perform any needed cleanup and remove yourself if you are assigned
 - PR with status green and no assignee car be merged anytime by anyone (if you need it, LGTM'd and green, merge it) - Otherwise the latest assignee can merge

Then in Github PR, you can check only PR where you are assigned: it will simplify your work so you know instantly if you have some work to do / some review requested by someone

### Deployment pull request (develop > master)

`release/vx.x.x > (master|develop)` pull requests are deployment pull requests: they should be named using semver and the last commit on the branch before merge should be tagged with the version name (use `git tag -a`).

Example or Pull Request commits, PR name is `release/v0.2.0`:
```
 - Fix bad typo on comment for device register method
 - Add a new route for phone number validation (reserved to India)
 - Fix weird behaviour on something
 - v0.2.0 (Tag here)
```
See gitflow: http://nvie.com/posts/a-successful-git-branching-model/

### Testing a microservice

There is two things about testing on our current environment:
  - We use some staging services (broker)
  - The microservice may use some database

### Write testable code
When you code, think about your tests. You will then make a very readable and testable code. The best thing of all is to use TDD so you will create a nice API and manage all the cases that you want to manage.
If you write your code before the tests, it may need difficult changes to make it testable without hack.

#### Datasource
Each service must have independant data source. To ensure this is the case, you must spawn your local DB in your test suite, provision it, run your tests using it, and then destroy it.

#### Staging broker usage
You can use the staging broker in your tests if necessary (if it can be avoided, best to do it!), but make sure your subscriber queues are marked as non-persistent (durable is set to false) when your tests are running (so the broker is cleaned up after your tests). On `node-bunnymq`, this can't be done yet: https://github.com/dial-once/node-bunnymq/issues/51

As there is no real value of testing non-RPC calls (you don't even know if you call it correctly :x), you can mock those.



