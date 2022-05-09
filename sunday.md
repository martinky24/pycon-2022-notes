# Pycon - Sunday, May 1

## Opening Keynote: Python Steering Council

- Unfortunately only 2/5 of the council was present
- Discussed the Steering Council, how it works, how it related to PSF, etc...
- Discussed major updates coming in 3.11
  - speed up
  - new error message updates
  - better tracebacks
  - improved exceptions (exception groups, etc)

## Serialization in Python

- 2 types of serialization: plaintext vs. binary
- Pickle
  - In standard lib
  - Don't need to define your own schema
  - Well documented
  - Fast
  - Security isn't great.
  - Only interoperable with Python
  - Load still required access to class def'n
- Dill
  - Same interface as Pickle, with some additional features
  - Primarily used to send python objects across the network as a byte stream
  - Allows one to share a class without the consumer needing access to the original class
- msgpack
  - only serializaed class attributes, useful sometimes but not always
- Marshmallow
  - Requires author to think about a schema
  - Nice integrations for flask
  - JSON, so does well with atoms that can be easily serialized to JSON

## Productionize Research Oriented Code

- Understand
  - Research oriented code mainly written to figure out something *new*, not a well-understood and known problem
  - [A rubric for ML production systems](https://research.google/pubs/pub45742/): Interesting paper on how "productionized" a ML model is, based on testing
  - Step 1: Read code before productionizing it. Add `#` comments, add `TODO` comments
- Modularize code
  - 3 steps in research oriented code:
    1. Preparation code
    1. Preprocessing code
    1. Calculation code
  - Dividing into each of those there is further room for modularity

Talk got less interesting and hard to pay attention to as it went on.

# Ben Davis Notes

## Getting Started with Statically Typed Programming in Python 3.10
- Use `Iterable` rather than say `list` when type annotating, this will allow for any iterable type and not force a user of a function to only pass a list.
  - `Collection` is another generic type to accept when type annotating
- Just consider what is best for reuse, one could for a `list[str]` but accepting an `Iterable[str]` is likely better design
- Union syntax can be used for accepting or returning multiple types when type annotating 
  - `def add(x: int | float, y: int | float) -> int | float:`
  - 3.9 requires `types.Union[int,float]`
- Rather than returning `None` make this scenario an exception with in a function, make what is returned from a function strict, use exceptions for error cases
- Type annotation for passing function pointers should be `Callable`
- In 3.10 you can use `TypeAlias` to create your own types for readablitiy, say two lists could be list of strings, we can type alias to say on list is a list of emails strings, the other a list of address strings
