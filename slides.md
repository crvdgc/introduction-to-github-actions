---
title: CI/CD with GitHub Actions
author: Liu Yuxi
date: 2022-04
---

# Introduction

## What is CI/CD

Continuous Integration & Continuous Delivery

$$
    \framebox{Devlopment} \rightarrow \framebox{Test} \rightarrow \framebox{Deployment}
$$

\pause

**Continuous**: make above steps interwoven, rather than in batches.

\pause

For this, we need **automation**.

## GitHub Actions

\columnsbegin
\column{.5\textwidth}

```yaml
name: learn-github-actions
on: [push]
jobs:
  check-bats-version:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: '14'
      - run: npm install -g bats
      - run: bats -v
```

\column{.5\textwidth}

Set-up hierarchy:

- **Workflow**: a configuration file
- **Job**: a set of *steps* that can execute in parallel
- **Step**: an *action* or a script
- **Action**: a configurable and reusable script

\columnsend

## GitHub Actions (Cont.)

![Runtime overview](assets/img/overview-actions-simple.png)

## Benefits of GitHub Actions over Buildkite

- Cost
  - Free for public repository
  - Free for self-host runners for private repository
  - Buildkite: per user
- Integration with GitHub
  - Do not need to grant additional access to the repository
  - UI

# Basics

## Function


# Examples

## Sequence of argument or argument of sequence

```python
x0: Tensor = torch.rand(10, 32, 512)
x1: Tensor = torch.rand(20, 32, 512)
x: Tensor = torch.cat(x0, x1)
```

\pause

```
[Pyright reportGeneralTypeIssues] [E] ...
  Type "Tensor" cannot be assigned to type "Tuple[Tensor, ...]
    | List[Tensor]"
    "Tensor" is incompatible with "Tuple[Tensor, ...]"
    "Tensor" is incompatible with "List[Tensor]"
```

However, `rand` is polymorphic and accept both.

\pause

```python
x: Tensor = torch.cat((x0, x1))
```

# Advanced typing

## How to describe a duck

```python
n_books: int = 30
book_noun: str = "book" if n_books == 1 else "books"
summary: str = "We have {} {}.".format(n_books, book_noun)
```

An `int` and a `str` can both be used in `format`, since they both implement the `__format__` method.

**Duck typing** checks if the value has the required capabilities.

\pause

How to describe and check?

## How to describe a duck (Cont.)

In general, inheritance from an Abstract Base Class (ABC)

```python
class Location(ABC):
    @abstractmethod
    def get_address(self) -> Address:
        pass

class Library(Location):
    def get_address(self) -> Address:
        return self.address
```

## How to describe a duck (Cont.)

For some known methods (methods with "magic names" `__`), there are pre-defined [ABCs](https://docs.python.org/3/library/collections.abc.html).

| ABC       | Abstract methods | Example usage       |
|:---------:|:-----------------|:--------------------|
| Container | `__contains__`   | `book in shelf`     |
| Iterable  | `__iter__`       | `for book in shelf` |
| Sized     | `__len__`        | `len(shelf)`        |
| ...       | ...              | ...                 |

Per [PEP585](https://peps.python.org/pep-0585/), these are in `collections.abc` instead of `typing` from Python 3.9.

# Difficulties

## Types versus values

[torch.nn.SmoothL1Loss](https://pytorch.org/docs/stable/generated/torch.nn.SmoothL1Loss.html)

```python
class SmoothL1Loss(size_average=None, reduce=None, reduction="mean", beta=1.0)
```

where `reduction` expects one of `"none"`, `"mean"`, `"sum"`.

\pause

```python
class Reduction(Enum):
  Mean = 0
  Sum = 1
class SmoothL1Loss(..., reduction: Optional[Reduction] = Reduction.Mean)
```

## Untyped (weakly typed) libraries

```python
arr: numpy.ndarray = numpy.array([1, 2, 3])
mask: torch.nn.Tensor = tensor != PAD_TOKEN
```

\pause

What is the shape?

What is the `dtype`?

## Mathematical symbols

Suppose $R$ is the radius of a sphere.

Then the volume can be calculated with the following formula:

$$V = \frac{4\pi}{3} R^3$$

## Code

```hs
data Maybe a = Just a | Nothing
```

## Inline LaTeX

\begin{center}
  \emph{Hello, World!}
\end{center}

## References

- PEP484 Type Hints: https://peps.python.org/pep-0484/
