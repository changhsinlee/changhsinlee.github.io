---
layout: post
title: "Mutable defaults in Python dataclass"
comments: true
lang: en
type: note
header-img: "/figure/source/thumbnails/pen-paper.png"
---

Me learning about how NOT to use mutable defaults in `dataclass`. 

I was working through the book [Pythonic Application Architecture Patterns](https://github.com/cosmicpython/book) and ran into an issue that took me a while to figure out. Turned out it had to do with mutable defaults in `dataclass`. 

PyCharm always warns me about mutable defaults when I put things like

```py
def my_function(x=[]):
    ...
```

but I didn't know why. It got me this time.

## My issue

In [the code snippet used in chapter 1 of the book](https://github.com/cosmicpython/code/blob/chapter_01_domain_model/model.py), there is a `Batch` object:

```py
class Batch:
    def __init__(
        self, ref: str, sku: str, qty: int, eta: Optional[date]
    ):
        self.ref = ref
        self.sku = sku
        self.qty = qty
        self.eta = eta
        self._allocations = set()  # type: Set[OrderLine]
        
```

I decided to be cute and used `dataclass` instead to define `Batch`:

```py
@dataclass
class Batch:
    ref: str
    sku: str
    qty: int
    eta: date = None
    _allocations: set() 
```

But when I run the tests,

```py
def test_prefers_earlier_batches():
    earliest = Batch("speedy-batch", "MINIMALIST-SPOON", 100, eta=today)
    latest = Batch("slow-batch", "MINIMALIST-SPOON", 100, eta=later)
    line = OrderLine("order1", "MINIMALIST-SPOON", 10)

    allocate(line, [medium, latest])

    assert earliest.available_quantity == 90
    assert latest.available_quantity == 100
```

Both assertions gave me 90?! In other words, my code was modifying `latest` when it was not supposed to.

When I run the PyCharm debugger, I found that the `latest` instance got modified at the same time as the `earliest`. I then change my code to exactly what the book says and it worked. That's when I figure it could be a mutiple default issue.

## What was the issue?

It turned out my `Batch` through `dataclass` was evaluated to 

```py
class Batch:

    _allocations = set()

    def __init__(
        self, ref: str, sku: str, qty: int, eta: Optional[date]
    ):
        self.ref = ref
        self.sku = sku
        self.qty = qty
        self.eta = eta
```

according to the official doc, so all my instances are sharing the same `_allocations` reference in memory!

* The doc: https://docs.python.org/3/library/dataclasses.html#mutable-default-values

## How to properly declare the dataclass

Will Gaggioli from [Penny University](https://www.pennyuniversity.org/) pointed out that I should use the `field()` function:

* https://docs.python.org/3/library/dataclasses.html#dataclasses.field


```py
@dataclass
class Batch:
    ref: str
    sku: str
    qty: int
    eta: date = None
    _allocations: Set = field(default_factory=set)
```

Bottom line: Read the docs and be careful about using mutable defaults in Python!