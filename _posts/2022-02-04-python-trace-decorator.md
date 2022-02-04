---
layout: post
title: Trace decorator
subtitle: Trace decorator
cover-img: /assets/img/trace-class.jpg
thumbnail-img: /assets/img/trace-class.jpg
share-img: /assets/img/trace-class.jpg
gh-repo: bartekmaciejewski/bartekmaciejewski.github.io
gh-badge: [star, fork, follow]
tags: [python, decorator]
comments: true
---

## Trace decorator

```
import types
from functools import wraps

trace_types = (
    types.MethodType,
    types.FunctionType,
    types.BuiltinFunctionType,
    types.BuiltinMethodType,
    types.MethodDescriptorType,
    types.ClassMethodDescriptorType
)


def trace_func(func):
    if hasattr(func, 'tracing'):
        return func

    @wraps(func)
    def wrapper(*args, **kwargs):
        result = None
        try:
            result = func(*args, **kwargs)
            return result
        except Exception as e:
            result = e
            raise
        finally:
            print(f'{func.__name__} ({args!r}, {kwargs!r}) -> {result!r}')

    wrapper.tracing = True
    return wrapper


def trace(decorated_class):
    for key in dir(decorated_class):
        value = getattr(decorated_class, key)
        if isinstance(value, trace_types) and key != "__new__": # __new__ can't be called this way (https://stackoverflow.com/questions/59217884/new-method-giving-error-object-new-takes-exactly-one-argument-the-typ)
            wrapped = trace_func(value)
            setattr(decorated_class, key, wrapped)
    return decorated_class


@trace
class Name:
    def __init__(self, name: str):
        self.name = name

    def name_uppercase(self, force: bool):
        return self.name + " is upper " + force.__str__()


n = Name("aaaaa")
print(n.name_uppercase(True))
print(n.name)
```
