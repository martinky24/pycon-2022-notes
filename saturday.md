# Pycon - Saturday, April 30

## Opening Keynote #1: Sara Issaoun

- Event Horizon Telescope team, worked to take a famous image of a black hole
- Not a lot of Python talk (didn't mentioned it once), all very interesting astronomy stuff though

## Opening Keynote #2: Peter Wang

- co-founder of Anaconda
- Pushing for greater python tooling in the web browser
- Came up with something called `pyscript`, which hopes to create python apps in the browser. Not sure if `pyscript` is released yet

## How to use and how NOT to use code quality metrics?

- More of a PR talk for their company [Sourcery](https://sourcery.ai/)
- Maybe worth investigating a little, but little immediate scientific value / lessons learned here
- "Simple code is better"
- "less evaluation code is better"
- **Useful**: "high code coverage doesn't imply good tests, but low test coverage usually implies bad tests"

## Making Data Classes Work For You

- Similar to James Murphy's videos on data classes: [vid1](https://www.youtube.com/watch?v=vBH6GRJ1REM), [vid2](https://www.youtube.com/watch?v=vCLetdhswMg)
- Talked a bit about how to use data classes with input verification

## Coding With Others

Basic things that help bring code to a high production level:

- Documentation
  - README!!!! Fill it out
    - What project does
    - How to contribute
    - How to install
  - If possible, make docs accessible via browser
- Version control
  - git, a must for codebase with multiple people
  - Keep commit messages useful. Commit messages are documentation of code history
  - Consider using the "accepted" format of commit messages (first line brief description, then blankline, then long message talking about the commit)
  - `git reset HEAD~` - undo last commit, while preserving changes in working directory. Useful for splitting up a commit.
  - `git add -p/--patch` - lets you select individual lines / chunks of lines to add for a commit **IMPORTANT**
  - `git cherry-pick [hash]` - take a commit from one branch to another
  - `git rebase -i [REF]` - take branch, snap it on the end of main, as if you did it all in one day (I need to do better to understand this)
- Code quality tools
  - LINTERS! Pylint, Flake8
  - FORMATTERS!
    - Narrow scope: `isort`, `2to3`, `pyupgrade`
    - Comprehensive: `black`, `yapf`
    - New tools: `usort` (`isort` replacement), `ufmt` (bundles `usort` with `black`)
  - Where to run these tools?
    - Standalone `black src/` - good to have, should be automated
    - IDE/Editor
    - You can run with tests
    - Continuous integration steps
    - **DO THIS**: `pre-commit`
- Pull requests
  - Code review, feedback from others
  - Knowledge sharing
  - TO READ: [Reviewing Pull Requests](https://chelseatroy.com/2019/12/18/reviewing-pull-requests/), [Submitting Pull Requests](https://chelseatroy.com/2019/12/13/async-collaboration-1-submitting-pull-requests/)
- Dependency Management
  - Tools to make sure that our code has everything it needs to run properly
  - Recommends [`poetry`](https://python-poetry.org/)
  - Recommends [this docker tutorial](https://pythonspeed.com/docker)
