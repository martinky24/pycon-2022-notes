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
