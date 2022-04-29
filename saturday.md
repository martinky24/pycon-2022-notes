# Pycon - Friday, April 29

## Opening Keynote

Main takeaways

- I should be actively adding typing to more code I write
  - Maybe note for mypi integration (yet), but because it's low overhead tooling to help improve self-documenting code
  - If something is hard to type, you probably need to refactor it

- I need to focus more on single type outputs of functions, if it's impossible to output something of that type, an exception is reasonable

## Python Oddities

- General notes on variables, mutability, data structures, operators "gotchas"
- Scobp abuse
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

### Personal notes

- Emphasis on investment in infrastructure to make Python work
- They mention that core infrastructure is important to be well maintained (and carefully implemented). Invest in it. Otherwise it only slows down people downstream. Don't move too fast...
