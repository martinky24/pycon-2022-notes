# Pycon - Saturday, April 30

Major notes:

- `git add -p`
- "Modern Python" means add some sort of typing
- Look into `concurrent.futures` with more rigor
- Put a little bit of effort into understanding how to use asyncio WITHIN Flask
- Look into `Austin` and `Scalene` (profiling tools)

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

## Observability Driven Development

- Think of observability early, not as part of a post-mortem when something goes wrong
- LOGGING! Look into logging tooling?
- Good logging:
  - Write logs for a computer to process, not for a human to read, but make it so a human can read and understand
  - Put as much info as you can in python logging extra fields
  - Traceability - Request identifiers should be present in all log messages in order to be able to trace what happened in a particular request
    - Similar to how our "tasking" logging puts the ID of a task in each logging statement
  - Business traceability: Customer tracability should exist in all requests in order to measure the experience of a particular customer
  - Be intentional about log levels
  - **Personal Note**: Should we move all logging 1 level up in our obstask app? debug->info, run system at info. It's not that much logging?
  - **Note**: Look into the python logging `extra` keyword

## Moving Editable Installs Forward

- Contentious topic, hard to make forward progress in packaging
- Editable installs don't work with generated code
- Editable installs don't work with cython
- Editable installs don't work with including/excluding specified files very well

## Advanced Control Flow with Threads (as opposed to async)

[READ SLIDES](https://github.com/ajdavis/why-should-async-get-all-the-love)

- Asyncio introduced a lot of cool stuff... but a lot of the stuff they exposed was already possible with threads
- Threads and asyncio are 2 ways to do concurrency (concurrency is not parallelism)
- If you REALLY need parallelism, use multi-processing
- Concurrency: Dealing with events in *partial* order. E.g. waiting for data on many network connections at once
- Memory: Asyncio more efficient for *huge* (hundreds) number of tasks
- Speed: Async.io does not speed up code, it just allows concurrency. It often slows down code if i/o isn't a problem
- Compatability: Asyncio is incompatable with Flask/Django (personal note: I should look into this)
- Getting threads *right* can be difficult (locks, etc...)
  - `concurrent.futures` can help make threads less tricky. (Personal note: I should look into this/futures)
- Threads are not as good at cancellation as asyncio
- Personal note: I should go through the slides here

## Write faster Python! Common performance anti-patterns

[REPO](https://github.com/tonybaloney/anti-patterns)

- Faster code can come from a few big hammers:
  - Scale out infrastructure (MORE RAM!!)
  - Rewrite in Cython
  - Implement in PyPy
  - Optimize DB/IO
- Minor version python upgrades (3.11 has ~25% performance gain on 3.10)
- OR: Optimize existing code
  - DO:
    - Create a benchmark, measure existing code
    - Vary inputs on the benchmark
    - Isolate changes, 1 at a time, small and atomic
    - Reproduce impact 1000s of times. 1-off runs don't mean much.
  - DON'T:
    - Assume the impact will be the same against python versions
    - Make performance gain for <10% improvements (that's generally closer to noise)
  - Use a profiler
    - tracing profiler
      - pretty accurate
      - lots of overhead
    - sampling profiler
      - Lower overhead
    - Profilers:
      - austin (sampler, low overhead, line-level profiling)
      - scalene (sampler, low overhead, line-level profiling)
      - cprofile (buildin, funtion-level profiling)
  - [`perflint`](https://github.com/tonybaloney/perflint): Alpha tool to perhaps help with performance oriented linting

bad habits:

- Loop invarients
  - If something is never changed within a loop, don't evaluate it within the loop
  - This includes dict lookup
- Missing comprehensions
  - List comprehensions are faster than for loops if you're taking a list and creating another list, usually
- Inefficient data structures
  - NOTE TO SELF: Find his tree on when to use which data structure
  - Class, Dataclass, Dict, NamedTuple all have significant performance differences depending on the action
    - Consideration: How much mapping is there from source -> target?
    - Consideration: What operations are primarily being done? Iterating, Mapping, Sorting, Searching?
    - Consideration: Is the API important?
- Don't eagerly iterate iterables
  - don't wrap an iterable in `list()`
- Don't use lists if you're not changing the contents
  - Use a tuple
- Calling too-many functions
  - In python there is notable overhead to calling a function
  - His example for this is informative with `add`, `a()` and `b()`
  - Avoid "simple utility functions" in a hot loop if necessary. It sucks, but it may save some time. This leads to code repetition, so it's not great. But it can be used as a big hammer.
