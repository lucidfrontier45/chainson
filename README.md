# chainson
Chainable JSON Encoder

## Install

```bash
$ pip install chainson
```

## Usage

JSON Chain Encoder can be created by first instantiating a `JSONEncoderChain`
and then add check and encode functions.


```python
import json
import datetime
from chainson import JSONEncoderChain

chain = JSONEncoderChain()\
    .add_encoder(
        (lambda o: isinstance(o, datetime.datetime), lambda o: o.isoformat())
    ).add_encoder(
        (lambda o: isinstance(o, complex), lambda o: {"real": o.real, "imag": o.imag})
    )


chain.dumps(some_obj)
json.dumps(some_obj, default=chain.default)
```

Functions can also be specified in an OOP way.

```python
from chainson import AbstractJSONEncoderWithChecker

class DateTimeJSONEncoder(AbstractJSONEncoderWithChecker):
    def chk(self, o):
        return isinstance(o, datetime.datetime)

    def enc(self, o):
        return o.isoformat()

chain.add(DateTimeJSONEncoder())
```