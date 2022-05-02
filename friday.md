# Pycon - Friday, April 29

Major Notes:

- We should be using typing more. 3 reasons: Typing, correctness (if used with tooling like MyPy), and adding typing drives keeping functions simpler and more correct (in terms of simple, consumable input/output)
- Structural pattern matching is a really cool and powerful addition worth learning, not available util 3.10 or so
- Investment in infrastructure is essential to drive software efficiency across the organization, no way around it

## Opening Keynote

Main takeaways

- I should be actively adding typing to more code I write
  - Maybe not for mypy integration (yet), but because adding typing is low overhead tooling to help improve self-documenting code
  - If something is hard to type, you probably need to refactor it

- Personal: I need to focus more on "single type" outputs of functions, if it's impossible to output something of that type, an exception is reasonable. Less instances of outputting `None` if something can't happen.

## Python Oddities

- General notes on variables, mutability, data structures, operators "gotchas"
- Scope abuse
- immutability vs. mutability abuse
- `is`/`id` "gotchas"

```python
 x = ([1, 2],)
x[0] += [3, 4]
```

## Structural Pattern Matching

### Clerical

- Implemented in Python 3.10
- [Pep-634](https://peps.python.org/pep-0634/): Written for core devs
- [Pep-635](https://peps.python.org/pep-0635/): Written for steering council
- [Pep-636](https://peps.python.org/pep-0636/): Tutorial, written for consumers. **This is what is important to read if anything!**
- Early prototypes implemented in [gvanrossum/patma](https://github.com/gvanrossum/patma)

### Technical

- Pattern matching combines control flow and destructuring

```python
match meal:
    case entree, side:
        ...
    case _:
        ...  # fall through
```

- pretty powerful for runtime type parameterization

### Personal Notes

- Pretty powerful, similar to what's in Mathematica but not as wide-spread
- Would like to learn to use well, but not until 3.10 so will be a while before we use
- To look at this further, explore implmenting a red/black tree structure using pattern matching `match`/`case` statement

## Python in the Enterprise

- Stabalize what one can
- Figuring out packaging and deployment
- Establish "official" documentation
- Problems
  - Variations in python versions
  - Building up infrastructure around C++ (non-Python) tooling
- **Deployment** comes up again and again
- All "internal" projects were open to all engineers, to encourage transparency, knowledge sharing, and forward progress
- People from outside the infrastructure team opening issues/pull requests against infrastructure is encouraged
- Worked on replacig core infrastructure with python packages
  - supporting python idioms
  - optimizing performance
  - high level of reliability
- At Bloomberg, Python is the leading contender for new application development

<!-- markdownlint-disable-next-line MD024 -->
### Personal Notes

- Emphasis on investment in infrastructure to make Python work
- They mention that core infrastructure is important to be well maintained (and carefully implemented). Invest in it. Otherwise it only slows down people downstream. Don't move too fast...

## Implementing a Pipe Operator in Python

Fun how-to on going from cpython source code to interpreted python magic. Not a lot that immediately relates to my day to day, but fun perspective on the language.

## [What to do when the Bug is in Someone Else's Code](https://pganssle-talks.github.io/pycon-us-2022-upstream-bugs/#/)

(slides in link above worth looking at)

- There are risks in dependencies - one of them being a bug in someone else's code
- First goal is to see if you can fix it. Find the source, open a PR. Maybe viable, maybe not. Depends on release cycles, etc...
- Rewrite/wrap the function yourself. If possible. Technical debt, once its fixed in a new release you don't want to keep using your implementation.
- Monkey patching. Most functions can be overwritten in some way. Very dangerous if not careful (and even if done carefully), but if required it is a tool for the toolbox.
  - Scope problems
  - Lots of room for unintended consequences
- Store vendor code in your own source, and make the modifications yourself.
  - hacky
  - hard to implement
  - hard to maintain
- Forking
  - Maintaining a fork that upsteam doesn't know about
  - No guarantees of future compatability
  - Update all your patches adds friction to the upgrade process

## When to Refactor your Code into Generators

(wasn't focusing well on this talk)

- Generators are a worthy tool to add to the toolbox. Effective use can do a lot to aid in refactoring and minimizing slightly different code duplication
- `itertools` provides tooling and utilities to further improve the story here

## Effective Protobuf

- Do not write your own serialization code.
- When considering a (not home-made) serialization scheme, consider:
  - portable (multi-language, multi-machine) vs. non-portable
  - dynamic vs. static typed
  - Textual vs. binary
- Popular serialization frameworks:
  - JSON, widely adopted, decent performance, very portable
  - Pickle, decent performance, not portable
  - Protobuf
    - Highly portable (lots of support across popular languages)
    - Static typing (easy to document serialization)
    - Great performance (static typing, binary, etc...)
    - Good backward and forward compatibility
    - Generally low overhead (1 proto file, the rest is derived from that)
    - Time tested, very mature
    - CON: Hard to read by human or without the spec
    - CON: Doesn't handle messages larger than a few megabytes well
    - CON: Information with dedicated serialization formats is better handled by the tools optimized for those use cases (matrices, audio, video)

<!-- markdownlint-disable-next-line MD024 -->
### Personal Notes

- There's not a single use case in our current projects where the network overhead of protobuf matters
- The talk was mostly about byte-level optimizations that I've not seen affect us at all
- Interesting stuff, but not for the problems we're solving

## Migrating 3.8 million lines of code from Python2 to Python3

<!-- markdownlint-disable-next-line MD024 -->
### Personal Notes

- Everyone uses pylint, black, flake8, per-commit, etc... Conversationally, everyone generally expects that everyone else uses it too
