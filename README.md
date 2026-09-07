# Clean Python Code

- This is a project by luxdev Hq student.

## Table of Contents

![Uncle Bob surrounded with computers](assets/Robert_C._Martin_surrounded_by_computers.jpg)

1. [Introduction](#introduction)

2. [Naming Things](#naming-things)
   - [Use intention revealing names](#use-intention-revealing-names)
   - [Meaningful Distinctions](#meaningful-distinctions)
   - [Avoid Disinformation](#avoid-disinformation)
   - [Pronounceable Names](#pronounceable-names)
   - [Searchable Names](#searchable-names)
   - [Don't be cute](#dont-be-cute)
   - [Avoid Encodings](#avoid-encodings)
     - [Hungarian Notation](#hungarian-notation)
     - [Member Prefixes](#member-prefixes)
     - [Interfaces & Implementations](#interfaces--implementations)
   - [Gratuitous Context](#gratuitous-context)
   - [Avoid Mental Mapping](#avoid-mental-mapping)
   - [Class Names](#class-names)
     - [Types of Objects](#types-of-objects)
     - [Simple Superclass Name](#simple-superclass-name)
     - [Qualified Subclass Name](#qualified-subclass-name)
   - [Method Names](#method-names)
   - [Pick One Word per Concept](#pick-one-word-per-concept)
   - [Don't Pun](#dont-pun)
   - [Use Solution Domain Names](#use-solution-domain-names)
   - [Use Problem Domain Names](#use-problem-domain-names)
   - [Add Meaningful Context](#add-meaningful-context)

3. [Functions](#functions)
   - [Small](#small)
   - [Do One Thing](#do-one-thing)
   - [One level of Abstraction](#one-level-of-abstraction)
   - [Avoid Conditionals](#avoid-conditionals)
   - [Use Descriptive Names](#use-descriptive-names)
   - [Function Arguments](#function-arguments)
   - [Avoid Side Effects](#avoid-side-effects)
     - [Pure Functions](#pure-functions)
     - [Niladic Functions](#1-niladic-functions)
     - [Argument Mutation](#2-argument-mutation)
     - [Exceptions](#3-exceptions)
     - [I/O](#4-io)
   - [Command Query Separation](#command-query-separation)
   - [Don't Repeat Yourself](#dont-repeat-yourself-dry)

4. [Objects and Data Structures](#objects-and-data-structures)
   - [The Law of Demeter](#the-law-of-demeter)
   - [The data and object anti-symmetry](#the-data-and-object-anti-symmetry)
   - [Data Transfer Objects](#data-transfer-objects)

5. [Classes](#classes)
   - [Classes should be small](#classes-should-be-small)
   - [Cohesion](#cohesion)
   - [Organising for change](#organising-for-change)
   - [Depend on abstractions, not details](#depend-on-abstractions-not-details)

6. [SOLID Principles](#solid-principles)
   - [Single Responsibility Principle](#single-responsibility-principle)
   - [Open/Closed Principle](#openclosed-principleocp)
   - [Liskov Substitution Principle](#liskov-substitution-principle)
   - [Interface Segregation Principle](#interface-segregation-principle)
   - [Dependency Inversion Principle](#dependency-inversion-principle)

7. [Testing](#testing)

8. [Concurrency](#concurrency)

9. [Error Handling](#error-handling)

10. [Formatting](#formatting)

11. [Comments](#comments)

12. [Translation](#translation)

Software engineering principles, from Robert C. Martin's book
[_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
adapted for Python. This is not a style guide. It's a guide to producing readable, reusable, and refactorable software in Python.

## Introduction

---

Software is read far more often than it is written. A line of code is typed once, but it will be read by
you, by your teammates and by the person who maintains the system long after you have moved on. Every
minute you invest in making that line obvious is paid back many times over.

**Clean code** is code that is easy to read, easy to reason about and cheap to change. Robert C. Martin
describes it as code that "always looks like it was written by someone who cares". It is not about
cleverness, it is about communication.

This repository adapts the ideas from
[_Clean Code_](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) to Python.
A few things to keep in mind while reading:

- **This is not a style guide.** Formatting is a solved problem in Python; hand it to
  [`black`](https://black.readthedocs.io/), [`ruff`](https://docs.astral.sh/ruff/) or
  [PEP 8](https://peps.python.org/pep-0008/) and spend your energy on design instead.
- **These are guidelines, not laws.** Every rule here has a cost. A rule applied without judgement
  produces code that is technically compliant and practically unreadable.
- **Python is not Java.** Many "Clean Code" examples in the wild are ceremonial because they come from a
  language without first class functions, default arguments, tuples or duck typing. Where Python offers a
  simpler tool than a design pattern, the notes say so.
- **Refactoring is continuous.** Nobody writes clean code on the first pass. You write code that works,
  then you clean it while the tests keep you honest.

The chapters that follow move from the smallest unit of design to the largest: names, then functions, then
data, then classes, then the SOLID principles that govern how classes depend on one another, and finally
the cross cutting concerns of testing, concurrency, error handling and comments.

**[⬆ back to top](#table-of-contents)**

## Naming Things

---

Modern software is so complex that no one can understand all parts of a non-trivial project alone. The only way humans tame details is through abstractions. With abstraction, we focus on the essential and forget about the non-essential at that particular time. You remember the way you learned body biology?? You focused on one system at a time, digestive, nervous, cardiovascular e.t.c and ignored the rest. That is abstraction at work.

A variable name is an abstraction over memory, a function name is an abstraction over logic, a class name is an abstraction over a packet of data and the logic that operates on that data.

The most fundamental abstraction in writing software is **naming**. Naming things is just one part of the story, using good names is a skill that unfortunately, is not owned by most programmers and that is why we have come up with so many refactorings concerned with naming things.

Good names bring order to the chaotic environment of crafting software and hence, we better be good at this skill so that we can enjoy our craft.

**[⬆ back to top](#table-of-contents)**

### **Use intention revealing names**

---

This rule enforces that programmers should make their code read like well written prose by naming parts <br>
of their code perfectly. With such good naming, a programmer will never need to resort to comments or unnecessary <br> doc strings.
Below is a code snippet from a software system. Would you make sense of it without any explanation?

**Bad** :angry:

```python
from typing import List

def f(a : List[List[int]])->List[List[int]]:
    return [i for i in a if i[1] == 0]
```

It would be ashaming that someone would fail to understand such a simple function. What could have gone wrong??

The problem is simple.This code is littered with **mysterious names**. We have to agree that this is code and not a detective novel. Code should be clear and precise.

What this code does is so trivial. It takes in a collection of orders and returns the pending orders. Let's pause for a moment and appreciate the extreme over engineering in this solution.
The programmer assumes that each order is coded as a list of `ints` (`List[int]`) and that the second element is the order status. He decides that 0 means pending and 1 means cleared.

Notice the first problem... that snippet doesn't contain knowledge about the domain. This is a design smell known as a **missing abstraction**. We are missing the Order abstraction.

> **Missing Abstraction** <br>
> This smell arises when clumps of data are used instead creating a class or an interface

We have a thorny problem right now, we lack meaningful domain abstractions. One of the ways of solving the missing abstraction smell is to **map domain entities**. So lets create an abstraction called Order.

```python
from typing import List

class Order:
    def __init__(self, order_id : int, order_status : int) -> None:
        self._order_id = order_id
        self._order_status = order_status

    def is_pending(self) -> bool:
        return self._order_status == 0

    #more code goes here
```

> We could also have used the **namedtuple** in the python standard library but we won't be too functional that early. Let us stick with OOP for now. **namedtuples** contain only data and not data with code that acts on it.

Let us now refactor our entity names and use our newly created abstraction too. We arrive at the following code snippet.

**Better: :smiley:**

```python
from typing import List

Orders = List[Order]

def get_pending_orders(orders : Orders)-> Orders:
    return [order for order in orders if order.is_pending()]
```

This function reads like well written prose.

Notice that the `get_pending_orders()` function delegates the logic of finding the order status to the Order class. This is because the Order class knows its internal representation more than anyone else, so it better implement this logic. This is known as the **Most Qualified Rule** in OOP.

> **Most Qualified Rule** <br>
> Work should be assigned to the class that knows best how to do it.

> We are using the listcomp for a reason. Listcomps are examples of **iterative expressions**. They serve one role and that is creating lists. On the other hand, for-loops are **iterative commands** and thus accomplish a myriad of tasks. Pick the right tool for the job.

Never allow client code know your implementation details. In fact the ACM A.M Laureate Babra Liskov says it soundly in her book [Program development in Java. Abstraction, Specification and OOD](https://book4you.org/book/1164544/93467d). The **Iterator design pattern** is one way of solving that problem.

Here is another example of a misleading variable name.

**Bad** :angry:

```python
student_list= {'kasozi','vincent', 'bob'}
```

This variable name is so misleading.

- It contains noise. why the list suffix?
- It is lying to us. Lists are not the same as sets. They may all be collections but they are not the same at all.

To prove that lists are not sets, below is a code snippet that returns the methods in the List class that aren't in the Set class.

```python
sorted(set(dir(list())) - set(dir(set())))
```

Once it has executed, `append()` is one of the returned functions implying that sets don't support `append()` but instead support `add()`. So you write the code below, your code breaks.

> Sets are not sequences like lists. In fact, they are unordered collections and so adding the `append()` method to the set class would be misleading. `append()` means we are adding at the end which may not be the case with sets.

**Bad** :angry:

```python
student_list= {'kasozi','vincent', 'bob'}
student_list.append('martin') #It breaks!!
```

It is better to use a different variable name is neutral to the data structure being used.
In this case, once you decide to change data structure used, your variable won't destroy the semantics of your code.

**Good** :smiley:

```python
students = {'kasozi', 'vincent', 'bob'}
```

> You can not achieve good naming with a bad design. You can see that mapping domain entities into our code has made our codebase use natural names.

**[⬆ back to top](#table-of-contents)**

### Meaningful-Distinctions

---

When two things in your code are different, their names must tell you **how** they are different. Names that
differ only by a noise word, a number or a synonym force the reader to open both definitions and diff them
by hand.

Number series names (`a1`, `a2`, ... `aN`) are the laziest form of this. They carry no information at all.

**Bad** :angry:

```python
from typing import List

def copy_marks(source: List[int], destination: List[int]) -> None:
    for i in range(len(source)):
        destination[i] = source[i]
```

Compare that with the version the reader actually has to decode:

**Bad** :angry:

```python
from typing import List

def copy(a1: List[int], a2: List[int]) -> None:
    for i in range(len(a1)):
        a2[i] = a1[i]
```

Is `a1` the source or the destination? You cannot know without reading the body. `source` and `destination`
answer the question in the signature.

**Noise words** are the subtler version of the same mistake. `Info`, `Data`, `Object`, `Manager`, `Variable`
and `The` are all words that could be deleted without changing the meaning of the name.

**Bad** :angry:

```python
class Customer:
    ...

class CustomerData:      # how is this different from Customer?
    ...

class CustomerInfo:      # ...or from this?
    ...

class CustomerObject:    # ...or this?
    ...
```

If you cannot explain to a colleague when to reach for `Customer` and when to reach for `CustomerData`, then
you do not have four concepts, you have one concept and three redundant classes.

**Good** :smiley:

```python
from dataclasses import dataclass
from decimal import Decimal


@dataclass(frozen=True)
class Customer:
    """A customer as the business talks about them."""
    customer_id: str
    full_name: str


@dataclass(frozen=True)
class CustomerCreditProfile:
    """Everything the credit department needs to score a customer."""
    customer_id: str
    credit_limit: Decimal
    outstanding_balance: Decimal
```

Now the two names describe two genuinely different ideas, and the distinction lives in the name rather than
in tribal knowledge.

The same rule applies to functions. If a module exposes `get_account()`, `fetch_account()` and
`retrieve_account()`, a reader has to assume there are three different behaviours, because otherwise why
would there be three names? Pick one verb per concept and stick to it (see
[Pick One Word per Concept](#pick-one-word-per-concept)).

**Bad** :angry:

```python
def get_active_users(): ...
def fetch_active_accounts(): ...   # same thing, different words
def retrieve_live_customers(): ... # same thing again
```

**Good** :smiley:

```python
def get_active_users(): ...
def get_active_accounts(): ...
def get_active_customers(): ...
```

> **Rule of thumb:** if you can swap two names in your head and the code still makes sense, the names are not
> meaningfully distinct.

**[⬆ back to top](#table-of-contents)**

### Avoid-Disinformation

---

A name that is merely vague slows the reader down. A name that is **wrong** sends them in the opposite
direction, and they will trust it, because names are the only documentation people actually read.

#### Do not lie about the type

Suffixes such as `list`, `dict` or `str` are a promise. Break the promise and you have planted a bug in the
reader's mind.

**Bad** :angry:

```python
account_list = {'123': 400.0, '456': 250.0}   # it is a dict, not a list
```

**Good** :smiley:

```python
balance_by_account = {'123': 400.0, '456': 250.0}
```

`balance_by_account['123']` reads exactly like what it does, and it survives the day someone swaps the dict
for an ordered mapping or a database row.

#### Do not lie about the meaning

**Bad** :angry:

```python
def is_valid(email: str) -> bool:
    return '@' in email and email.endswith('.com')
```

The name claims general validity; the body only accepts `.com` addresses. Every caller that trusts the name
ships a bug for `.org` users.

**Good** :smiley:

```python
def is_dotcom_email(email: str) -> bool:
    return '@' in email and email.endswith('.com')
```

The behaviour did not change, but nobody is misled any more, and the awkward name makes the arbitrary rule
visible so it can be questioned.

#### Do not use names with a foreign meaning

`hp`, `aix` and `sco` are names of Unix platforms. `Account` in a banking system means something specific to
the domain experts. Reusing such a word for something else is disinformation even if the spelling is
innocent.

#### Beware of near identical names

```python
XYZControllerForEfficientHandlingOfStrings
XYZControllerForEfficientStorageOfStrings
```

These differ by two words in the middle of a thirty-character name. Autocomplete will happily hand you the
wrong one, and code review will not catch it.

#### Never use lower case `l` or upper case `O` as names

**Bad** :angry:

```python
l = 1
O = 0
if O == l:      # is that a zero and a one, or two letters?
    l = O
```

In most fonts `l` is indistinguishable from `1` and `O` from `0`.

**Good** :smiley:

```python
line_count = 1
offset = 0
if offset == line_count:
    line_count = offset
```

> **Type hints are part of the name.** `def process(items)` tells you nothing;
> `def refund(orders: list[Order]) -> list[Refund]` cannot lie without the type checker noticing. Let
> `mypy` or `pyright` keep your names honest.

**[⬆ back to top](#table-of-contents)**

### Pronounceable-names

---

When naming things in your code, it is much better to use names that are easy to pronounce by programmers.
This enables developers to discuss the code without the need to sound silly as they mention the names. If
you are a polyglot in natural languages, it is much better to use the language common to most developers
when naming your entities.

Humans have evolved for language. That part of your brain is doing real work while you read code, so a name
you can say out loud is a name you can hold in your head, remember tomorrow and mention in a stand-up
without spelling it letter by letter.

**Bad** :angry:

```python
from typing import List
import math

def sqrs(first_n: int) -> List[int]:
    if first_n > 0:
        return [int(math.pow(i, 2)) for i in range(first_n)]
    return []

lstsqrs = sqrs(5)
```

How can a human pronounce `sqrs` and `lstsqrs`? This is a serious problem. Let's correct it.

**Good** :smiley:

```python
from typing import List
import math

def generate_squares(first_n: int) -> List[int]:
    if first_n > 0:
        return [int(math.pow(i, 2)) for i in range(first_n)]
    return []

squares = generate_squares(5)
```

The problem shows up most painfully in data records, where the unpronounceable name is repeated at every
call site:

**Bad** :angry:

```python
from dataclasses import dataclass

@dataclass
class DtaRcrd102:
    genymdhms: str      # "gen why emm dee aich emm ess"?
    modymdhms: str
    pszqint: str = '102'
```

**Good** :smiley:

```python
from dataclasses import dataclass
from datetime import datetime

@dataclass
class Customer:
    generation_timestamp: datetime
    modification_timestamp: datetime
    record_id: str = '102'
```

The second version supports an actual conversation: *"Hey Bob, look at the record with generation timestamp
of last Tuesday."*

#### When abbreviations are fine

Do not overcorrect. An abbreviation that every programmer in the room already pronounces as a word is a real
word for naming purposes:

```python
html_response          # not hyper_text_markup_language_response
http_client            # not hyper_text_transfer_protocol_client
url, uuid, csv, json, api, db, id
```

The test is social, not textual: **can two developers say this name out loud to each other and both know
what it means?** If yes, keep it. If it comes out as a spelling bee, rename it.

Loop counters are the traditional exception. `i`, `j` and `k` inside a three-line loop are pronounceable and
carry decades of shared convention, so they are fine; `i` as a module-level variable is not.

**[⬆ back to top](#table-of-contents)**

### Searchable-Names

---

For example, you are looking for some part of the code where you calculate something and you remember that
it was about work days in a week.

**Bad** :angry:

```python
for i in range(0, 34):
    s += (t[i] * 4) / 5
```

What is easier to find, `5` or `WORK_DAYS_PER_WEEK`? Searching for `5` in any real codebase returns
thousands of hits, and none of them tell you whether they mean the same five.

Single letter names and bare numeric literals ("magic numbers") share the same flaw: they cannot be found,
and they cannot be changed with confidence. If the working week ever becomes four days, the `5` above is
indistinguishable from every other `5` in the system.

It is normal to name a local variable with one character in a short function, but if you can avoid it, do.

**Good** :smiley:

```python
from typing import Final, List

REAL_DAYS_PER_IDEAL_DAY: Final[int] = 4
WORK_DAYS_PER_WEEK: Final[int] = 5


def estimate_weeks(task_estimates: List[int]) -> float:
    total_weeks = 0.0
    for task_estimate in task_estimates:
        real_task_days = task_estimate * REAL_DAYS_PER_IDEAL_DAY
        total_weeks += real_task_days / WORK_DAYS_PER_WEEK
    return total_weeks
```

Three things improved at once:

1. Every constant is now greppable, and `Final` tells both the reader and the type checker that it is not
   meant to be reassigned.
2. The loop iterates over the collection instead of over a hard coded `range(0, 34)` that silently breaks
   when the number of tasks changes.
3. `sum` is no longer shadowed. Naming a variable `sum`, `list`, `id`, `type` or `input` hides a builtin and
   is a bug waiting for the line that needs the real one.

> The length of a name should be proportional to the size of its scope. A three-line comprehension can use
> `n`; a module level constant that appears in twenty files deserves `WORK_DAYS_PER_WEEK`.

**[⬆ back to top](#table-of-contents)**

### Don't-be-cute

---

Humour ages badly and does not survive translation. A joke name is only funny to the person who wrote it, on
the day they wrote it, in their culture, and it is a puzzle for everybody else forever after.

**Bad** :angry:

```python
def whack()   -> None: ...   # deletes a record
def eat_my_shorts() -> None: ...   # aborts the job
def holy_hand_grenade() -> None: ...   # clears the cache
```

**Good** :smiley:

```python
def delete_record() -> None: ...
def abort_job() -> None: ...
def clear_cache() -> None: ...
```

The same applies to slang and colloquialisms. `kill_it()`, `blow_away()` and `nuke()` all mean "delete", but
only `delete()` means it in every English speaking country and in every translation of your documentation.

> **Say what you mean. Mean what you say.** Cleverness in a name is a cost paid by every future reader so
> that one author could enjoy a moment.

**[⬆ back to top](#table-of-contents)**

### Avoid-Encodings

---

Encoding type or scope information into a name was invented for languages and editors that could not tell
you either. Python has type hints, and your editor has "go to definition"; the encoding is now pure
overhead that has to be maintained by hand and that silently rots the moment the type changes.

**Bad** :angry:

```python
str_name = 'kasozi'
i_count = 3
f_rate = 0.5
l_accounts = ['a', 'b']
```

**Good** :smiley:

```python
name: str = 'kasozi'
count: int = 3
rate: float = 0.5
accounts: list[str] = ['a', 'b']
```

The annotation says the same thing, a type checker verifies it, and renaming a type does not leave a lie
behind in the identifier.

**[⬆ back to top](#table-of-contents)**

#### Hungarian-Notation

---

Hungarian Notation prefixes each name with a code for its type: `strName`, `iCount`, `bIsReady`,
`arrItems`. It made sense in 1980s C, where a compiler was weakly typed and an editor could not tell you
anything about a symbol.

**Bad** :angry:

```python
def calculate(fPrice: float, iQuantity: int) -> float:
    fTotal = fPrice * iQuantity
    return fTotal
```

**Good** :smiley:

```python
def calculate_total(price: float, quantity: int) -> float:
    return price * quantity
```

The real damage is that the prefix eventually lies. The day `iQuantity` becomes a `Decimal`, you either
rename it in every file or you leave a permanent piece of disinformation in the codebase. Type hints move
with the code because tooling checks them.

**[⬆ back to top](#table-of-contents)**

#### Member-Prefixes

---

You do not need to prefix instance attributes to mark them as members. Inside a class, `self.` already says
it, and outside a class the attribute is reached through an object.

**Bad** :angry:

```python
class Account:
    def __init__(self, balance: float) -> None:
        self.m_balance = balance          # 'm_' for member
        self.m_owner_name = 'unknown'

    def deposit(self, amount: float) -> None:
        self.m_balance += amount
```

**Good** :smiley:

```python
class Account:
    def __init__(self, balance: float) -> None:
        self.balance = balance
        self.owner_name = 'unknown'

    def deposit(self, amount: float) -> None:
        self.balance += amount
```

Python does have one meaningful naming convention here, and it is about **visibility, not membership**:

```python
class Account:
    def __init__(self, balance: float) -> None:
        self.balance = balance      # public API
        self._ledger = []           # internal, subject to change without notice
        self.__token = 'secret'     # name mangled to _Account__token
```

- A single leading underscore is a convention meaning *"this is not part of the public interface"*. Nothing
  enforces it; it is a message to humans and to linters.
- A double leading underscore triggers name mangling. Use it only when you genuinely need to avoid a name
  clash in a subclass, not as a security feature.

If a class has so many attributes that you feel the urge to prefix them for organisation, the class is
probably doing too much. Split it (see the
[Single Responsibility Principle](#single-responsibility-principle)).

**[⬆ back to top](#table-of-contents)**

#### Interfaces-&-Implementations

---

In some ecosystems the convention is to mark the interface: `IShapeFactory` and `ShapeFactory`. If you have
to encode one of the two, prefer encoding the implementation, because the interface is the name that callers
depend on and it should be the clean one.

Python's version of this problem shows up with `abc.ABC` and `typing.Protocol`.

**Bad** :angry:

```python
from abc import ABC, abstractmethod

class IPaymentGateway(ABC):
    @abstractmethod
    def charge(self, amount: float) -> None: ...

class PaymentGateway(IPaymentGateway):
    def charge(self, amount: float) -> None: ...
```

The caller now depends on a name with a stray `I` in it, and the two names are one character apart.

**Good** :smiley:

```python
from abc import ABC, abstractmethod

class PaymentGateway(ABC):
    """What every gateway must be able to do."""

    @abstractmethod
    def charge(self, amount: float) -> None: ...


class StripeGateway(PaymentGateway):
    def charge(self, amount: float) -> None: ...


class FakeGateway(PaymentGateway):
    """Used by the test-suite; records charges instead of making them."""

    def __init__(self) -> None:
        self.charges: list[float] = []

    def charge(self, amount: float) -> None:
        self.charges.append(amount)
```

The abstraction owns the plain name; each implementation is named after what makes it different.

Often you do not need the base class at all. A `Protocol` gives you the same static guarantees with no
inheritance and no runtime coupling:

```python
from typing import Protocol

class PaymentGateway(Protocol):
    def charge(self, amount: float) -> None: ...


def check_out(total: float, gateway: PaymentGateway) -> None:
    gateway.charge(total)
```

Any object with a matching `charge` method satisfies `PaymentGateway`, including one written by a library you
do not control.

**[⬆ back to top](#table-of-contents)**

### Gratuitous-Context

---

Adding context is good; adding the *same* context to every name in the system is noise. If you are building
the "Gas Station Deluxe" application, do not prefix every class with `GSD`.

**Bad** :angry:

```python
# file: gas_station/gsd_models.py
class GSDAccount: ...
class GSDAccountAddress: ...
class GSDCustomer: ...

def gsd_calculate_gsd_account_total(gsd_account: GSDAccount) -> float: ...
```

Typing `GSD` into autocomplete now offers every class in the application, which is exactly as useful as no
autocomplete at all.

**Good** :smiley:

```python
# file: gas_station/models.py
class Account: ...
class Address: ...
class Customer: ...

def calculate_total(account: Account) -> float: ...
```

The module already supplies the context, and Python lets the caller decide how much of it to keep:

```python
from gas_station import models

account = models.Account()
```

The same applies inside a class. A method on `Account` does not need to repeat the class name:

**Bad** :angry:

```python
class Account:
    def get_account_balance(self) -> float: ...
    def set_account_owner(self, owner: str) -> None: ...
```

**Good** :smiley:

```python
class Account:
    def get_balance(self) -> float: ...
    def set_owner(self, owner: str) -> None: ...
```

`account.get_balance()` already reads as a sentence; `account.get_account_balance()` stutters.

> Shorter names are generally better than longer ones, **so long as they are clear**. Add no more context
> to a name than is necessary.

**[⬆ back to top](#table-of-contents)**

### Avoid-Mental-Mapping

---

Readers should not have to mentally translate your names into names they already know. This is the problem
with single letter variables outside a tiny scope: the reader has to keep a private lookup table in their
head while they read, and every lookup is a chance to get it wrong.

**Bad** :angry:

```python
def process(d: dict) -> list:
    r = []
    for k, v in d.items():
        if v > 0:
            t = k.upper()
            r.append((t, v))
    return r
```

To review that function you must first decide that `d` is a stock level per product, `r` is the result, `k`
is a product code, `v` is a quantity and `t` is the normalised code. None of it is written down.

**Good** :smiley:

```python
def in_stock_products(stock_by_product: dict[str, int]) -> list[tuple[str, int]]:
    available = []
    for product_code, quantity in stock_by_product.items():
        if quantity > 0:
            available.append((product_code.upper(), quantity))
    return available
```

Nothing has to be decoded; the names are the explanation. And once the names are honest, the simplification
becomes obvious:

```python
def in_stock_products(stock_by_product: dict[str, int]) -> list[tuple[str, int]]:
    return [
        (product_code.upper(), quantity)
        for product_code, quantity in stock_by_product.items()
        if quantity > 0
    ]
```

> **Professionals write code that others can understand.** One difference between a smart programmer and a
> professional programmer is that the professional understands that clarity is king.

**[⬆ back to top](#table-of-contents)**

### Class-Names

---

A class is a noun. It models a thing, so its name should be a noun or a noun phrase in `PascalCase`:
`Customer`, `WikiPage`, `Account`, `AddressParser`.

Avoid verbs, and be suspicious of the words `Manager`, `Processor`, `Data`, `Info`, `Handler` and `Util`.
They are not wrong by definition, but they are the words we reach for when we do not know what a class is
for, and a class that we cannot name is usually a class that does too much.

**Bad** :angry:

```python
class DataManager:
    def handle(self, stuff): ...
```

**Good** :smiley:

```python
class InvoiceRepository:
    def save(self, invoice: 'Invoice') -> None: ...
    def find_by_id(self, invoice_id: str) -> 'Invoice': ...
```

`InvoiceRepository` tells you what it holds, where it sits in the design and what you may ask of it.

**[⬆ back to top](#table-of-contents)**

#### Types-of-Objects

---

Most classes fall into one of a handful of roles, and naming is much easier once you know which role you are
naming. A useful split:

| Role | What it is | Naming pattern | Example |
| --- | --- | --- | --- |
| **Value object** | Immutable, compared by value, no identity | The concept itself | `Money`, `EmailAddress`, `DateRange` |
| **Entity** | Has an identity that outlives its attributes | The domain noun | `Customer`, `Order`, `Account` |
| **Service** | Stateless behaviour over other objects | Verb phrase turned noun | `PaymentProcessor`, `TaxCalculator` |
| **Repository / Gateway** | Talks to storage or to the outside world | `<Noun>Repository`, `<Noun>Gateway` | `OrderRepository`, `StripeGateway` |
| **Factory / Builder** | Constructs other objects | `<Noun>Factory`, `<Noun>Builder` | `ReportFactory`, `QueryBuilder` |
| **Data transfer object** | A bag of fields crossing a boundary | `<Noun>Request` / `<Noun>Response` | `CreateOrderRequest` |

```python
from dataclasses import dataclass
from decimal import Decimal


@dataclass(frozen=True)      # value object: two Money objects with the same
class Money:                 # amount and currency ARE the same money
    amount: Decimal
    currency: str


@dataclass                   # entity: two customers with the same name are
class Customer:              # still two different customers
    customer_id: str
    name: str
```

Mixing roles in one class is the most common source of unmaintainable code. A `Customer` that also knows how
to save itself to Postgres is an entity and a repository at once, and it now has two reasons to change.

**[⬆ back to top](#table-of-contents)**

#### Simple-Superclass-Name

---

The higher a class sits in a hierarchy, the more abstract it is, and the shorter and more general its name
should be. A base class name is a promise about the whole family, so it must not describe any one member.

**Bad** :angry:

```python
class AbstractBaseSavingsAccountImplementation: ...
class CheckingAccount(AbstractBaseSavingsAccountImplementation): ...
```

The base name mentions savings, so a checking account inheriting from it reads as nonsense, and the
`Abstract`, `Base` and `Implementation` noise adds nothing.

**Good** :smiley:

```python
from abc import ABC, abstractmethod


class Account(ABC):
    @abstractmethod
    def add_interest(self) -> None: ...


class SavingsAccount(Account): ...
class CheckingAccount(Account): ...
```

> A short, general superclass name is a sign that the abstraction is real. If you struggle to name the base
> class without listing its subclasses, the hierarchy is probably wrong.

**[⬆ back to top](#table-of-contents)**

#### Qualified-Subclass-Name

---

Subclass names are the mirror image: they carry the qualifier that says **how this one differs** from the
family. The convention is `<Qualifier><Superclass>`.

**Good** :smiley:

```python
class Account(ABC): ...

class SavingsAccount(Account): ...
class CheckingAccount(Account): ...
class FixedDepositAccount(Account): ...
```

Read as a sentence, `SavingsAccount is an Account` is true, which is a cheap first test of the
[Liskov Substitution Principle](#liskov-substitution-principle).

Watch for qualifiers that describe an implementation detail rather than a kind:

**Bad** :angry:

```python
class FastAccount(Account): ...      # fast is not a kind of account
class Account2(Account): ...         # a version number is not a qualifier
class AccountImpl(Account): ...      # 'Impl' says nothing
```

**Good** :smiley:

```python
class CachedAccountRepository(AccountRepository): ...   # caching IS the distinction
class PostgresAccountRepository(AccountRepository): ... # so is the backing store
```

**[⬆ back to top](#table-of-contents)**

### Method-Names

---

If a class is a noun, a method is a verb. Methods do things, so name them with a verb or a verb phrase in
`snake_case`: `save`, `delete_page`, `calculate_interest`.

**Bad** :angry:

```python
class Account:
    def balance_calculation(self) -> float: ...   # noun phrase
    def new_owner(self, owner: str) -> None: ...  # ambiguous: get or set?
```

**Good** :smiley:

```python
class Account:
    def calculate_balance(self) -> float: ...
    def change_owner(self, owner: str) -> None: ...
```

Predicates that return a `bool` read best as questions: `is_`, `has_`, `can_`, `should_`.

```python
def is_overdrawn(self) -> bool: ...
def has_enough_collateral(self, loan: float) -> bool: ...
def can_withdraw(self, amount: float) -> bool: ...
```

Python does **not** want Java-style accessors. If all a getter does is return an attribute, expose the
attribute; if it later needs logic, `@property` upgrades it without touching a single caller.

**Bad** :angry:

```python
class Account:
    def __init__(self, balance: float) -> None:
        self._balance = balance

    def get_balance(self) -> float:
        return self._balance

    def set_balance(self, balance: float) -> None:
        self._balance = balance
```

**Good** :smiley:

```python
class Account:
    def __init__(self, balance: float) -> None:
        self.balance = balance          # just an attribute


class InterestBearingAccount:
    def __init__(self, deposits: list[float]) -> None:
        self._deposits = deposits

    @property
    def balance(self) -> float:         # computed, but read like an attribute
        return sum(self._deposits)
```

When a constructor cannot express what it builds, hide it behind a classmethod whose name says what it makes:

```python
from datetime import date


class Money:
    def __init__(self, amount: float, currency: str) -> None:
        self.amount = amount
        self.currency = currency

    @classmethod
    def in_shillings(cls, amount: float) -> 'Money':
        return cls(amount, 'KES')


rent = Money.in_shillings(25_000)   # better than Money(25_000, 'KES')
```

**[⬆ back to top](#table-of-contents)**

### Pick-One-Word-per-Concept

---

Pick one word for one abstract concept and stay with it across the whole codebase. `fetch`, `retrieve` and
`get` as equivalent methods on different classes is a puzzle: the reader has to remember which class uses
which word, and every difference in wording suggests a difference in behaviour that is not there.

**Bad** :angry:

```python
class CustomerRepository:
    def fetch(self, customer_id: str) -> Customer: ...

class OrderRepository:
    def retrieve(self, order_id: str) -> Order: ...

class InvoiceRepository:
    def get(self, invoice_id: str) -> Invoice: ...
```

**Good** :smiley:

```python
class CustomerRepository:
    def get(self, customer_id: str) -> Customer: ...

class OrderRepository:
    def get(self, order_id: str) -> Order: ...

class InvoiceRepository:
    def get(self, invoice_id: str) -> Invoice: ...
```

Now the API is guessable: once you have used one repository you have used them all. A consistent lexicon is
a gift to every reader who comes after you.

The same holds for the objects themselves. Do not have a `Controller`, a `Manager` and a `Driver` in one
system if they all play the same role. Write down the vocabulary somewhere (a `CONTRIBUTING.md`, a glossary,
a docstring in `__init__.py`) and treat it as part of the design.

**[⬆ back to top](#table-of-contents)**

### Don't-Pun

---

The previous rule has a twin. Using one word for **two** concepts is a pun, and it is just as confusing as
using two words for one concept.

Suppose `add` in your codebase has always meant "return a new value by joining two existing values". Now you
need a method that puts a single item into a collection. Calling it `add` is a pun: the word is the same,
the semantics are not.

**Bad** :angry:

```python
class ShoppingCart:
    def add(self, other: 'ShoppingCart') -> 'ShoppingCart':
        """Combine two carts into a new one."""

    def add(self, item: Item) -> None:          # same word, different concept
        """Put one item into this cart."""
```

**Good** :smiley:

```python
class ShoppingCart:
    def merge(self, other: 'ShoppingCart') -> 'ShoppingCart':
        """Combine two carts into a new one."""

    def append(self, item: Item) -> None:
        """Put one item into this cart."""
```

Python makes the cost concrete: two methods with the same name in one class means the second one silently
replaces the first. Where you genuinely have one concept with several input shapes, use
`functools.singledispatchmethod` or an explicit classmethod, not a pun.

> Author names like a technical writer, not like a poet. The goal is a codebase where a reader can guess
> right without looking anything up.

**[⬆ back to top](#table-of-contents)**

### Use-Solution-Domain-Names

---

The people reading your code are programmers. Use the vocabulary of computer science, algorithms, patterns
and mathematics where it fits: it is precise, and it saves you from inventing a worse word for something that
already has a name.

**Bad** :angry:

```python
class JobHolderThatKeepsThingsInOrder:
    def put_in(self, job): ...
    def take_out(self): ...
```

**Good** :smiley:

```python
from queue import PriorityQueue


class JobQueue:
    def __init__(self) -> None:
        self._jobs: PriorityQueue = PriorityQueue()

    def enqueue(self, job: 'Job') -> None: ...
    def dequeue(self) -> 'Job': ...
```

A reader who knows what a priority queue is now understands the class in one line, including its performance
characteristics.

The same applies to design patterns. `AccountVisitor`, `RetryDecorator` and `ConnectionPool` all import a
whole chapter of shared understanding for free.

```python
class LoggingPaymentDecorator(Payment):     # says exactly what it is
    ...
```

Just make sure the name is true. Calling something a `Factory` when it is not one is disinformation of the
worst kind, because it is *confident* disinformation.

**[⬆ back to top](#table-of-contents)**

### Use-Problem-Domain-Names

---

When there is no computer science term for what you are doing, use the language of the business. Code that
speaks the domain's vocabulary lets a domain expert read it and spot a bug you cannot see.

**Bad** :angry:

```python
def process(record: dict) -> float:
    value = record['amount'] * 0.16
    return value
```

**Good** :smiley:

```python
from decimal import Decimal
from typing import Final

VAT_RATE: Final[Decimal] = Decimal('0.16')


def value_added_tax(invoice: 'Invoice') -> Decimal:
    return invoice.taxable_amount * VAT_RATE
```

An accountant can read the second version and tell you whether 16% is still the rate, and whether the base
should really be the taxable amount. They can do nothing with the first.

Getting the vocabulary right is a design activity, not a naming afterthought: the words the business uses
usually map onto the classes the system needs. If the business says "a policy is underwritten and then
issued", expect a `Policy` with `underwrite()` and `issue()`, not a `PolicyManager` with `handle_state()`.

> **The rule of thumb:** if the concept has a name in computer science, use it. If it does not, use the name
> the domain experts use. Never invent a third word.

**[⬆ back to top](#table-of-contents)**

### Add-Meaningful-Context

---

Very few names are meaningful on their own. `state` could be a US state, an HTTP status or a state machine's
current node. Most names need context, and the question is where to put it.

The weakest option is prefixing (see [Gratuitous Context](#gratuitous-context)). Better options, roughly in
order of strength:

**1. A well named enclosing function**

**Bad** :angry:

```python
def print_guess_statistics(candidate: str, count: int) -> None:
    if count == 0:
        number = 'no'
        verb = 'are'
        plural_modifier = 's'
    elif count == 1:
        number = '1'
        verb = 'is'
        plural_modifier = ''
    else:
        number = str(count)
        verb = 'are'
        plural_modifier = 's'
    print(f'There {verb} {number} {candidate}{plural_modifier}')
```

The three variables are only meaningful together, and you have to read the whole function to see that.

**2. A class that names the group**

**Good** :smiley:

```python
class GuessStatisticsMessage:
    def __init__(self, candidate: str, count: int) -> None:
        self._candidate = candidate
        self._count = count

    def __str__(self) -> str:
        return f'There {self._verb} {self._number} {self._candidate}{self._plural_modifier}'

    @property
    def _verb(self) -> str:
        return 'is' if self._count == 1 else 'are'

    @property
    def _number(self) -> str:
        return 'no' if self._count == 0 else str(self._count)

    @property
    def _plural_modifier(self) -> str:
        return '' if self._count == 1 else 's'
```

The class name supplies the context, so each part can have a short name and each part can be tested.

**3. Group related fields into a type instead of repeating a prefix**

**Bad** :angry:

```python
def ship(street: str, city: str, state: str, postal_code: str, country: str) -> None: ...
```

**Good** :smiley:

```python
from dataclasses import dataclass


@dataclass(frozen=True)
class Address:
    street: str
    city: str
    state: str          # unambiguous now: it is part of an Address
    postal_code: str
    country: str


def ship(destination: Address) -> None: ...
```

The `Address` type gives `state` its meaning, shrinks the signature from five arguments to one (see
[Function Arguments](#function-arguments)) and gives you somewhere to put validation.

**4. The module and package**

`billing/invoice.py` needs no `Billing` prefix on `Invoice`. Let the import path carry the context:
`from billing.invoice import Invoice`.

> Only add context when the name genuinely needs it. `Address` does not need to become `CustomerAddress`
> unless you also have a `WarehouseAddress` to tell it apart from.

**[⬆ back to top](#table-of-contents)**

## **Functions**

### **Small**

---

The first rule of functions is that they should be small. The second rule is that they should be smaller
than that.

There is no magic number, but there is a reliable signal: a function you can take in **without scrolling**
and **without holding state in your head** is small enough. In practice that lands somewhere between three
and fifteen lines for most Python.

Length is a symptom, not the disease. A long function is long because it is doing several things, and each
of those things is a concept that deserves a name. Extracting them is not busywork; it is how the concepts
become visible.

**Bad** :angry:

```python
def send_invoices(customers, tax_rate, smtp_host, dry_run=False):
    results = []
    for customer in customers:
        if not customer.get('active'):
            continue
        subtotal = 0.0
        for line in customer['orders']:
            if line['status'] == 'shipped':
                subtotal += line['price'] * line['quantity']
        if subtotal == 0:
            continue
        tax = subtotal * tax_rate
        total = subtotal + tax
        body = f"Dear {customer['name']},\n\nYou owe {total:.2f} ({tax:.2f} tax).\n"
        if not dry_run:
            server = smtplib.SMTP(smtp_host)
            server.sendmail('billing@example.com', customer['email'], body)
            server.quit()
        results.append((customer['email'], total))
    return results
```

To review that function you have to keep four unrelated jobs in your head at once: filtering customers,
summing orders, formatting an email and talking to an SMTP server. Note also the flag argument (`dry_run`),
which is a confession that the function does two things.

**Good** :smiley:

```python
from decimal import Decimal
from typing import Iterable, List


def send_invoices(customers: Iterable[Customer], tax_rate: Decimal, mailer: Mailer) -> List[Invoice]:
    invoices = [invoice_for(customer, tax_rate) for customer in customers if is_billable(customer)]
    for invoice in invoices:
        mailer.send(invoice.recipient, render_invoice(invoice))
    return invoices


def is_billable(customer: Customer) -> bool:
    return customer.is_active and amount_due(customer) > 0


def amount_due(customer: Customer) -> Decimal:
    return sum(
        (order.price * order.quantity for order in customer.orders if order.is_shipped),
        start=Decimal('0'),
    )


def invoice_for(customer: Customer, tax_rate: Decimal) -> Invoice:
    subtotal = amount_due(customer)
    return Invoice(recipient=customer.email, subtotal=subtotal, tax=subtotal * tax_rate)


def render_invoice(invoice: Invoice) -> str:
    return f'Dear customer,\n\nYou owe {invoice.total:.2f} ({invoice.tax:.2f} tax).\n'
```

Every function now fits on a screen, and the `dry_run` flag disappeared: passing a different `Mailer` (a
real one in production, a recording fake in tests) covers the same need without a branch.

Two Python-specific notes:

- **Blocks inside `if`, `for` and `while` should usually be one line long** — ideally a call to a well named
  function. That keeps nesting shallow, and nesting is what makes a function hard to read.
- **Indentation should rarely exceed two levels.** Three or more nested blocks means there is a function
  hiding in there. Guard clauses and early `return`s flatten most of them.

**Bad** :angry:

```python
def grade(student):
    if student is not None:
        if student.is_enrolled:
            if student.marks:
                return sum(student.marks) / len(student.marks)
    return None
```

**Good** :smiley:

```python
def grade(student: Student | None) -> float | None:
    if student is None or not student.is_enrolled or not student.marks:
        return None
    return sum(student.marks) / len(student.marks)
```

**[⬆ back to top](#table-of-contents)**

### Do-One-Thing

---

> **Functions should do one thing. They should do it well. They should do it only.**

The hard part is agreeing on what "one thing" means, because every function can be described as several
smaller steps. Two tests work well in practice:

1. **Can you extract another function from it whose name is not merely a restatement of its body?** If yes,
   the function was doing more than one thing.
2. **Can you describe it in one sentence with no "and", "then" or "or"?** "Validate the trade *and* store
   it" is two things.

**Bad** :angry:

```python
def register(email: str, password: str) -> str:
    if '@' not in email:
        raise ValueError('bad email')
    if len(password) < 8:
        raise ValueError('weak password')

    hashed = hashlib.sha256(password.encode()).hexdigest()

    connection = sqlite3.connect('users.db')
    connection.execute('INSERT INTO users VALUES (?, ?)', (email, hashed))
    connection.commit()

    smtplib.SMTP('localhost').sendmail(
        'noreply@example.com', email, 'Subject: Welcome!\n\nThanks for joining.'
    )
    return email
```

This function validates, hashes, persists **and** sends mail. It has four reasons to change, it cannot be
tested without a database and an SMTP server, and there is no way to reuse the validation on its own.

**Good** :smiley:

```python
def register(email: str, password: str, users: UserRepository, mailer: Mailer) -> User:
    validate_credentials(email, password)
    user = User(email=email, password_hash=hash_password(password))
    users.add(user)
    mailer.send_welcome(user)
    return user


def validate_credentials(email: str, password: str) -> None:
    if '@' not in email:
        raise InvalidEmail(email)
    if len(password) < MIN_PASSWORD_LENGTH:
        raise WeakPassword()


def hash_password(password: str) -> str:
    return hashlib.sha256(password.encode()).hexdigest()
```

`register` still mentions four steps, but it no longer *performs* them: it is one level of policy, delegating
to one level of detail. That is exactly the shape the next section is about.

Sections within a function are the clearest sign of a violation. If you find yourself writing
`# --- validation ---` and `# --- persistence ---` comments, you have found your extraction points; the
comment is trying to tell you the function's name.

**[⬆ back to top](#table-of-contents)**

### One-level-of-Abstraction

---

Mixing levels of abstraction inside one function forces the reader to constantly change altitude: one line
is about business policy, the next is about string slicing, the one after is about socket timeouts. Each
switch costs attention.

**Bad** :angry:

```python
def publish_report(rows: list[dict]) -> None:
    html = '<table>'
    for row in rows:                                    # low level: string building
        html += '<tr>' + ''.join(f'<td>{v}</td>' for v in row.values()) + '</tr>'
    html += '</table>'

    if datetime.now().weekday() >= 5:                   # high level: business rule
        return

    with open('/var/www/report.html', 'w') as handle:    # low level: file system
        handle.write(html)
    notify_subscribers()                                # high level again
```

**Good** :smiley:

```python
def publish_report(rows: list[dict]) -> None:
    if is_weekend():
        return
    write_report(render_html(rows))
    notify_subscribers()


def render_html(rows: list[dict]) -> str:
    body = ''.join(render_row(row) for row in rows)
    return f'<table>{body}</table>'


def render_row(row: dict) -> str:
    cells = ''.join(f'<td>{value}</td>' for value in row.values())
    return f'<tr>{cells}</tr>'


def write_report(html: str) -> None:
    Path('/var/www/report.html').write_text(html)
```

`publish_report` now reads like a summary of the process. Each function it calls is one step lower, and each
of those is again internally consistent.

#### The Stepdown Rule

Code reads best as a top-down narrative. Every function should be followed by those at the next level of
abstraction, so that reading the module is like reading a set of "TO" paragraphs:

> **To** publish a report, we skip weekends, render the rows to HTML, write the file and notify subscribers.
> &nbsp;&nbsp;**To** render rows to HTML, we render each row and wrap them in a table.
> &nbsp;&nbsp;&nbsp;&nbsp;**To** render a row, we wrap each value in a cell.

Following the stepdown rule means the reader can stop at any depth. Someone reviewing a business rule reads
the first function and leaves; someone debugging the markup keeps descending.

**[⬆ back to top](#table-of-contents)**

### Avoid Conditionals

---

Let us meet Joe. Joe is a junior web developer who works at a certain company in Nairobi. Joe's company has got a new client who wants Joe's company to build him an application to manage his bank.

The client specifies that this application will manage user bank accounts. Joe organizes a meeting with the client and they agree to meet so that Joe can collect the client's business needs. Let us watch Joe as he puts his OOP programming skills to work.

After their meeting, they agree that the user account will be able to accomplish the behaviour specified in the figure below.

![Account class](assets/Account_class.PNG)

The code below provides the implementation details of this class.

```python
class Account:
    def __init__(self, acc_number: str, amount: float, name: str):
        self.acc_number = acc_number
        self.amount = amount
        self.name = name

    def get_balance(self) -> float:
        return self.amount

    def __eq__(self, other: Account) -> bool:
        if isinstance(other, Account):
            return self.acc_number == other.acc_number

    def deposit(self, amount: float) -> bool:
        if amount > 0:
            self.amount += amount

    def withdraw(self, amount: float) -> None:
        if (amount > 0) and (amount <= self.amount):
            self.amount -= amount

    def _has_enough_collateral(self, loan: float) -> bool:
        if loan < self.amount / 2:
            return True

    def __str__(self) -> str:
        return f'Account acc number : {self.acc_number} amount : {self.amount}'

    def add_interest(self) -> None:
        self.deposit(0.1 * self.amount)

    def get_loan(self, amount : float) -> bool:
        if self._has_enough_collateral(amount):
            return True
        else:
            return False
```

The application is a success and after a month, the client comes back to Joe asking for more features. The client says that he now wants the application to work with more than one type of account. The application should now process SavingsAccount and CheckingAccount accounts. The difference between them is outlined below.

- When authorizing a loan, a checking account needs a
  balance of two thirds the loan amount, whereas savings accounts require only one half the loan amount.

- The bank gives periodic interest to savings accounts but not checking accounts.

- The representation of an account will return
  “Savings Account” or “Checking Account,” as appropriate.

Joe rolls up his sleeves and starts to make modifications to the original Account class to introduce the new features. Below is his approach.

**Bad** :angry:

```python
class Account:
    def __init__(self, acc_number: str, amount: float, name: str, type : int):
        self.acc_number = acc_number
        self.amount = amount
        self.name = name
        self.type = type

    def _has_enough_collateral(self, loan: float) -> bool:
        if self.type == 1:
            return self.amount >= loan / 2;
        elif selt.type == 2:
            return self.amount >= 2 * loan / 3;
        else:
            return False

    def __str__(self) -> str:
        if self.type == 1:
            return ' SavingsAccount'
        elif self.type == 2:
            return 'CheckingAccount'
        else:
            return 'InvalidAccount'

    def add_interest(self) -> None:
        if self.type == 1: self.deposit(0.1 * self.amount)


    def get_loan(self, amount : float) -> bool:
        True if self._has_enough_collateral(amount) else False

    #... other methods
```

> **Note:** We have only shown the methods that changed.

With this implementation, Joe is happy and he ships the app into production since it works as the client had wanted. But something has really gone wrong here.

![conditionals](assets/code_smell.png)

The problem are these conditionals here. They work for now but they will cause a maintenance nightmare very soon. What will happen if the client comes back asking Joe to add more account types? Joe will have to open this class and add more IFs. What happens of the client asks him to delete some of the account types? He will open the same class and edit all Ifs again.

This class is now violating the **Single Responsibility Principle** and the **Open Closed Principle**. The class has more than one reason to change and still, it is not closed for modification and
these IFs may also run slow.

This smell is called the **Missing Hierarchy** smell.

> **Missing Hierarchy** <br/>
> This smell arises when a code segment uses conditional logic (typically in conjunction
> with “tagged types”) to explicitly manage variation in behavior where a hierarchy
> could have been created and used to encapsulate those variations.

To solve this problem, we will need to introduce an hierarchy of account types.
We will achieve this by creating a super abstract class Account and implement all the common methods but mark the account specific methods abstract.
Different account types can then inherit from this base class.

![IF_refactor](assets/IF_Refactor.PNG)

With this new approach, account specific methods will be implemented by subclasses and note that we will throw away those annoying IFs and replace them with polymorphism hence the **Replace Conditionals with Polymorphism** rule.

Below are the implementation of Account, SavingsAccount and CheckingAccount.

![abstract methods](assets/Capture.png)

**SavingsAccount class**

**Good** :smiley:

```python
class SavingAccount(Account):
    def __init__(self, acc_number: str, amount: float, name: str):
        Account.__init__(self, acc_number, amount, name)

    def __eq__(self, other: SavingsAccount) -> bool:
         if isinstance(other, SavingsAccount):
            return self.acc_number == other.acc_number


    def _has_enough_collateral(self, loan: float) -> bool:
        return self.amount >= loan / 2;

    def get_loan(self, amount : float) -> bool:
        return _has_enough_collateral(float)

    def __str__(self) -> str:
        return f'Saving Account acc number : {self.acc_number}'

    def add_interest(self) -> None:
        self.deposit(0.1 * self.amount)
```

**CheckingAccount class**

**Good** :smiley:

```python
class CheckingAccount(Account):
    def __init__(self, acc_number: str, amount: float, name: str):
        Account.__init__(self, acc_number, amount, name)

    def __eq__(self, other: SavingsAccount) -> bool:
         if isinstance(other, CheckingAccount):
            return self.acc_number == other.acc_number


    def has_enough_collateral(self, loan: float) -> bool:
        return self.amount >= 2 * loan / 3;

    def get_loan(self, amount : float) -> bool:
        return _has_enough_collateral(float)

    def __str__(self) -> str:
        return f'Checking Account acc number : {self.acc_number}'

    #empty method.
    def add_interest(self) -> None:
        pass
```

Notice that each branch of the original annoying `if else` is now implemented in its class. Now if the client comes back and asks Joe to add a fixed deposit account, Joe will just create a new class called FixedDeposit and it will inherit from that abstract Account class. With this design, note that :

- To add new functionality, we add more classes and ignore all existing classes. This is the **Open Closed Principle.**

Note that the CheckingAccount class leaves the add_interest method empty. This is a code smell known as the **Rebellious Hierarchy** design smell and we shall fix it later when we get to the **Interface Segregation Principle**.

> **REBELLIOUS HIERARCHY** <br>
> This smell arises when a subtype rejects the methods provided by its supertype(s).
> In this smell, a supertype and its subtypes conceptually share an IS-A relationship,
> but some methods defined in subtypes violate this relationship. For example, for
> a method defined by a supertype, its overridden method in the subtype could:
>
> - throw an exception rejecting any calls to the method
> - provide an empty (or NOP i.e., NO Operation) method
> - provide a method definition that just prints “should not implement” message
> - return an error value to the caller indicating that the method is unsupported.

After a year, Joe's client comes back and asks Joe to add a Current account. Guess what Joe does?? You guessed right, he just creates a new class for this new account and inherits from Account class as shown in the figure below.
![CurrentAccount](assets/CurrentAccount.png)

**[⬆ back to top](#table-of-contents)**

### Use-Descriptive-Names

---

> **You know you are working on clean code when each routine turns out to be pretty much what you expected.**
> — Ward Cunningham

Half the battle of making code readable is choosing good names for the things it does. Everything in
[Naming Things](#naming-things) applies here, with three additions specific to functions.

**1. A long descriptive name beats a short enigmatic one, and beats a long comment.**

**Bad** :angry:

```python
def calc(d):
    """Calculate the number of days a payment is overdue."""
    ...
```

**Good** :smiley:

```python
def days_overdue(due_date: date) -> int:
    ...
```

The docstring became unnecessary because the signature says it.

**2. Do not fear renaming.** Modern editors rename symbols reliably and your tests will catch what they
miss. Names are cheap to change and expensive to leave wrong.

**3. Be consistent.** Use the same phrases, nouns and verbs across a module so that the API is guessable
(see [Pick One Word per Concept](#pick-one-word-per-concept)):

```python
find_by_id(...)      find_by_email(...)      find_all(...)
```

A useful trick: **name the function after what it returns, or after the effect it has** — never after how it
does it.

**Bad** :angry:

```python
def loop_through_orders_and_add(orders): ...    # describes the implementation
```

**Good** :smiley:

```python
def total_value(orders: list[Order]) -> Decimal: ...
```

The second name survives the day you replace the loop with a comprehension or a database `SUM`.

**[⬆ back to top](#table-of-contents)**

### Function-Arguments

---

The ideal number of arguments for a function is zero. Next comes one, then two. Three should be avoided
where possible, and more than three needs a very good reason.

Arguments are hard for two reasons: each one is a concept the reader has to hold while reading the name, and
each one multiplies the number of cases a test has to cover.

#### Common forms

**Niladic (zero arguments).** The easiest to understand — but in a function (rather than a method) it usually
means the input arrives through a global or through I/O, which is a side effect. See
[Niladic Functions](#1-niladic-functions).

**Monadic (one argument).** There are two good reasons to pass a single argument: asking a question about it
(`is_overdrawn(account)`), or transforming it into something else (`parse_date(text)`). A third form, an
*event* (`notify_password_changed(user)`), takes an input and changes state without returning anything; use
it deliberately and make the name say so.

**Dyadic (two arguments).** Fine when the two arguments have a natural order or are two halves of one value
— `Point(x, y)`, `assert_equal(expected, actual)`. Awkward when they do not, because the reader has to
remember the order. `write_field(output_stream, name)` is a dyad that would read better as a method:
`output_stream.write_field(name)`.

**Triadic and beyond.** Usually a sign that some of the arguments belong together in an object.

**Bad** :angry:

```python
def create_circle(x: float, y: float, radius: float) -> Circle: ...

create_circle(0, 0, 5)
```

**Good** :smiley:

```python
@dataclass(frozen=True)
class Point:
    x: float
    y: float


def create_circle(center: Point, radius: float) -> Circle: ...

create_circle(Point(0, 0), radius=5)
```

Wrapping `x` and `y` in `Point` is not cheating: those two values genuinely are one concept, and now they
have a name and a place for behaviour like `distance_to`.

#### Flag arguments are ugly

Passing a boolean into a function loudly proclaims that the function does more than one thing: one thing if
the flag is true, another if it is false.

**Bad** :angry:

```python
def render(page: Page, is_suite: bool) -> str:
    if is_suite:
        ...
    else:
        ...
```

**Good** :smiley:

```python
def render_for_suite(page: Page) -> str: ...
def render_for_single_test(page: Page) -> str: ...
```

If you cannot split the function, at least make the flag keyword-only so the call site is readable. Python
gives you this with a bare `*`:

```python
def dump(data: dict, *, indent: bool = False) -> str: ...

dump(payload, indent=True)      # dump(payload, True) is now a TypeError
```

#### Mutable default arguments

This is the classic Python trap, and it is an argument bug rather than a style issue: the default is
evaluated **once**, at function definition time, so every call shares the same list.

**Bad** :angry:

```python
def append_item(item: str, items: list[str] = []) -> list[str]:
    items.append(item)
    return items

append_item('a')    # ['a']
append_item('b')    # ['a', 'b']  <- the same list, still there
```

**Good** :smiley:

```python
def append_item(item: str, items: list[str] | None = None) -> list[str]:
    items = [] if items is None else items
    return [*items, item]
```

#### Prefer keyword arguments at the call site

A call that reads as a sentence needs no comment:

```python
transfer(account_a, account_b, 500)                             # which way round?
transfer(source=account_a, destination=account_b, amount=500)   # obvious
```

#### `*args` and `**kwargs`

Variadic arguments are fine when the function truly treats them uniformly (`print`, `max`, `sum`). They are
a problem when used to paper over an unclear interface, because they erase the signature: neither the reader
nor the type checker can see what the function accepts.

```python
def total(*amounts: Decimal) -> Decimal:        # good: all arguments are the same thing
    return sum(amounts, start=Decimal('0'))


def do_stuff(*args, **kwargs):                  # bad: what does it take? nobody knows
    ...
```

**[⬆ back to top](#table-of-contents)**

### Avoid-Side-Effects

#### **Pure Functions**

---

What on earth is a pure function?? Well, adequately put, a pure function is one without side effects.
Side effects are invisible inputs and outputs from functions. In pure Functional programming,functions behave like mathematical functions. Mathematical functions are transparent-- they will always return the same output when given the same input. Their output only depends on their inputs.

Below are examples of functions with side effects:

#### 1. **Niladic-Functions**

---

**Bad** :angry:

```python
class Customer:
    def __init__(self, first_name : str)-> None:
        self.first_name = first_name

    #This method is impure, it depends on global state.
    def get_name(self):
        return self.first_name

    #more code here
```

Niladic functions have this tendency to depend on some invisible input especially if such a function is member function of a class. Since all class members share the same class variables, most methods aren't pure at all. Class variable values will always depend on which method was called last. In a nutshell, most niladic functions depend on some **global state** in this case `self.first_name`.

The same can be said to functions that return None. These too aren't pure functions. If a function doesn't return, then it is doing something that is affecting global state. **Such functions can not be composed in fluent APIs.** The sort method of the list class has side effects, it changes the list in place whereas the sorted builtin function has not side effects because it returns a new list.

> `sort()` and `reverse()` are now discouraged and instead using the built-in `reversed()` and `sorted()` are encouraged.

**Bad** :angry:

```python
names = ['Kasozi', 'Martin', 'Newton', 'Grady']

#wrong: sorted_names now contains None
sorted_names = names.sort()

#correct: sorted_names now contain the sorted list
sorted_names = sorted(names)
```

> **static methods** <br>
> One way to solve this problem is to use static methods inside a class. Static methods know nothing about the class data and hence their outputs only depend on their inputs.

#### 2. **Argument Mutation**

---

Functions that mutate their input arguments aren't pure functions. This becomes more pronounced when we run on multiple cores. More than one function may be reading from the same variable and each function can be context switched from the CPU at any time. If it was not yet done with editing the variable, others will read garbage.

**Bad** :angry:

```python
from typing import List

Marks = List[int]

marks = [43, 78, 56, 90, 23]

def sort_marks(marks : Marks) -> None:
    marks.sort()

def calculate_average(marks : Marks) -> float:
    return sum(marks)/float(len(marks))
```

From the above code snippet, we have two functions that both read the same list. `sort_marks()` mutates its input argument and this is not good. Now imagine a scenario when `calculate_average_mark()` was running and before it completed, it was context switched and `sort_marks()` allowed to run.

sort_marks will update the list in place and change the order of elements in the list, by the time `calculate_average_average()` will run again, it will be reading garbage.

**Good :smiley:**

```python
from typing import List

Marks = List[int]

marks = [43, 78, 56, 90, 23]

#sort_marks now returns a new list and uses the sorted function

#Mutates input argument
def sort_marks(marks : Marks) -> Marks:
    return sorted(marks)

# Doesn't mutate input argument
def find_average_mark(marks : Marks) -> float:
    return sum(marks)/len(marks)
```

This problem can also be solved by using immutable data structures.

> Function purity is also vital for unit-testing. Impure functions are hard to test especially if the side effect has to do with I/O. Unlike mutation, you can’t avoid side effects related to I/O; whereas mutation is an implementation detail, I/O is usually a requirement.

#### 3. **Exceptions**

---

Some function signatures are more expressive than others, by which I mean that they give us
more information about what the function is doing, what inputs are permissible, and what outputs we can expect. The signature `() → ()`, for example, gives us no information at all: it may print some text, increment a counter, launch a spaceship... who knows! On the other hand, consider this signature:

`(List[int], (int → bool)) → List[int]`

Take a minute and see if you can guess what a function with this signature does. Of course, you
can’t really know for sure without seeing the actual implementation, but you can make an
educated guess. The function returns a list of `ints` as input; it also takes a list of `ints`, as well as a
second argument, which is a function from int to `bool`: a predicate on int.

But is not honest enough. What happens if we pass in an empty list?? This function may throw an exception.

> Exceptions are hidden outputs from functions and functions that use exceptions have side effects.

**Bad** :angry:

```python
def find_quotient(first : int, second : int)-> float:
    try:
        return first/second
    except ZeroDivisionError:
        return None
```

What is wrong with such a function? In its signature, it claims to return a float but we can see that sometimes it fails. Such a function is not honest and such functions should be avoided.

> Functiona languages handle errors using other means like Monads and Options. Not with exceptions.

#### 4. **I/O**

---

Functions that perform input/output aren't pure too. Why? This is because they return different outputs when given the same input argument. Let me explain more about this. Imagine a function that takes in an URL and returns HTML, if the HTML is changed, the function will return a different output but it is still taking in the same URL. Remember mathematical functions don't behave like this.

**Bad** :angry:

```python
def read_HTML(url : str)-> str:
    try:
        with open(url) as file:
            data = file.read()
        data = file.read()
        return data
    except FileNotFoundError:
        print('File Not found')
```

This function is plagued with more than one problem.

- Its signature is not honest. It claims that the function returns a string and takes in a string but from the implementation, we see it can fail.
- This function is performing IO. IO operations produce side effects and thus this function is not pure.

> You can build pure functions in python with the help of the **operator** and **functools** modules. There is a package **fn.py** to support functional programming in Python 2 and 3. According
> to its author, Alexey Kachayev, fn.py provides “implementation of missing features to
> enjoy FP” in Python. It includes a @recur.tco decorator that implements tail-call optimization
> for unlimited recursion in Python, among many other functions, data structures,
> and recipes.

### Command-Query-Separation

---

Functions should either **do** something or **answer** something, but not both. A function that changes the
state of an object is a *command*; a function that reports something about an object is a *query*. Mixing
the two leads to call sites that are impossible to read without opening the function.

**Bad** :angry:

```python
class Account:
    def __init__(self, balance: float) -> None:
        self.balance = balance
        self.is_frozen = False

    def withdraw(self, amount: float) -> bool:
        """Withdraw money and report whether it worked."""
        if self.is_frozen or amount > self.balance:
            return False
        self.balance -= amount
        return True
```

Now read the call site:

```python
if account.withdraw(500):
    ...
```

What does that line mean? "If withdrawing 500 succeeds"? "If we can withdraw 500"? The `if` makes it look
like a question, but money moves as a side effect of asking. Worse, the boolean silently swallows *why* it
failed — a frozen account and insufficient funds are two very different problems for the caller.

**Good** :smiley:

```python
class Account:
    def __init__(self, balance: float) -> None:
        self.balance = balance
        self.is_frozen = False

    def can_withdraw(self, amount: float) -> bool:      # query: no state change
        return not self.is_frozen and amount <= self.balance

    def withdraw(self, amount: float) -> None:          # command: no return value
        if self.is_frozen:
            raise AccountFrozen(self)
        if amount > self.balance:
            raise InsufficientFunds(needed=amount, available=self.balance)
        self.balance -= amount
```

```python
if account.can_withdraw(500):
    account.withdraw(500)
```

Each line now says exactly one thing. Note the second half of the rule at work: **commands report failure by
raising**, not by returning a status code that a caller can forget to check (see
[Exceptions](#3-exceptions)).

#### Recognising a violation

Ask two questions:

- **Can I call this twice in a row and get the same answer?** If not, it is not a pure query.
- **Does an `if` statement around this call change the state of the program?** If yes, you have a
  command-query hybrid.

#### When to break the rule

Sometimes the atomic version is the correct one, because splitting it opens a race window:

```python
if account.can_withdraw(500):     # another thread withdraws here...
    account.withdraw(500)         # ...and this raises
```

For concurrent code, a single atomic operation is right — but then name it as a command and let it raise, or
return a *result object* that describes what happened, rather than a bare boolean:

```python
@dataclass(frozen=True)
class WithdrawalResult:
    succeeded: bool
    new_balance: Decimal
    reason: str | None = None
```

Python's own library shows both styles: `dict.get(key)` is a query, `dict.pop(key)` is deliberately a hybrid
because atomicity matters, and its name (`pop`, not `get`) warns you that it mutates.

**[⬆ back to top](#table-of-contents)**

### Don't Repeat Yourself (DRY)

---

Let us imagine that we are working on a banking application. We all know that such an application will manipulate bank account objects among other things.
Let us assume that at the start of the project, we have only two types of accounts to work with;

- Savings Account
- Checking Account

We roll up our sleeves and put our OOP knowledge to test. We craft two classes to model both and Savings and Checking accounts.

**SavingsAccount class**

**Bad** :angry:

```python
class SavingsAccount:
    def __init__(self, acc_number: str, amount: float, name: str):
        self.acc_number = acc_number
        self.amount = amount
        self.name = name

    def get_balance(self) -> float:
        return self.amount

    def __eq__(self, other: SavingsAccount) -> bool:
        if isinstance(other, SavingsAccount):
            return self.acc_number == other.acc_number

    def deposit(self, amount: float) -> bool:
        if amount > 0:
            self.amount += amount

    def withdraw(self, amount: float) -> None:
        if (amount > 0) and (amount <= self.amount):
            self.amount -= amount

    def has_enough_collateral(self, loan: float) -> bool:
        if loan < self.amount / 2:
            return True

    def __str__(self) -> str:
        return f'Saving Account acc number : {self.acc_number}'

    def add_interest(self) -> None:
        self.deposit(0.1 * self.amount)
```

**CheckingAccount class**

**Bad** :angry:

```python
class CheckingAccount:
    def __init__(self, acc_number: str, amount: float, name: str):
        self.acc_number = acc_number
        self.amount = amount
        self.name = name

    def get_balance(self) -> float:
        return self.amount

    def __eq__(self, other: SavingsAccount) -> bool:
        if isinstance(other, SavingsAccount):
            return self.acc_number == other.acc_number

    def deposit(self, amount: float) -> bool:
        if amount > 0:
            self.amount += amount

    def withdraw(self, amount: float) -> None:
        if (amount > 0) and (amount <= self.amount):
            self.amount -= amount

    def has_enough_collateral(self, loan: float) -> bool:
        if loan < self.amount / 5:
            return True

    def __str__(self) -> str:
        return f'Checking Account acc number : {self.acc_number}'

    def add_interest(self) -> None:
        self.deposit(0.5 * self.amount)
```

The table describes all the methods added to both classes.

| method                    | description                                         |
| ------------------------- | --------------------------------------------------- |
| `get_balance()`           | returns the account balance                         |
| `__str__()`               | returns the string representation of account object |
| `add_interest()`          | adds a given interest to a given account            |
| `has_enough_collateral()` | checks if the account can be granted a loan         |
| `withdraw()`              | withdraws a given amount from the account           |
| `deposit()`               | deposits an amount to the account                   |
| `__eq__()`                | checks if 2 accounts are the same                   |

The Unified Modeling Language (UML) class diagrams of both classes are shown below. Notice the duplication in method names.

![](assets/umlsc.PNG)

If you look more closely, both these classes contain the same methods and to make it worse, most of these methods contain exactly the same code. This is a **bad** practice and it leads to a maintenance nightmare. Identical code is littered in more than one place and so if we ever make changes to one of the copies, we have to change all the others.

There is a software principle that helps in solving such a problem and this principle is known as **DRY** for Don't Repeat Yourself.

> The “Don’t Repeat Yourself” Rule <br>
> A piece of code should exist in exactly one place.

It is evident from our bad design that we have two classes that both claim to do same thing really well and so we just violated the **Most Qualified Rule**. In most cases, such scenarios arise due to failing to identify similarities between objects in a system.

To solve this problem, we will use inheritance. We will define a new abstract class called BankAccount and we will implement all the method containing the similar logic in this abstract class. Then we will leave the different methods to be implemented by subclasses of BankAccount.

Below is the UML diagram for our new design.

![Inheritance Hierachy of SavingsAccount and CheckingAccount](assets/inheritance.PNG)

**BankAccount** class

**Good** :smiley:

```python
from abc import ABC, abstractmethod

class BankAccount(ABC):
    def __init__(self, acc_number: str, amount: float, name: str):
        self.acc_number = acc_number
        self.amount = amount
        self.name = name

    def get_balance(self) -> float:
        return self.amount

    def deposit(self, amount: float) -> bool:
        if amount > 0:
            self.amount += amount

    def withdraw(self, amount: float) -> None:
        if (amount > 0) and (amount <= self.amount):
            self.amount -= amount

    @abstractmethod
    def __eq__(self, other: SavingsAccount) -> bool:
        pass

    @abstractmethod
    def has_enough_collateral(self, loan: float) -> bool:
        pass

    @abstractmethod
    def __str__(self) -> str:
        pass

    @abstractmethod
    def add_interest(self) -> None:
        pass
```

> **Note :** In the BankAccount abstract class, the methods `__eq__()`, `has_enough_collateral()`, `__str__()` and `add_interest()` are abstract and so it is the responsible of subclasses to implement them.

**SavingsAccount class**

**Good** :smiley:

```python
class SavingAccount(BankAccount):
    def __init__(self, acc_number: str, amount: float, name: str):
        BankAccount.__init__(self, acc_number, amount, name)

    def __eq__(self, other: SavingsAccount) -> bool:
         if isinstance(other, SavingsAccount):
            return self.acc_number == other.acc_number


    def has_enough_collateral(self, loan: float) -> bool:
        if loan < self.amount / 2:
            return True


    def __str__(self) -> str:
        return f'Saving Account acc number : {self.acc_number}'


    def add_interest(self) -> None:
        self.deposit(0.1 * self.amount)
```

**CheckingAccount class**

**Good** :smiley:

```python
class CheckingAccount(BankAccount):
    def __init__(self, acc_number: str, amount: float, name: str):
        BankAccount.__init__(self, acc_number, amount, name)

    def __eq__(self, other: SavingsAccount) -> bool:
         if isinstance(other, CheckingAccount):
            return self.acc_number == other.acc_number


    def has_enough_collateral(self, loan: float) -> bool:
        if loan < self.amount / 5:
            return True


    def __str__(self) -> str:
        return f'Checking Account acc number : {self.acc_number}'


    def add_interest(self) -> None:
        self.deposit(0.5 * self.amount)
```

With this new design, if we ever want to modify the methods common to both classes, we only edit them in the abstract class. This simplifies our codebase maintenance. In fact, this was of organizing code is so ideal for implementing the **Replace Ifs with Polymorphism (RIP)** principle as we shall see later.

## **Objects and Data Structures**

---

There is a distinction hiding in most codebases that few people make explicit:

- An **object** hides its data behind abstractions and exposes behaviour that operates on that data.
- A **data structure** exposes its data and has essentially no meaningful behaviour.

Both are legitimate. What causes pain is the hybrid — a class with private attributes, a getter and a setter
for each of them, and no behaviour. That is a data structure wearing an object's coat: it pays the cost of
encapsulation without getting any of the benefit.

**Bad** :angry:

```python
class Vehicle:
    def __init__(self, fuel_tank_capacity: float, fuel_level: float) -> None:
        self._fuel_tank_capacity = fuel_tank_capacity
        self._fuel_level = fuel_level

    def get_fuel_tank_capacity(self) -> float:
        return self._fuel_tank_capacity

    def get_fuel_level(self) -> float:
        return self._fuel_level
```

```python
# every caller has to know how to compute this
percentage = vehicle.get_fuel_level() / vehicle.get_fuel_tank_capacity() * 100
```

The class hides nothing: the callers know the tank is measured in litres and that a percentage is the ratio
of the two. Change the internal representation and every call site breaks.

**Good** :smiley:

```python
class Vehicle:
    def __init__(self, fuel_tank_capacity: float, fuel_level: float) -> None:
        self._fuel_tank_capacity = fuel_tank_capacity
        self._fuel_level = fuel_level

    def percent_fuel_remaining(self) -> float:
        return self._fuel_level / self._fuel_tank_capacity * 100

    def refuel(self, litres: float) -> None:
        self._fuel_level = min(self._fuel_level + litres, self._fuel_tank_capacity)
```

The caller asks the vehicle a question in the vehicle's own terms. How fuel is stored is now genuinely
private and can change freely.

### The Law of Demeter

A method should only talk to its immediate friends: itself, its own attributes, its arguments and objects it
creates. It should not reach through one object to get at another.

**Bad** :angry: — a "train wreck"

```python
output_dir = ctx.get_options().get_scratch_dir().get_absolute_path()
```

That single line couples the caller to `ctx`, to options, to scratch directories and to paths. Any of the
four can break it.

**Good** :smiley:

```python
output_dir = ctx.scratch_directory_path()
```

Ask the object for what you want, do not navigate its internals to get it yourself. A useful phrasing:
**Tell, don't ask.**

**Bad** :angry:

```python
if order.customer.subscription.status == 'active':
    order.apply_discount(0.1)
```

**Good** :smiley:

```python
if order.customer.is_subscribed():
    order.apply_discount(0.1)
```

Note that the law applies to *objects*, not to data structures. `config['db']['host']` is not a violation:
a dict is a data structure and has no behaviour to hide.

### The data and object anti-symmetry

Objects and data structures are opposites, and each is good at exactly what the other is bad at:

- **Objects** make it easy to add new *types* without changing existing functions, and hard to add new
  *operations* (every class must implement them).
- **Data structures** make it easy to add new *operations* without changing existing types, and hard to add
  new *types* (every function must handle them).

This is why "everything should be an object" is bad advice, and so is its opposite. Choose based on which
axis you expect to grow.

```python
# Data structure + functions: easy to add new operations, painful to add a new shape
@dataclass(frozen=True)
class Rectangle:
    width: float
    height: float


@dataclass(frozen=True)
class Circle:
    radius: float


def area(shape: Rectangle | Circle) -> float:
    match shape:
        case Rectangle(width=w, height=h):
            return w * h
        case Circle(radius=r):
            return math.pi * r ** 2
```

```python
# Objects: easy to add a new shape, painful to add a new operation to all of them
class Shape(ABC):
    @abstractmethod
    def area(self) -> float: ...


class Rectangle(Shape):
    def area(self) -> float: ...


class Circle(Shape):
    def area(self) -> float: ...
```

### Data Transfer Objects

The purest data structure is a class with public fields and no functions at all. Python has first class
support for them, and you should use it rather than passing raw dicts around:

```python
from dataclasses import dataclass
from decimal import Decimal


@dataclass(frozen=True)
class OrderLine:
    sku: str
    quantity: int
    unit_price: Decimal
```

Compared with `{'sku': ..., 'quantity': ..., 'unit_price': ...}`, the dataclass gives you a name for the
concept, a place to document it, autocompletion, type checking, equality and an obvious place to add
behaviour when the day comes. `frozen=True` also makes it hashable and safe to share.

For DTOs crossing an untrusted boundary (an HTTP request, a message queue), a validating library such as
`pydantic` is the natural extension of the same idea: parse once at the edge, and pass typed objects
everywhere inside.

> **Rule of thumb:** hide data behind behaviour when the data has rules; expose it plainly when it does not.
> Do not do both.

**[⬆ back to top](#table-of-contents)**

## **Classes**

---

Classes follow the same rules as functions, one size up: they should be **small**, they should do **one
thing**, and their name should say what that thing is.

### Classes should be small

For a function, we count lines. For a class, we count **responsibilities**.

The first hint is the name. If you cannot name a class without using `Manager`, `Processor`, `Super` or
`And`, it probably has more than one responsibility. The second hint is the description: if the sentence
describing the class needs an "and", split it.

**Bad** :angry:

```python
class SuperDashboard:
    def get_last_focused_component(self): ...
    def set_last_focused(self, component): ...
    def get_major_version_number(self) -> int: ...
    def get_minor_version_number(self) -> int: ...
    def get_build_number(self) -> int: ...
    def refresh_widgets(self): ...
    def persist_layout(self): ...
    def send_telemetry(self): ...
```

Version numbers, focus tracking, rendering, persistence and telemetry are five reasons for this class to
change (see the [Single Responsibility Principle](#single-responsibility-principle)).

**Good** :smiley:

```python
@dataclass(frozen=True)
class Version:
    major: int
    minor: int
    build: int

    def __str__(self) -> str:
        return f'{self.major}.{self.minor}.{self.build}'


class FocusTracker:
    def last_focused(self) -> Component: ...
    def record_focus(self, component: Component) -> None: ...


class Dashboard:
    def refresh_widgets(self) -> None: ...
```

A system with many small, single-purpose classes has no more moving parts than a system with a few large
ones — it has the same amount of logic, better organised. The difference is that you only need to understand
the handful of classes involved in the change you are making.

### Cohesion

A class is **cohesive** when its methods use its attributes. In a maximally cohesive class, every method
touches every attribute; that is rare and not a goal, but the trend matters.

Low cohesion is a split waiting to happen: if methods `a` and `b` only touch `self.x`, and methods `c` and
`d` only touch `self.y`, you have two classes sharing a namespace.

**Bad** :angry:

```python
class ReportEngine:
    def __init__(self, rows, smtp_host):
        self.rows = rows            # used only by the two methods below
        self.smtp_host = smtp_host  # used only by send()

    def to_csv(self) -> str:
        return '\n'.join(','.join(map(str, row)) for row in self.rows)

    def row_count(self) -> int:
        return len(self.rows)

    def send(self, body: str, to: str) -> None:
        smtplib.SMTP(self.smtp_host).sendmail('reports@example.com', to, body)
```

**Good** :smiley:

```python
class Report:
    def __init__(self, rows: list[list[str]]) -> None:
        self.rows = rows

    def to_csv(self) -> str:
        return '\n'.join(','.join(row) for row in self.rows)

    def row_count(self) -> int:
        return len(self.rows)


class Mailer:
    def __init__(self, smtp_host: str) -> None:
        self._smtp_host = smtp_host

    def send(self, body: str, to: str) -> None:
        smtplib.SMTP(self._smtp_host).sendmail('reports@example.com', to, body)
```

A class whose cohesion has dropped to zero — no shared state at all — is not a class. In Python that is a
module of functions, and you should write it as one instead of using a class as a namespace:

**Bad** :angry:

```python
class MathUtils:
    @staticmethod
    def mean(values: list[float]) -> float:
        return sum(values) / len(values)
```

**Good** :smiley:

```python
# statistics.py
def mean(values: list[float]) -> float:
    return sum(values) / len(values)
```

### Organising for change

The goal of class design is not to prevent change — it is to make change **local**. When a new requirement
arrives, you want to add a class rather than open several existing ones.

Compare a class that must be edited for every new query type:

**Bad** :angry:

```python
class Sql:
    def create(self, table, columns): ...
    def insert(self, table, fields): ...
    def select_all(self, table): ...
    def select_with_criteria(self, table, criteria): ...
    def _prepare_columns(self, columns): ...      # used by two of the above
```

with a design where a new query is a new class:

**Good** :smiley:

```python
class Sql(ABC):
    @abstractmethod
    def generate(self) -> str: ...


class CreateSql(Sql):
    def generate(self) -> str: ...


class SelectSql(Sql):
    def generate(self) -> str: ...


class SelectWithCriteriaSql(Sql):
    def generate(self) -> str: ...
```

Adding `UpdateSql` now touches no existing code, which is the
[Open/Closed Principle](#openclosed-principleocp) in practice, and each class is independently testable.

### Depend on abstractions, not details

Classes should depend on interfaces rather than concrete implementations, so that the things most likely to
change (a database driver, an HTTP API, the clock) sit behind a boundary you control.

**Bad** :angry:

```python
class PortfolioValuer:
    def total(self) -> Decimal:
        prices = requests.get('https://api.example.com/prices').json()   # hard-wired
        ...
```

That class cannot be tested without a network, and it cannot be reused with a different price source.

**Good** :smiley:

```python
class PriceSource(Protocol):
    def price_of(self, symbol: str) -> Decimal: ...


class PortfolioValuer:
    def __init__(self, prices: PriceSource) -> None:
        self._prices = prices

    def total(self, holdings: dict[str, int]) -> Decimal:
        return sum(
            (self._prices.price_of(symbol) * quantity for symbol, quantity in holdings.items()),
            start=Decimal('0'),
        )
```

Tests inject a fixed price list; production injects the HTTP client. This is the
[Dependency Inversion Principle](#dependency-inversion-principle), and it is what makes the rest of the SOLID
principles usable.

**[⬆ back to top](#table-of-contents)**

## **SOLID Principles**

---

### Single Responsibility Principle

---

The single responsibility principle (SRP) instructs developers to write code that has one and only one
reason to change. If a class has more than one reason to change, it has more than one responsibility.
Classes with more than a single responsibility should be broken down into smaller classes, each of
which should have only one responsibility and reason to change.

It is difficult to overstate the importance of delegating to abstractions. It is the lynchpin of adaptive
code and, without it, developers would struggle to adapt to changing requirements in the way
that Scrum and other Agile processes demand.

Let us meet Vincent. Vincent is a developer and he loves his job really a lot. Vincent loves to keep learning and he buys books that talk about software but he is always busy that he fails to read them. Vincent has a new client that wants an application developed for him.

**_The client wants a program that reads trade records from a file, parse them, log any errors, process the records and them save them to a database._**

The data is stored in the following format. The first 3 capitals are the source currency code, the next 3 capitals are the destination currency code. The first integer is the lot and the last float is the price.

```markdown
UGAUSD,2,45.3
UGAUSD,7,76.4
UGAEUR,7,76.4
HJDSGS,1,76.3
ygfuhf,tj,89
```

With these requirements, Vincent works out a first prototype of this application and tests to see if it works as the client wanted. Below is the class code.

![TradeProcessor](assets/TradeProcessclass.PNG)

```python
from typing import List
from sqlalchemy import create_engine, Column, Integer, String, Float
from sqlalchemy.orm import sessionmaker
from base import Base

class TradeProcessor(object):
    @staticmethod
    def process_trades(filename):
        lines: List[str] = []
        with open(filename) as ft:
            for line in ft: lines.append(line)
        trades: List[TradeRecord] = []

        for index, line in enumerate(lines):
            fields = line.split(',')
            if len(fields) != 3:
                print(f'Line {index} malformed. Only {len(fields)} field(s) found.')
                continue
            if len(fields[0]) != 6:
                print(f'Trade currencies on line {index} malformed: "{fields[0]}"')
                continue
            trade_amount = 0
            try:
                trade_amount = float(fields[1])
            except ValueError:
                print(f"WARN: Trade amount on line {index} not a valid integer: '{fields[1]}'")

            trade_price = 0
            try:
                trade_price = float(fields[2])
            except ValueError:
                print(f"WARN: Trade price on line {index} not a valid decimal:'{fields[2]}'")

            print(trade_amount)
            sourceCurrencyCode = fields[0][:3]
            destinationCurrencyCode = fields[0][3:]
            trade = TradeRecord(source=sourceCurrencyCode, dest=destinationCurrencyCode,
                                lots=trade_amount, amount=trade_price)
            trades.append(trade)

        engine = create_engine('postgresql://postgres:u2402/598@localhost:5432/python')
        Session = sessionmaker(bind=engine)
        Base.metadata.create_all(engine)
        session = Session()
        for trade in trades:
            session.add(trade)
        session.commit()
        session.close()
```

> **Note :** In this example we used the SqlAlchemy ORM for persistence but we could have used any DB APIs out there.

Below is the code for the TradeRecord class that SqlAlchemy uses to persist our data.

```python
class TradeRecord(Base):
    __tablename__ = 'TradeRecord'
    id = Column(Integer, primary_key=True)
    source_curreny = Column(String)
    dest_currency = Column(String)
    lots = Column(Integer)
    amount = Column(Float)

    def __init__(self, source, dest, lots, amount):
        self.source_curreny = source
        self.dest_currency = dest
        self.lots = lots
        self.amount = amount
```

If you look closely at the TradeProcessor class, it is the best example of a class that has a ton of responsibilities to change. The method `process_trades` is a hidden class within itself. It is doing more than one thing as listed below.

1. It reads every line from a File object, storing each line in a list of strings.
2. It parses out individual fields from each line and stores them in a more structured list of
   Trade­Record instances.
3. The parsing includes some validation and some logging to the console.
4. Each TradeRecord is then stored to a database.

We can see that the responsibilities of the TradeProcessor are :

1. Reading files
2. Parsing strings
3. Validating string fields
4. Logging
5. Database insertion.

The single responsibility principle states that this class, like all others,
should only have a single reason to change. However, the reality of the TradeProcessor is that it will
change under the following circumstances:

- When the client decides not to use a file for input but instead read the trades from a remote call to a web service.
- When the format of the input data changes, perhaps with the addition of an extra field indicating the broker for the transaction.
- When the validation rules of the input data change.
- When the way in which you log warnings, errors, and information changes. If you are using a
  hosted web service, writing to the console would not be a viable option.
- When the database changes in some way for example the client decides not to store the data in a relational database and opt for document storage, or the database is moved behind a web service that
  you must call.

For each of these changes, this class would have to be modified. Furthermore, unless you maintain
a variety of versions, there is no possibility of adapting the TradeProcessor so that it is able to read
from a different input source, for example. Imagine the maintenance headache when you are asked to
add the ability to store the trades in a web service!!!

#### **Refactoring towards the SRP**

---

We are going to achieve this in two steps.

1. Refactor for Clarity
2. Refactor for Adaptability

#### **Refactor for clarity**

---

The first thing we are going to do is to break down the monstrous `process_trades()` method into smaller more specialized methods that do only one thing. Here we go.
If you look closely, the `process_trades()` method is doing 3 things:

1. Reading data from the file.
2. Parsing and Logging and
3. Storing to the data.

![process_trades_refactor](<assets/process_trades%20(2).png>)

So we can see from a very high level refactor it to something like below

```python
 @staticmethod
    def process_trades(filename):
        lines: List[str] = TradeProcessor.__read_trade_data(filename)
        trades: List[TradeRecord] = TradeProcessor.__parse_trades(lines)
        TradeProcessor.__store_trades(trades)
```

> Notice how these 4 smaller methods are easier to test than the original monolith!!

![TradeProcessor_class refactor](assets/methods_refactor.PNG)
Now let us look into the implementations of these new more focused methods.

#### read_trade_data()

---

```python
@staticmethod
    def __read_trade_data(filename: str) -> List[str]:
        lines: List[str]
        lines = [line for line in open(filename)]
        return lines
```

This method takes in the name of the file to read, it uses a list comprehension to enumerate over the read lines and returns a list of strings. Really simple!!!.

#### parse_trades

---

```python
@staticmethod
    def __parse_trades(trade_data: List[str]) -> List[TradeRecord]:
        trades: List[TradeRecord] = []
        for index, line in enumerate(trade_data):
            fields: List[str] = line.split(',')
            if not TradeProcessor.__validate_trade_data(fields, index + 1):
                continue
            trade = TradeProcessor.__create_trade_record(fields)
            trades.append(trade)
        return trades
```

This method takes in a list of strings produced by the `read_trade_data()` methods and tries to parse according to a given structure. **methods should do only one thing** and hence the `parse_trades()` method delegates to two other methods to accomplish its task.

1. The `validate_trade_data()` method. This is responsible for validating the read string to check if it follows a given format.
2. The `create_trade_record()` method. This takes in a list of validated strings and uses them to create a `TradeRecord` object to persist to the database.

Let us work on the implements of these two new methods.

#### validate_trade_data()

---

```python
   @staticmethod
    def __validate_trade_data(fields: List[str], index: int) -> bool:
        if len(fields) != 3:
            TradeProcessor.__log_message(f'WARN: Line {index} malformed. Only {len(fields)} field(s) found.')
            return False
        if len(fields[0]) != 6:
            TradeProcessor.__log_message(f'WARN: Trade currencies on line {index} malformed: {fields[0]}')
            return False
        try:
            trade_amount = float(fields[1])
        except ValueError:
            TradeProcessor.__log_message(f"WARN: Trade amount on line {index} not a valid integer: '{fields[1]}'")
            return False
        try:
            trade_price = float(fields[2])
        except ValueError:
            TradeProcessor.__log_message(f'WARN: Trade price on line {index} not a valid decimal:{fields[2]}')
            return False
        return True
```

This method should be self explanatory since it is a refactor from the original `process_trades()` method. One thing has changed in it. The method no longer does the logging by itself. It delegates the logging to another method called `log_message()`. We shall see the advantage of this later.

Below is the implementation of the `log_message()` method.

```python
@staticmethod
def __log_message(message: str) -> None:
    print(message)
```

#### create_trade_record()

---

```python
@staticmethod
    def __create_trade_record(fields: List[str]) -> TradeRecord:
        in_curr = slice(0, 3);
        out_curr = slice(3, None)
        source_curr_code = fields[0][in_curr]
        dest_curr_code = fields[0][out_curr]
        trade_amount = int(fields[1])
        trade_price = float(fields[2])

        trade_record = TradeRecord(source_curr_code, dest_curr_code,
                                   trade_amount, trade_price)
        return trade_record
```

This is also straight forward. The reason why we use slice objects here is to make our code readable. The last method we will look at is the `store_trades()` which persists our data to a database.

#### store_trades()

---

```python
@staticmethod
    def __store_trades(trades: List[TradeRecord]) -> None:
        engine = create_engine('postgresql://postgres:54875/501@localhost:5432/python')
        Session = sessionmaker(bind=engine)
        Base.metadata.create_all(engine)
        session = Session()
        for trade in trades:
            session.add(trade)
        session.commit()
        session.close()
        TradeProcessor.__log_message(f'{len(trades)} trades processed')

```

This method uses an ORM known as SQLAlchemy to persist our data. ORMs write the SQL for us behind the scene and this increases the flexibility of our application.

> This method is far from ideal, notice that it hard codes the connection strings and this very bad. There are tones of github repositories with exposed database connection strings. It would be better to read the connection string from a configuration file and add the configure file to gitignore.

At the moment, our class that had only one big method now has a bunch of methods as shown in the following code snippet and UML class diagram:

```python

class TradeProcessor(object):
    @staticmethod
    def process_trades(filename):
        lines: List[str] = TradeProcessor.read_trade_data(filename)
        trades: List[TradeRecord] = TradeProcessor.parse_trades(lines)
        TradeProcessor.store_trades(trades)

    @staticmethod
    def __read_trade_data(filename: str) -> List[str]:
        lines: List[str]
        lines = [line for line in open(filename)]
        return lines

    @staticmethod
    def __log_message(message: str) -> None:
        print(message)

    @staticmethod
    def __validate_trade_data(fields: List[str], index: int) -> bool:
        if len(fields) != 3:
            TradeProcessor.log_message(f'Line {index} malformed. Only {len(fields)} field(s) found.')
            return False
        if len(fields[0]) != 6:
            TradeProcessor.log_message(f'Trade currencies on line {index} malformed: {fields[0]}')
            return False
        try:
            trade_amount = float(fields[1])
        except ValueError:
            TradeProcessor.log_message(f"Trade amount on line {index} not a valid integer: '{fields[1]}'")
            return False
        try:
            trade_price = float(fields[2])
        except ValueError:
            TradeProcessor.log_message(f'Trade price on line {index} not a valid decimal:{fields[2]}')
            return False
        return True

    @staticmethod
    def __create_trade_record(fields: List[str]) -> TradeRecord:
        in_curr = slice(0, 3);
        out_curr = slice(3, None)
        source_curr_code = fields[0][in_curr]
        dest_curr_code = fields[0][out_curr]
        trade_amount = int(fields[1])
        trade_price = float(fields[2])

        trade_record = TradeRecord(source_curr_code, dest_curr_code,
                                   trade_amount, trade_price)
        return trade_record

    @staticmethod
    def __parse_trades(trade_data: List[str]) -> List[TradeRecord]:
        trades: List[TradeRecord] = []
        for index, line in enumerate(trade_data):
            fields: List[str] = line.split(',')
            if not TradeProcessor.validate_trade_data(fields, index + 1):
                continue
            trade = TradeProcessor.create_trade_record(fields)
            trades.append(trade)
        return trades

    @staticmethod
    def __store_trades(trades: List[TradeRecord]) -> None:
        engine = create_engine('postgresql://postgres:u2402/501@localhost:5432/python')
        Session = sessionmaker(bind=engine)
        Base.metadata.create_all(engine)
        session = Session()
        for trade in trades:
            session.add(trade)
        session.commit()
        session.close()
        TradeProcessor.log_message(f'{len(trades)} trades processed')


```

![Refactored TradeProcessor class](assets/Refactored_TradeProcessClass.PNG)

Looking back at this refactor, it is a clear improvement on the original implementation. However, what have you really achieved? Although the new ProcessTrades method is indisputably smaller
than the monolithic original, and the code is definitely more readable, you have gained very little by way of adaptability. You can change the implementation of the LogMessage method so that it, for example, writes to a file instead of to the console, but that involves a change to the TradeProcessor class, which is precisely what you wanted to avoid.

This refactor has been an important stepping stone on the path to truly separating the responsibilities of this class. It has been a **refactor for clarity**, not for adaptability. The next task is to split each responsibility into different classes and place them behind interfaces. What you need is true abstraction to achieve useful adaptability.

#### **Refactoring for adaptability**

---

In the previous refactor, we broke down the `process_trades()` method into smaller more focused methods. But still, that didn't solve our problem, our class was still doing lots of things. In this section, we are going to distribute the different responsibilities across classes.

From the previous section, we agreed that our class was serving 3 main responsibilities, Data reading, Data parsing and data storage. So we will start with taking out the code that does that into other classes.

We are going to create 3 abstract classes that will be used by the TradeProcessor class as shown in the following UML diagram.

![](assets/First_Refactor.PNG)

In the above UML diagram, the TradeProcessor class now has private polymorphic hidden fields that it uses to accomplish its tasks. Since we already created smaller specific methods, we know which method goes to which abstraction. Below are the implementations of the new abstract classes.

```python
class DataProvider(ABC):
    @abstractmethod
    def read_trade_data(self):
        pass


class TradeDataParser(ABC):
    @abstractmethod
    def parse_trade_data(self, lines: List[str]) -> List[TradeRecord]:
        pass


class TradeRepository(ABC):
    @abstractmethod
    def persist_trade_data(self, trade_data: List[TradeRecord]) -> None:
        pass
```

Notice that all of them are abstract classes with abstract methods and so can't be directly instantiated. We shall then have implementors of these abstract classes to use with the `TradeProcess()` class.

Below is the new implementation of the TradeProcessor class.

```python
class TradeProcessor(object):
    def __init__(self, provider: DataProvider, parser: TradeDataParser,
                 persister: TradeRepository) -> None:
        self._provider = provider
        self._parser = parser
        self._persister = persister

    def process_trades(self):
        lines = self._provider.read_trade_data()
        trades = self._parser.parse_trade_data(lines)
        self._persister.persist_trade_data(trades)
```

We are now doing it the object oriented way, we are having objects encapsulating computations (wait for the **strategy pattern** later). The objects that do the real work are injected into the TradeProcessor class when it is being instantiated. This is an example of **dependency inversion** which is implemented by the **dependency injection** pattern. More on this later.

The class is now significantly different from its previous incarnation. It no longer contains the
implementation details for the whole process but instead contains the blueprint for the process.
The class models the process of transferring trade data from one format to another. This is its only
responsibility, its only concern, and the only reason that this class should change. If the process itself
changes, this class will change to reflect it. But if you decide you no longer want to retrieve data from
a file, log on to the console, or store the trades in a database, this class remains as is.

> The more observant readers may be asking where the objects injected into the TradeProcessor class come from. Well, they come from a dependency injection container. One thing that the **Single Responsibility Principle** gives rise to are lots of small classes. To assemble such small classes to work well can be a hard thing to do, and that is when dependency injection containers come to the resucue.

Since the `TradeProcessor` class now just models the workflow of converting between trade data formats, it no longer cares about where the data comes from, how it is parsed, validated and where it is stored. This means we can have different implementations of the

`DataProvider` abstraction

- Relational Database
- Text Files
- NoSql Databases
- Web services
- e.t.c

`TradeDataParser` abstraction

- CommaParser
- TabParser
- ColonParser

`TradeRepository` abstraction

- Relational Database
- Text Files
- NoSql Databases
- Web services
- e.t.c

The UML below shows some of the classes implementing the above abstract classes. Notice that we can swap between any of the different implementations and `TradeProcessor` will not even know. This is what software engineers call **loose coupling**.

![Comma_parser](assets/CommaParser.PNG)

From the above diagram, we are confident that once a new storage mechanism pops up, we just roll up a class to implement the new functionality, we make sure that the class inherits from the right base class. We then inject this new class instance in `TradeProcessor`. This is the **Open Closed Principle** as we will see in the next section.

If you look so closely at the above diagram, you can notice that as new requirements pop up, we get a class **big bang**. We shall solve this problem later when we look at **decorators**.

So far, we have solved 3 problems. These are:

- What happens if we need to use another data source.
- What happens if we need to store the data to a different storage.
- What happens when the business requirements call for a new parsing strategy.

**What happens if new business rules come up that need new validation rules?**

Remember that the original
`parse_trades()` method delegated responsibility for validation and for mapping. You can repeat the
process of refactoring so that the `CommaParser` class does not have more than one responsibility. At the moment, `CommaParser` is implemented as shown below

```python

class CommaParser(TradeDataParser):
    def parse_trade_data(self, trade_data : List[str]) -> List[TradeRecord]:
        trades: List[TradeRecord] = []
        for index, line in enumerate(trade_data):
            fields: List[str] = line.split(',')
            if not CommaParser.__validate_trade_data(fields, index + 1):
                continue
            trade = CommaParser.__create_trade_record(fields)
            trades.append(trade)
        return trades

    @staticmethod
    def __log_message(message: str) -> None:
        print(message)

    def __create_trade_record(self,fields: List[str]) -> TradeRecord:
        in_curr = slice(0, 3);
        out_curr = slice(3, None)
        source_curr_code = fields[0][in_curr]
        dest_curr_code = fields[0][out_curr]
        trade_amount = int(fields[1])
        trade_price = float(fields[2])

        trade_record = TradeRecord(source_curr_code, dest_curr_code,
                                   trade_amount, trade_price)
        return trade_record

    def __validate_trade_data(self, fields: List[str], index: int) -> bool:
        if len(fields) != 3:
            CommaParser.__log_message(f'Line {index} malformed. Only {len(fields)} field(s) found.')
            return False
        if len(fields[0]) != 6:
            CommaParser.__log_message(f'Trade currencies on line {index} malformed: {fields[0]}')
            return False
        try:
            trade_amount = float(fields[1])
        except ValueError:
            CommaParser.__log_message(f"Trade amount on line {index} not a valid integer: '{fields[1]}'")
            return False
        try:
            trade_price = float(fields[2])
        except ValueError:
            CommaParser.__log_message(f'Trade price on line {index} not a valid float:{fields[2]}')
            return False
        return True
```

We can see that the current implementation of `CommaParser` is not ideal. The class is having more than one responsibility to change. So we can refactor out the two methods `__validate_trade_data()` and `__create_trade_record()` into new classes since they both change for different reasons.

We will create 2 new abstractions -- `TradeMapper` (responsible for mapping validated fields into `TradeRecord` instances) and `TradeValidator` (responsible for validating the input data before creating `TradeRecord` instances).

Our new design is shown in the following UML diagram.

![ParserHierachy](assets/ParserHierachy.PNG)

This is a flexible design in such a way that if the parsing rules change, i.e. text is separated by tab and not ',', we just implement `TradeDataParser` in a new class.Incase the data validation rules change too, we just roll up a new class inheriting from `TradeValidator`.

Below are the implementations of the new abstractions. Note that the interface for `TradeDataParser` has changed and now takes in instances of `TradeValidator` and `TradeMapper` to help it accomplish it's task

```python

class TradeMapper(ABC):
    @abstractmethod
    def create_trade_record(self, fields: List[str]) -> TradeRecord:
        pass


class TradeValidator(ABC):
    @abstractmethod
    def validate_trade_data(self, fields: List[str], index: int) -> bool:
        pass


class TradeDataParser(ABC):
    @abstractmethod
    def parse_trade_data(self, trade_data: List[str]) -> TradeRecord:
        pass
```

And then here is the new implementation of the CommaParser in terms of these new abstractions.

![Delegation](assets/delegation.png)

Pay attention to the green rectangles. In the constructor, two dependencies are injected in `mapper` and `validator` and these two are used by CommaParser to parse the input assuming the string components are separated by commas hence the `split(',')`. Other parsers would implement it differently.

Below are possible implementations of the `TradeMapper` and `TradeValidator` abstractions.

```python
class SimpleTradeMapper(TradeMapper):
    def create_trade_record(self, fields: List[str]) -> TradeRecord:
        in_curr = slice(0, 3);
        out_curr = slice(3, None)
        source_curr_code = fields[0][in_curr]
        dest_curr_code = fields[0][out_curr]
        trade_amount = int(fields[1])
        trade_price = float(fields[2])

        trade_record = TradeRecord(source_curr_code, dest_curr_code,
                                   trade_amount, trade_price)
        return trade_record
```

```python

class SimpleValidator(TradeValidator):
    @staticmethod
    def __log_message(message: str) -> None:
        print(message)

    def validate_trade_data(self, fields: List[str], index: int) -> bool:
        if len(fields) != 3:
            SimpleValidator.__log_message(f'Line {index} malformed. Only {len(fields)} field(s) found.')
            return False
        if len(fields[0]) != 6:
            SimpleValidator.__log_message(f'Trade currencies on line {index} malformed: {fields[0]}')
            return False
        try:
            trade_amount = float(fields[1])
        except ValueError:
            SimpleValidator.__log_message(f"Trade amount on line {index} not a valid integer: '{fields[1]}'")
            return False
        try:
            trade_price = float(fields[2])
        except ValueError:
            SimpleValidator.__log_message(f'Trade price on line {index} not a valid float:{fields[2]}')
            return False
        return True

```

We are almost there but still we are having a smell in our design. We would love to be able to log to different destinations -- console, text file or even a database. But if you look closely at the implementations of `TradeRepository` and `TradeValidator`, the logger is hard coded and it always logs to the console.

We have to solve this problem before we run out of business. We are going to refactor this function into its abstraction. The following snippet reveals the snippet for this change.

```python
from abc import ABC, abstractmethod

class TradeLogger(ABC):
    @abstractmethod
    def log_message(self, message):
        pass


class SimpleValidator(TradeValidator):
    def __init__(self, logger: TradeLogger)->None:
        if instance(logger, TradeLogger):
            self._logger = logger
        else:
            raise AssertionError('Bad Argument')

    def validate_trade_data(self, fields: List[str], index: int) -> bool:
        if len(fields) != 3:
            self._logger.log_message(f'Line {index} malformed. Only {len(fields)} field(s) found.')
            return False
        if len(fields[0]) != 6:
            self._logger.log_message(f'Trade currencies on line {index} malformed: {fields[0]}')
            return False
        try:
            trade_amount = float(fields[1])
        except ValueError:
            self._logger.log_message(f"Trade amount on line {index} not a valid integer: '{fields[1]}'")
            return False
        try:
            trade_price = float(fields[2])
        except ValueError:
            self._logger.log_message(f'Trade price on line {index} not a valid float:{fields[2]}')
            return False
        return True
```

After all these refactorings, we finally have a collection of abstractions that work together to solve the simple problem we posed at the beginning of this chapter.

The figure below shows the design of the abstractions.

![framework](assets/framework.PNG)

> **Note** that none of these are concrete classes and so they can not be instantiated. To use the `TradeProcessor` class, you will need concrete implementations of all these abstractions and then you will have to wire them together to accomplish a task. **Dependency Injection** containers do this wiring.

From a monolith, we have created a miniature framework for converting trade data between formats. Congratulations!!!!!.

### Open/Closed Principle(OCP)

---

We will now go to the next principle on my list of the SOLID principles of Object Oriented software design--**The Open/Closed Principle**. This principle states that **A software artifact should be closed for modification but open for extension**.

At first, this definition seems to be a paradox. How can a software module be closed for modification but open for extension?? Well, we shall see how achieve this goal with the principles we shall discuss in this section.

This term was first coined in 1988 by **Bertran Meyer** in his book **Object-Oriented Software Construction (Prentice Hall)**. The modern definition of this principle was offered by Martin Roberts and goes as follows

> **Open for extension** : This means that the behavior of the module can be extended.
> As the requirements of the application change, we are able to extend the module
> with new behaviors that satisfy those changes. In other words, we are able to
> change what the module does. <br/> <br/> > **Closed for modification** : Extending the behavior of a module does not result
> in changes to the source or binary code of the module. The binary executable
> version of the module, whether in a linkable library, a DLL, or a Java .jar, remains untouched.

There are 2 exceptions to this rule. Code can be edited if :

1. Fixing bugs.

If a module contains a bug, we can either choose to write a new similar module without the bugs but this would be an overkill solution. So we tend to prefer fixing the buggy module to writing a new one.

2. Client awareness.

Another situation where it is possible to edit the source code of a module is when the changes don't affect the client of the module.This places an
emphasis on how coupled the software modules are, at all levels of granularity: between classes and
classes, assemblies and assemblies, and subsystems and subsystems.

If a change in one class forces a change in another, the two are said to be tightly coupled. Conversely, if a class can change in isolation without forcing other classes to change, the participating
classes are loosely coupled. At all times and at all levels, loose coupling is preferable. Maintaining
loose coupling limits the impact that the OCP has if you allow modifications to existing code that
does not force further changes to clients

To illustrate the OCP rule, we are going to use the following techniques

1. Strategy pattern
1. Decorator design pattern

#### Strategy design pattern.

---

> **Strategy Pattern** <br/> Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from the clients that use it.

This definition seems abstract enough but we are going to try explaininig it in the following example.
Consider the following class that is part of an e-commerce application. The class contains a method that selects the which payment to choose for settling a payment as shown below.

**Bad** :angry:

```python
class OnlineCart:
    def check_out(self, payment_type: str) -> None:
        if payment_type == 'creditCard':
            self.process_credit_card_payment()
        elif payment_type == 'payPal':
            self.process_paypal_payment()
        elif payment_type == 'GoogleCheckout':
            self.process_google_payment()
        elif payment_type == 'AmazonPayments':
            self.process_amazon_payment()
        else:
            pass

    def process_credit_card_payment(self):
        print('paying with credit card...')

    def process_paypal_payment(self):
        print('Paying with paypal...')

    def process_google_payment(self):
        print('Paying with google check out')

    def process_amazon_payment(self):
        print('Paying with amazon ...')
```

The above class is neither extendable nor flexible. If a new payment method comes up, the conditional logic will have to be changed and a new method added to the class. This class violets the OCP rule and thus needs to be refactored.

There are many ways to solve this simple problem but I will stick with the original solution proposed by the GoF programmers. We will use the **strategy pattern**. We will model each payment method as a class and we will use composition and inject in the payment strategy at run-time.

**Good** :smile:

```python
from abc import ABC, abstractmethod

class Payment(ABC):
    def __init__(self, payment_id: str):
        self.id = payment_id

    @abstractmethod
    def pay(self):
        pass

```

This is the interface that all payment strategies are supposed to implement. We are going to use to create a family of payment strategies.

**Good** :smile:

```python
class OnlineCart:
    def __init__(self, payment: Payment) -> None:
        if isinstance(payment, Payment):
            self.payment = payment
        else:
            raise AssertionError('Bad argument')

    def check_out(self):
        self.payment.pay()


```

The `OnLineCart` class no longer contains the conditional logic and all the corresponding methods have been pulled out. They will be implemented by the corresponding payment strategies as the following code snippet reveals.

**Good** :smile:

```python

class CreditCard(Payment):
    def __init__(self, *, card_number: str) -> None:
        Payment.__init__(self, card_number)

    def pay(self) -> None:
        print(f'Payment made with card number {self.id}')


class Paypal(Payment):
    def __init__(self, *, paypal_id: str) -> None:
        Payment.__init__(self, paypal_id)

    def pay(self) -> None:
        print(f'Payment made with paypal id {self.id}')


class GoogleCheckOut(Payment):
    def __init__(self, *, google_checkout: str) -> None:
        Payment.__init__(self, google_checkout)

    def pay(self) -> None:
        print(f'Payment made with google checkout with id {self.id}')


class AmazonPayment(Payment):
    def __init__(self, *, amazon_payment: str) -> None:
        Payment.__init__(self, amazon_payment)

    def pay(self) -> None:
        print(f'Payment made with amazon services using id {self.id}')


```

To use the `OnlineCart` class, we inject in the payment strategy to use for making the payment as shown in the following snippet.

```python
# we are paying using paypal
paypal : Payment = PayPal(paypal_id='ERTWF342T')
cart : OnlineCart = OnlineCart(paypal)
cart.check_out()
```

Note that the `OnlineCart` class no longer cares about which payment method is being used, it delegates that responsibility to the wrapped object. `OnlineCart` is now open for extension (we can change its behavior by passing in different objects) but it is closed for modification (we don't change its source code to add new functionality).

![Strategy_Pattern](assets/strategypattern.PNG)

From the above UML class diagram, we can notice that once a new payment method shows up, we just create a new class for that method, inherit from `Payment` and inject it in `OnlineCart`. This code is flexible and extendable.

> **Note**: The same design could be achieved with lambda expressions though at times the logic in the respective strategiess may be complex enough that it is implemented in more than one function. This is why i decided to use this rather verbose method.

#### The Decorator design pattern

---

> **Decorator Pattern**<br/>Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to sub-classing for extending functionality.

The decorator design pattern was first proposed in 1994 in the seminal work of the Gang of Four book. It is a technique of adding capabilities to a class without changing its source code.
We are going to view this under various examples.

We are going to continue with our example in the previous section. Consider that we want to add some logging information after making the payment. There are many ways to solve this problem and one of them is to edit the `OnlineCart` class to add logging features as shown below;

![code_smell](assets/modification.png)

This is a **code smell**. Notice that we have modified the class, this is violation of the Open/Closed principle. There is even a more serious problem than this one. In this case we are logging to the console, what will happen if we want to log to a database or to a text file? We will have to constantly open this class and modify it, this is serious violation of the OCP rule.

One solution to this problem is the **decorator pattern**. Decorators are just classes that wrapper other classes. The **wrapped classes** have exactly the same interface as the **wrapper classes**. We achieve this by using both **composition** and **inheritance** as the following UML diagram reveals.

![decorator_pattern](assets/decoraror_pattern.PNG)

In this case, the `Component` is an interface that is both supported by `ConcreteComponent` and `Decorator` this means that both `ConcreteComponent` and `Decorator` can be swapped without breaking existing code.

Notice also that the `Decorator` contains a `Component` inside it implying that it delegates some of its tasks to the wrapped component.

Let us use this technique to add logging capabilities to the `OnlineCart` class without having to modify it.

Looking very closely at the code for `OnlineCart`, notice that the `Payment` object is being injected during the instantiation of the class and so the `OnlineCart` class doesn't control what type of payment it receives (remember `Payment` is polymorphic).

![DI](assets/OnlineCart.png)

This means we can inject in anything that is similar to `Payment`. That is what the **Decorator pattern** is based on. **Dependency Injection** is the prerequisite to achieving all this flexibility. We shall cover dependency injection fully under the **Dependency Inversion Principle (DIP)**.

The following diagram shows the idea behind the decorator pattern.
![Interception](assets/interception.PNG)

In the first row, the `OnlineCart` class is directly depending on the `Payment` abstraction. In the second row, the `OnlineCart` class no longer depends directly on the `Payment` abstraction, there has been some redirection.

Ok!! time for some code.

Below is the code for our abstract decorator class.

```python
class Decorator(Payment, ABC):
    def __init__(self, payment: Payment) -> None:
        if isinstance(payment, Payment):
            self.payment = payment
        else:
            raise AssertionError('Bad argument')

    @abstractmethod
    def pay(self):
        pass
```

Pay close attention to this class.

1. It uses multiple inheritance : This is because we need the class to both be abstract and still inherit from the `Payment` class.
2. It takes in a `Payment` dependency and inherits from `Payment`. This is typical of decorator classes.
3. The `pay()` method is abstract since concrete decorators will have to define their implementations.

We can now implement our `Logging` decorator that adds logging capabilities to the `OnlineCart` class without modifying it. Let's go!!!

```python
class ConsoleLoggingDecorator(Decorator):
    def __init__(self, payment: Payment):
        Decorator.__init__(self, payment)

    def pay(self):
        self.payment.pay()
        print(f'Logging Payment made with id {self.payment.id}')
```

Simple!!! This is the console logging decorator because it logs on the console using `print`. We can create a file logging decorator that logs into a text file as shown below.

```python
class FileLoggingDecorator(Decorator):
    def __init__(self, *, payment: Payment, filename: str):
        Decorator.__init__(self, payment)
        self.filename = filename

    def pay(self):
        self.payment.pay()
        _file = open(self.filename, 'a')
        _file.writelines(f'{self.payment.id} payment logged\n')
        _file.close()
```

The snippet shows the code that sets up the `OnlineCart` class to use the `ConsoleLoggingDecorator` and then the `FileLoggingDecorator`

```python
#using the console logger
credit_card: Payment = CreditCard(card_number='RTGW@#')
decorator: FileLoggingDecorator = ConsoleLoggingDecorator(payment=credit_card)
cart: OnlineCart = OnlineCart(decorator)
cart.check_out()

#using the file logger
credit_card: Payment = CreditCard(card_number='RTGW@#')
decorator: FileLoggingDecorator = FileLoggingDecorator(filename='dta.txt', payment=paypal)
cart: OnlineCart = OnlineCart(decorator)
cart.check_out()
```

**Note**: We first create a bare payment object and then wrap it in a decorator which we then inject into `OnlineCart` class. The decorator adds some capabilities (in this case logging) to the payment object.

We can add any functionality to the `OnlineCart` class without modifying it. Capabilities like Profiling, Laziness, Immutability, e.t.c.

The following UML class diagram shows our work up to to this point in time.

![Decorator_refactor](assets/Decorator_refactor.PNG)

We can add more organization to the decorator hierachy by using the **Template pattern**. That will be a story for the next time.

### Liskov Substitution Principle

---

> **Subtypes must be substitutable for their base types.**

Barbara Liskov's formulation is more precise: if `S` is a subtype of `T`, then objects of type `T` may be
replaced with objects of type `S` without altering any of the desirable properties of the program.

In practice this means inheritance is a promise about **behaviour**, not just about shared code. A caller
that holds a reference to the base class must be able to use any subclass without knowing which one it has,
and without being surprised.

#### The classic violation: Rectangle and Square

Mathematically, a square *is a* rectangle. In code, it is not.

**Bad** :angry:

```python
class Rectangle:
    def __init__(self, width: float, height: float) -> None:
        self.width = width
        self.height = height

    def set_width(self, width: float) -> None:
        self.width = width

    def set_height(self, height: float) -> None:
        self.height = height

    def area(self) -> float:
        return self.width * self.height


class Square(Rectangle):
    def set_width(self, width: float) -> None:
        self.width = width
        self.height = width          # a square must stay square

    def set_height(self, height: float) -> None:
        self.width = height
        self.height = height
```

Now write a function against the base class:

```python
def stretch(rectangle: Rectangle) -> None:
    rectangle.set_width(5)
    rectangle.set_height(4)
    assert rectangle.area() == 20     # obviously true for a Rectangle
```

`stretch(Square(2, 2))` fails: the area is 16. The function is correct, the subclass is "correct" in
isolation, and yet together they are broken. `Square` is not substitutable for `Rectangle` because it
strengthened an invariant that callers of `Rectangle` were entitled to rely on.

**Good** :smiley: — model the shared abstraction, not the resemblance

```python
from abc import ABC, abstractmethod
from dataclasses import dataclass


class Shape(ABC):
    @abstractmethod
    def area(self) -> float: ...


@dataclass(frozen=True)
class Rectangle(Shape):
    width: float
    height: float

    def area(self) -> float:
        return self.width * self.height


@dataclass(frozen=True)
class Square(Shape):
    side: float

    def area(self) -> float:
        return self.side ** 2
```

Notice that immutability removed the problem entirely: without setters there is no invariant for a subclass
to break. *"Prefer immutability"* is often the cheapest way to satisfy the LSP.

#### A more realistic violation

**Bad** :angry:

```python
class Account:
    def withdraw(self, amount: float) -> None:
        self.balance -= amount


class FixedDepositAccount(Account):
    def withdraw(self, amount: float) -> None:
        raise NotImplementedError('You cannot withdraw from a fixed deposit')
```

Every function that takes an `Account` and calls `withdraw` now has to know which subclass it received —
which is precisely what the base class was supposed to spare it from. Look for these smells:

- A subclass that **raises `NotImplementedError`** for an inherited method.
- A subclass whose override is **empty** ("this one does nothing").
- Callers that use `isinstance` or `type()` to decide what to do.

**Good** :smiley: — split the hierarchy along what things can actually do

```python
class Account(ABC):
    @abstractmethod
    def deposit(self, amount: Decimal) -> None: ...

    @abstractmethod
    def balance(self) -> Decimal: ...


class WithdrawableAccount(Account, ABC):
    @abstractmethod
    def withdraw(self, amount: Decimal) -> None: ...


class SavingsAccount(WithdrawableAccount): ...
class CurrentAccount(WithdrawableAccount): ...
class FixedDepositAccount(Account): ...        # simply has no withdraw()
```

Now a function that needs to withdraw asks for a `WithdrawableAccount`, and the type checker rejects a fixed
deposit at compile time instead of at 3 a.m. in production.

#### The contract rules

A subclass may not:

- **Strengthen preconditions.** If the base accepts any positive amount, the subclass may not demand amounts
  under 1000.
- **Weaken postconditions.** If the base guarantees the balance is updated, the subclass must update it.
- **Break invariants** of the base class (the `Square` case).
- **Throw new exception types** that callers of the base were not told about.

It **may** weaken preconditions (accept more) and strengthen postconditions (guarantee more).

> **Rule of thumb:** if you have to read the documentation of the subclass in order to use the base class
> safely, the LSP is broken. And if a subclass only wants *some* of the parent's behaviour, prefer
> composition over inheritance — Python's duck typing and `Protocol` classes let you share an interface
> without sharing an implementation.

**[⬆ back to top](#table-of-contents)**

### Interface Segregation Principle

---

> **No client should be forced to depend on methods it does not use.**

Fat interfaces couple everybody to everything. When one class declares twenty methods and each caller uses
two of them, a change to any of the twenty forces every caller to be re-checked, re-compiled and re-tested.

#### The violation

**Bad** :angry:

```python
from abc import ABC, abstractmethod


class Worker(ABC):
    @abstractmethod
    def work(self) -> None: ...

    @abstractmethod
    def eat(self) -> None: ...

    @abstractmethod
    def sleep(self) -> None: ...


class HumanWorker(Worker):
    def work(self) -> None: ...
    def eat(self) -> None: ...
    def sleep(self) -> None: ...


class RobotWorker(Worker):
    def work(self) -> None: ...

    def eat(self) -> None:
        raise NotImplementedError('Robots do not eat')     # forced to implement

    def sleep(self) -> None:
        raise NotImplementedError('Robots do not sleep')
```

`RobotWorker` is dragged into a contract that has nothing to do with it, and the `NotImplementedError`s are
also an [LSP](#liskov-substitution-principle) violation — the two principles usually break together.

**Good** :smiley: — many small, role-based interfaces

```python
from typing import Protocol


class Workable(Protocol):
    def work(self) -> None: ...


class Feedable(Protocol):
    def eat(self) -> None: ...


class Sleepable(Protocol):
    def sleep(self) -> None: ...


class HumanWorker:
    def work(self) -> None: ...
    def eat(self) -> None: ...
    def sleep(self) -> None: ...


class RobotWorker:
    def work(self) -> None: ...


def run_shift(workers: list[Workable]) -> None:      # asks for exactly what it needs
    for worker in workers:
        worker.work()


def lunch_break(staff: list[Feedable]) -> None:
    for member in staff:
        member.eat()
```

`run_shift` accepts both humans and robots; `lunch_break` accepts only humans, and the type checker enforces
it. Neither class had to inherit from anything.

#### A practical example: printers

**Bad** :angry:

```python
class MultiFunctionDevice(ABC):
    @abstractmethod
    def print_document(self, doc: Document) -> None: ...

    @abstractmethod
    def scan(self) -> Document: ...

    @abstractmethod
    def fax(self, doc: Document, number: str) -> None: ...


class CheapPrinter(MultiFunctionDevice):
    def print_document(self, doc: Document) -> None: ...
    def scan(self) -> Document:
        raise NotImplementedError
    def fax(self, doc: Document, number: str) -> None:
        raise NotImplementedError
```

**Good** :smiley:

```python
class Printer(Protocol):
    def print_document(self, doc: Document) -> None: ...


class Scanner(Protocol):
    def scan(self) -> Document: ...


class Fax(Protocol):
    def fax(self, doc: Document, number: str) -> None: ...


class CheapPrinter:
    def print_document(self, doc: Document) -> None: ...


class OfficeAllInOne:
    """Satisfies Printer, Scanner and Fax without inheriting from any of them."""
    def print_document(self, doc: Document) -> None: ...
    def scan(self) -> Document: ...
    def fax(self, doc: Document, number: str) -> None: ...
```

#### ISP in everyday Python

You do not need `ABC` or `Protocol` to apply this principle — the idea is broader than interfaces:

- **Function parameters.** Take the narrowest type that works: `Iterable[str]` rather than `list[str]` if
  you only iterate; `Mapping` rather than `dict` if you only read.

  ```python
  def total(prices: Iterable[Decimal]) -> Decimal:      # accepts lists, tuples, generators, sets
      return sum(prices, start=Decimal('0'))
  ```

- **Configuration objects.** Do not pass the whole `Settings` object into a class that needs one timeout.
  Pass the timeout.

- **Test doubles as a signal.** If faking a dependency in a test requires stubbing eight methods you never
  call, the interface is too fat. Test pain is design feedback.

> **Rule of thumb:** the size of an interface should be decided by its *clients*, not by its implementation.
> Many small interfaces are easier to satisfy, easier to fake and easier to keep stable than one large one.

**[⬆ back to top](#table-of-contents)**

### Dependency Inversion Principle

---

> **A. High-level modules should not depend on low-level modules. Both should depend on abstractions.**
> **B. Abstractions should not depend on details. Details should depend on abstractions.**

The word "inversion" refers to the direction of the source-code dependency. In a naive design, business
logic imports the database driver, so the arrow points from policy down to detail. Inverted, the business
logic defines the interface it needs and the database module implements it — the arrow now points *up*,
towards the policy.

#### The violation

**Bad** :angry:

```python
import sqlite3
import smtplib


class OrderService:                                    # high-level policy
    def place_order(self, order: Order) -> None:
        connection = sqlite3.connect('shop.db')        # low-level detail
        connection.execute(
            'INSERT INTO orders VALUES (?, ?)', (order.id, str(order.total))
        )
        connection.commit()

        smtplib.SMTP('localhost').sendmail(            # another low-level detail
            'shop@example.com', order.customer_email, 'Thanks for your order'
        )
```

The business rule ("placing an order stores it and confirms it") is welded to SQLite and SMTP. You cannot
test it without both, you cannot move to Postgres without editing it, and the interesting logic is buried in
plumbing.

**Good** :smiley:

```python
from typing import Protocol


class OrderRepository(Protocol):            # abstraction owned by the policy
    def add(self, order: Order) -> None: ...


class Notifier(Protocol):
    def order_confirmed(self, order: Order) -> None: ...


class OrderService:                         # depends only on the abstractions
    def __init__(self, orders: OrderRepository, notifier: Notifier) -> None:
        self._orders = orders
        self._notifier = notifier

    def place_order(self, order: Order) -> None:
        self._orders.add(order)
        self._notifier.order_confirmed(order)
```

```python
class SqliteOrderRepository:                # detail, implements the abstraction
    def __init__(self, path: str) -> None:
        self._path = path

    def add(self, order: Order) -> None:
        with sqlite3.connect(self._path) as connection:
            connection.execute(
                'INSERT INTO orders VALUES (?, ?)', (order.id, str(order.total))
            )


class EmailNotifier:
    def __init__(self, smtp_host: str) -> None:
        self._smtp_host = smtp_host

    def order_confirmed(self, order: Order) -> None:
        smtplib.SMTP(self._smtp_host).sendmail(
            'shop@example.com', order.customer_email, 'Thanks for your order'
        )
```

The wiring happens once, at the edge of the application:

```python
def main() -> None:
    service = OrderService(
        orders=SqliteOrderRepository('shop.db'),
        notifier=EmailNotifier('localhost'),
    )
    service.place_order(build_order())
```

This is **dependency injection** — the mechanism — in service of **dependency inversion** — the design goal.

#### What it buys you

Testing becomes trivial, and the test reads like a specification:

```python
class InMemoryOrderRepository:
    def __init__(self) -> None:
        self.orders: list[Order] = []

    def add(self, order: Order) -> None:
        self.orders.append(order)


class RecordingNotifier:
    def __init__(self) -> None:
        self.confirmed: list[Order] = []

    def order_confirmed(self, order: Order) -> None:
        self.confirmed.append(order)


def test_placing_an_order_stores_and_confirms_it() -> None:
    orders, notifier = InMemoryOrderRepository(), RecordingNotifier()
    service = OrderService(orders, notifier)

    order = Order(id='1', total=Decimal('100'), customer_email='a@b.com')
    service.place_order(order)

    assert orders.orders == [order]
    assert notifier.confirmed == [order]
```

No database, no mail server, no mocking library, no sleeping for network timeouts — and the test breaks only
when the *behaviour* changes.

#### Doing it the Pythonic way

You rarely need a dependency injection framework. Python gives you lighter options:

```python
# 1. Constructor injection (the default choice)
class Report:
    def __init__(self, clock: Callable[[], datetime] = datetime.now) -> None:
        self._clock = clock


# 2. Pass a function, not an object with one method
def retry(action: Callable[[], T], attempts: int = 3) -> T: ...


# 3. Inject at the call site with a default
def render(template: str, *, loader: Loader = FileLoader()) -> str: ...
```

Two warnings:

- **Do not invert everything.** Abstracting a dependency that will never change (the `math` module, a
  dataclass you own) adds indirection and buys nothing. Invert at the boundaries: I/O, time, randomness,
  third-party services, anything slow or non-deterministic.
- **The abstraction belongs to the caller.** `OrderRepository` lives with `OrderService`, not with the
  SQLite code. If you put it in the database module, the dependency arrow never actually inverted.

> **Rule of thumb:** the parts of your system that encode business rules should be the parts that are
> hardest to break and easiest to test. If they import a driver, they are neither.

**[⬆ back to top](#table-of-contents)**

## **Testing**

---

Tests are not a chore you perform after the real work. They are the thing that makes the real work
*changeable*: without them, every refactoring in this guide is a gamble.

> **Test code is just as important as production code.** It is not a second-class citizen. It requires
> thought, design and care. It must be kept as clean as production code.

### The three laws of TDD

1. You may not write production code until you have written a failing test.
2. You may not write more of a test than is sufficient to fail (and not compiling counts as failing).
3. You may not write more production code than is sufficient to pass the currently failing test.

Working this way keeps tests and code in lockstep and guarantees that every line you ship is covered by
something that would have caught its absence.

### One assert (one concept) per test

A test that verifies five things fails on the first one and tells you nothing about the other four.

**Bad** :angry:

```python
def test_account():
    account = Account(100)
    account.deposit(50)
    assert account.balance == 150
    account.withdraw(30)
    assert account.balance == 120
    with pytest.raises(InsufficientFunds):
        account.withdraw(1000)
    assert account.is_active
```

**Good** :smiley:

```python
def test_deposit_increases_balance():
    account = Account(100)
    account.deposit(50)
    assert account.balance == 150


def test_withdrawal_decreases_balance():
    account = Account(100)
    account.withdraw(30)
    assert account.balance == 70


def test_withdrawing_more_than_the_balance_is_rejected():
    account = Account(100)
    with pytest.raises(InsufficientFunds):
        account.withdraw(1000)
```

Each name is a sentence about the system, and the suite doubles as documentation. Notice that the test names
are long — that is correct. A test name has one job: telling you what broke, from the failure output alone.

### Arrange, Act, Assert

Give every test the same three-part shape, and the reader never has to work out which part is which.

```python
def test_shipping_is_free_above_the_threshold():
    # Arrange
    cart = Cart(items=[Item('book', Decimal('60'))])

    # Act
    cost = shipping_cost(cart)

    # Assert
    assert cost == Decimal('0')
```

The `pytest` equivalent of a shared "Arrange" is a fixture:

```python
@pytest.fixture
def account() -> Account:
    return Account(balance=Decimal('100'))


def test_deposit_increases_balance(account: Account) -> None:
    account.deposit(Decimal('50'))
    assert account.balance == Decimal('150')
```

### F.I.R.S.T.

Clean tests follow five rules:

- **Fast.** Slow tests do not get run, and tests that do not get run do not prevent bugs.
- **Independent.** No test may depend on another running first, or on the order of the suite.
- **Repeatable.** The same result on your laptop, in CI and on a plane with no network. That means no
  reliance on the real clock, real randomness or a shared database — inject those (see
  [Dependency Inversion](#dependency-inversion-principle)).
- **Self-validating.** A test passes or fails. It does not print output for a human to inspect.
- **Timely.** Written just before the production code, while the design is still soft.

**Bad** :angry: — not repeatable

```python
def test_invoice_is_due_in_30_days():
    invoice = Invoice(issued_at=datetime.now())
    assert invoice.due_date == datetime.now() + timedelta(days=30)   # flaky
```

**Good** :smiley:

```python
def test_invoice_is_due_in_30_days():
    issued = datetime(2024, 1, 1)
    invoice = Invoice(issued_at=issued)
    assert invoice.due_date == datetime(2024, 1, 31)
```

### Test behaviour, not implementation

A test that reaches into private attributes or asserts on the exact sequence of internal calls will break
every time you refactor — which is exactly when you need it to keep working.

**Bad** :angry:

```python
def test_register_hashes_password(mocker):
    hasher = mocker.patch('app.hashlib.sha256')       # coupled to the algorithm
    register('a@b.com', 'secret123', users, mailer)
    hasher.assert_called_once()
```

**Good** :smiley:

```python
def test_registered_user_can_log_in_with_their_password():
    users = InMemoryUserRepository()
    register('a@b.com', 'secret123', users, RecordingMailer())
    assert authenticate('a@b.com', 'secret123', users) is not None


def test_the_raw_password_is_never_stored():
    users = InMemoryUserRepository()
    register('a@b.com', 'secret123', users, RecordingMailer())
    assert 'secret123' not in users.get('a@b.com').password_hash
```

Both tests would survive a switch from SHA-256 to bcrypt; the first one would not.

### Prefer fakes to mocks

A hand-written fake is usually clearer than a stack of `patch` calls, and it fails loudly when the real
interface changes:

```python
class InMemoryUserRepository:
    def __init__(self) -> None:
        self._users: dict[str, User] = {}

    def add(self, user: User) -> None:
        self._users[user.email] = user

    def get(self, email: str) -> User | None:
        return self._users.get(email)
```

Heavy mocking is often a design smell rather than a testing technique: if a unit cannot be tested without
patching five modules, it depends on five things it should not know about.

### Parametrise instead of copy-pasting

```python
@pytest.mark.parametrize(
    'amount, expected',
    [
        (Decimal('0'), Decimal('0')),
        (Decimal('100'), Decimal('16')),
        (Decimal('1000'), Decimal('160')),
    ],
)
def test_value_added_tax(amount: Decimal, expected: Decimal) -> None:
    assert value_added_tax(amount) == expected
```

> **Coverage is a floor, not a goal.** 100% coverage of trivial assertions proves nothing; a smaller suite
> that pins down real behaviour at the boundaries is worth far more.

**[⬆ back to top](#table-of-contents)**

## **Concurrency**

---

Concurrency decouples *what* gets done from *when* it gets done, and that decoupling can improve both
throughput and structure. It also introduces a whole class of bugs that appear once a month, in production,
and never in your test suite.

### Know which problem you have

Python offers three tools, and picking the wrong one is the most common mistake:

| Workload | Tool | Why |
| --- | --- | --- |
| **I/O bound** (network, disk, database) | `asyncio` or `threading` | The task is waiting, not computing; the GIL is released during I/O |
| **CPU bound** (parsing, maths, image work) | `multiprocessing` or `concurrent.futures.ProcessPoolExecutor` | Threads cannot use more than one core for Python bytecode |
| **Neither is fast enough** | A queue and separate workers (Celery, RQ, a message broker) | The unit of concurrency is a process on another machine |

Reaching for threads to speed up a CPU-bound loop makes it *slower*, because the threads fight over the GIL.

### Keep concurrency code separate

Concurrency is a responsibility of its own (see the
[Single Responsibility Principle](#single-responsibility-principle)). Do not scatter locks and thread
creation through your business logic.

**Bad** :angry:

```python
class ReportBuilder:
    def build(self, accounts: list[Account]) -> Report:
        results, threads = [], []
        lock = threading.Lock()

        def work(account: Account) -> None:
            value = self._summarise(account)      # business logic
            with lock:                            # ...tangled with threading
                results.append(value)

        for account in accounts:
            thread = threading.Thread(target=work, args=(account,))
            thread.start()
            threads.append(thread)
        for thread in threads:
            thread.join()
        return Report(results)
```

**Good** :smiley:

```python
class ReportBuilder:
    def summarise(self, account: Account) -> Summary:      # pure, single-threaded, testable
        ...


def build_report(accounts: list[Account], builder: ReportBuilder) -> Report:
    with ThreadPoolExecutor(max_workers=8) as pool:        # concurrency lives here, alone
        summaries = list(pool.map(builder.summarise, accounts))
    return Report(summaries)
```

`summarise` can now be tested and reasoned about without a single thought about threads, and the concurrency
strategy can be swapped for a process pool by changing one line.

### Limit the scope of shared data

Every piece of mutable data shared between threads is a place a race condition can live. Two defences, in
order of preference:

**1. Do not share.** Give each task its own data and combine the results at the end. Pure functions
(see [Pure Functions](#pure-functions)) are automatically thread safe.

**2. If you must share, guard it — and keep the critical section tiny.**

**Bad** :angry:

```python
balance = 0

def deposit(amount: int) -> None:
    global balance
    balance = balance + amount      # read, add, write: not atomic
```

Run that in ten threads and the final balance will be wrong, because another thread can run between the read
and the write.

**Good** :smiley:

```python
from threading import Lock


class Balance:
    def __init__(self) -> None:
        self._lock = Lock()
        self._amount = 0

    def deposit(self, amount: int) -> None:
        with self._lock:            # the lock is owned by the data it protects
            self._amount += amount

    @property
    def amount(self) -> int:
        with self._lock:
            return self._amount
```

Better still, let a `queue.Queue` do the synchronising for you — it is thread-safe by design, and a pipeline
of queues has no locks to forget.

### Async: do not block the event loop

With `asyncio`, a single blocking call stalls every other task in the process.

**Bad** :angry:

```python
async def fetch_all(urls: list[str]) -> list[str]:
    return [requests.get(url).text for url in urls]     # blocking, and sequential
```

**Good** :smiley:

```python
import asyncio
import httpx


async def fetch_all(urls: list[str]) -> list[str]:
    async with httpx.AsyncClient() as client:
        responses = await asyncio.gather(*(client.get(url) for url in urls))
    return [response.text for response in responses]
```

If you genuinely must call blocking code, push it off the loop:

```python
result = await asyncio.to_thread(legacy_blocking_call, argument)
```

### Practical rules

- **Write correct single-threaded code first,** and only then make it concurrent. Debugging a design flaw
  and a race at the same time is misery.
- **Never assume a test failure is a fluke.** "It passes if I run it again" is the signature of a race
  condition, not of a bad machine. Do not mark it flaky; find it.
- **Make your threaded code tunable and runnable on more threads than you have cores.** Bugs hide at low
  concurrency.
- **Acquire locks in a consistent order** everywhere, or you will deadlock.
- **Prefer immutable objects** across thread boundaries; `@dataclass(frozen=True)` costs nothing.
- **Use `concurrent.futures` before `threading`.** The pool handles lifecycle, exceptions and results;
  raw threads make you handle all three by hand.

**[⬆ back to top](#table-of-contents)**

## **Error Handling**

---

Error handling is important, but if it obscures logic, it is wrong. Things go wrong in every program; the
question is whether the code that deals with it hides the code that does the work.

### Use exceptions rather than return codes

Returning a status code forces every caller to check it, and nothing stops them forgetting.

**Bad** :angry:

```python
def delete_page(page_id: str) -> int:
    if not exists(page_id):
        return E_NOT_FOUND
    if not can_delete(page_id):
        return E_FORBIDDEN
    ...
    return OK


code = delete_page(page_id)          # nothing forces you to look at this
if code == OK:
    ...
```

**Good** :smiley:

```python
def delete_page(page_id: str) -> None:
    if not exists(page_id):
        raise PageNotFound(page_id)
    if not can_delete(page_id):
        raise DeletionForbidden(page_id)
    ...
```

The happy path is now the only path in the function body, and a caller who ignores the failure gets a loud
traceback instead of silent corruption.

### Separate the happy path from the error path

`try` blocks are transactions: what follows `except` should leave the program in a consistent state
regardless of what the block managed to do. Extract the bodies so the structure is visible.

**Bad** :angry:

```python
def report(path: str) -> None:
    try:
        handle = open(path)
        rows = [line.split(',') for line in handle]
        total = sum(Decimal(row[2]) for row in rows)
        print(f'Total: {total}')
        handle.close()
    except Exception as error:
        print(error)
```

**Good** :smiley:

```python
def report(path: str) -> None:
    try:
        print(f'Total: {total_of(path)}')
    except FileNotFoundError:
        logger.error('No such report file: %s', path)
        raise


def total_of(path: str) -> Decimal:
    with open(path) as handle:
        rows = (line.split(',') for line in handle)
        return sum((Decimal(row[2]) for row in rows), start=Decimal('0'))
```

### Never swallow an exception

**Bad** :angry:

```python
try:
    charge_customer(order)
except Exception:
    pass                     # the customer was never charged; nobody will ever know
```

A bare `except: pass` is the single most expensive line in this guide. If you truly can continue, say why in
a comment, catch the *specific* exception, and log it.

**Good** :smiley:

```python
try:
    send_analytics_event(order)
except AnalyticsUnavailable:
    # Analytics is best-effort: a dropped event must never block a sale.
    logger.warning('Analytics event dropped for order %s', order.id)
```

Related: `except Exception` also catches `KeyboardInterrupt`'s siblings and programming errors like
`AttributeError`, turning a bug into a mystery. Catch the narrowest exception that describes what you can
actually handle.

### Provide context with your exceptions

An exception should say what failed and what was being attempted.

**Bad** :angry:

```python
raise ValueError('invalid')
```

**Good** :smiley:

```python
raise InvalidTradeFile(
    f'Row {row_number} of {path} has {len(fields)} fields, expected 5'
)
```

When you re-raise, keep the original cause so the traceback tells the whole story:

```python
try:
    response = client.get(url)
except httpx.HTTPError as error:
    raise PriceSourceUnavailable(url) from error      # 'from' preserves the chain
```

### Define exception classes for the caller's needs

Group errors by what the caller will *do* about them, not by which library produced them.

**Bad** :angry:

```python
try:
    port = acme_port.open()
except DeviceResponseException as error:
    log(error)
except ATM1212UnlockedException as error:
    log(error)
except GMXError as error:
    log(error)
```

**Good** :smiley:

```python
class PortDeviceFailure(Exception):
    """Anything that goes wrong while talking to a port device."""


class AcmePort:
    def __init__(self, port: int) -> None:
        self._inner = ACMEPort(port)

    def open(self) -> None:
        try:
            self._inner.open()
        except (DeviceResponseException, ATM1212UnlockedException, GMXError) as error:
            raise PortDeviceFailure() from error
```

Wrapping a third-party API like this is almost always worth it: it collapses a family of errors into one
concept, and it keeps the vendor's names from leaking into your whole codebase (see
[Dependency Inversion](#dependency-inversion-principle)).

### Don't return None, don't pass None

Returning `None` pushes a check onto every caller, and the one who forgets gets an `AttributeError` far from
the cause.

**Bad** :angry:

```python
def find_employee(employee_id: str) -> Employee | None:
    ...

total = find_employee('123').salary       # AttributeError one day
```

**Good** :smiley:

```python
def employees_in(department: str) -> list[Employee]:
    return self._by_department.get(department, [])      # empty list, never None


for employee in employees_in('sales'):                   # no check needed
    ...
```

Where absence is genuinely a valid outcome, make it explicit in the type (`Employee | None`) so the type
checker forces the caller to handle it — or raise, if absence is an error.

Passing `None` into a function is worse, because no amount of defensive code in the callee fixes a caller
that should not have done it. Prefer a default, an empty collection, or a separate function.

### Fail fast

Validate at the boundary, then trust your own data.

```python
@dataclass(frozen=True)
class Percentage:
    value: Decimal

    def __post_init__(self) -> None:
        if not 0 <= self.value <= 100:
            raise ValueError(f'Percentage must be between 0 and 100, got {self.value}')
```

Once a `Percentage` exists, no function that takes one ever has to check it again. This is how types replace
defensive programming.

### Use `finally` and context managers for cleanup

**Bad** :angry:

```python
connection = pool.acquire()
result = query(connection)        # if this raises, the connection leaks
pool.release(connection)
return result
```

**Good** :smiley:

```python
with pool.acquire() as connection:
    return query(connection)
```

If a resource you own does not provide a context manager, write one — `contextlib.contextmanager` makes it
three lines:

```python
from contextlib import contextmanager


@contextmanager
def acquired(pool: Pool):
    connection = pool.acquire()
    try:
        yield connection
    finally:
        pool.release(connection)
```

**[⬆ back to top](#table-of-contents)**

## **Formatting**

---

Formatting is about communication, and communication is the professional developer's first order of
business. Code formatting matters far too much to ignore — and far too much to argue about, which is why the
right answer in Python is to **stop doing it by hand**.

### Automate it, then never discuss it again

```bash
pip install black ruff
black .          # formats every file the same way, no options to argue over
ruff check .     # catches unused imports, shadowed builtins, dead code
```

Add both to a pre-commit hook and to CI, and the entire class of "style feedback" disappears from code
review, leaving reviewers free to talk about design.

[PEP 8](https://peps.python.org/pep-0008/) is the baseline every Python tool implements: four spaces per
indent, `snake_case` for functions and variables, `PascalCase` for classes, `UPPER_SNAKE_CASE` for
constants, and a line limit (88 in `black`, 79 in strict PEP 8).

### Vertical formatting

**Files should be small.** A module of 200 lines is comfortable; one of 2,000 is a filing cabinet, not a
document.

**Blank lines separate concepts.** Each blank line is a visual cue that a new thought begins.

```python
import csv
from decimal import Decimal

TAX_RATE = Decimal('0.16')


def total_with_tax(subtotal: Decimal) -> Decimal:
    return subtotal + subtotal * TAX_RATE


def read_rows(path: str) -> list[list[str]]:
    with open(path) as handle:
        return list(csv.reader(handle))
```

**Related lines stay together.** Do not separate a variable from its first use with unrelated code.

**Vertical distance matters.** Declare a variable as close to its use as possible; keep a function near the
functions it calls (see [the Stepdown Rule](#the-stepdown-rule)); keep conceptually related functions close
even if neither calls the other.

**Newspaper metaphor.** A module should read like a newspaper article: the name at the top tells you the
topic, the first functions give the high-level story, and the details appear further down.

### Horizontal formatting

**Lines should be short enough to read without scrolling.** If a line is too long, the usual cause is too
much happening on it:

**Bad** :angry:

```python
return [transform(item) for item in fetch(source) if item.is_valid and item.created_at > cutoff and item.owner in allowed]
```

**Good** :smiley:

```python
def is_relevant(item: Item) -> bool:
    return item.is_valid and item.created_at > cutoff and item.owner in allowed


return [transform(item) for item in fetch(source) if is_relevant(item)]
```

**Use whitespace to show association.** Space around low-precedence operators, none around high:

```python
total = base_price + quantity*unit_price
```

**Do not align assignments in columns.** It looks tidy and it produces a diff on every line whenever one
name changes.

**Indentation shows hierarchy — do not collapse it.** Even when Python allows a one-liner:

**Bad** :angry:

```python
if not accounts: return None
```

**Good** :smiley:

```python
if not accounts:
    return None
```

### Team rules beat personal preference

A codebase should look like it was written by one person, not by a committee of individually tasteful
programmers. Agree the configuration once, put it in `pyproject.toml`, and let the tool enforce it:

```toml
[tool.black]
line-length = 88

[tool.ruff]
line-length = 88
select = ["E", "F", "I", "B"]     # errors, pyflakes, import order, bugbear
```

**[⬆ back to top](#table-of-contents)**

## **Comments**

---

> **Don't comment bad code — rewrite it.** — Brian W. Kernighan and P. J. Plauger

A comment is a failure to express yourself in code. Sometimes it is a necessary failure, but it is never a
success to be proud of, because comments are not compiled, not tested and not maintained. The code moves on;
the comment stays and starts lying.

### Explain yourself in code

**Bad** :angry:

```python
# check if the employee is eligible for full benefits
if employee.flags & HOURLY_FLAG and employee.age > 65:
    ...
```

**Good** :smiley:

```python
if employee.is_eligible_for_full_benefits():
    ...
```

It usually takes only a few seconds of thought to replace a comment with a function or a variable whose name
says the same thing — and that name gets checked by tests and updated by refactoring tools.

### Bad comments

**Redundant comments** — the code already said it, twice as fast:

```python
i += 1      # increment i
```

**Misleading comments** — a comment that was true once. This is the worst kind, because readers trust it.

**Commented-out code** — delete it. Git remembers; nobody else does, and everyone who sees it is afraid to
remove it.

```python
# def old_calculation(x):
#     return x * 1.15      # from 2019, may still be needed?
```

**Journal comments** — a changelog at the top of every file. Version control does this better.

```python
# 2019-01-04  KM  Added tax handling
# 2020-06-11  VJ  Fixed rounding
```

**Noise comments** — comments that say nothing at all:

```python
class Account:
    """The Account class."""

    def __init__(self) -> None:
        """The constructor."""
```

**Position markers and closing-brace comments** — `# ---- helpers ----` is a sign a module should be split.

**Attributions** — `# added by Kasozi`. Git blame knows.

**Too much information** — do not paste the RFC into a docstring; link to it.

### Good comments

Some comments earn their place:

**Legal comments** — a licence header, kept short.

**Intent — the "why", not the "what"**:

```python
# Sort by descending price first: the pricing API returns ties in a random
# order, and the test-suite needs a stable ordering to compare against.
items.sort(key=lambda item: (-item.price, item.sku))
```

**Warning of consequences**:

```python
@pytest.mark.slow      # takes ~40s: it builds the full search index
def test_full_reindex(): ...
```

**Clarification of something you cannot change**:

```python
assert response.status_code == 202     # the vendor returns 202, not 201, on create
```

**`TODO` comments** — acceptable as a marker for work that cannot be done now, as long as they are scanned
and cleared regularly:

```python
# TODO(kasozi): remove once the v1 pricing endpoint is decommissioned (JIRA-142).
```

**Docstrings on public APIs** — for anything another team or another program will import, a docstring is
documentation, not a comment, and it belongs there:

```python
def value_added_tax(invoice: Invoice) -> Decimal:
    """Return the VAT owed on ``invoice``.

    The rate is applied to the taxable amount only; zero-rated lines are
    excluded. See the Finance Act, section 5(2).

    Raises:
        NegativeAmount: if the invoice total is below zero.
    """
```

Note what that docstring does **not** do: it does not restate the signature. Types are the parameter
documentation; the prose is for the rules a reader cannot infer.

> **Rule of thumb:** if a comment explains *what* the code does, delete it and fix the code. If it explains
> *why* the code is the way it is, keep it — that information exists nowhere else.

**[⬆ back to top](#table-of-contents)**

## **Translation**

---

This guide is a Python adaptation of Robert C. Martin's *Clean Code*, produced by
[LuxDev HQ](https://github.com/LuxDevHQ) students and contributors.

The same material exists for other languages, and the principles carry over even where the idioms do not:

- **JavaScript** — [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)
- **Python** — [clean-code-python](https://github.com/zedr/clean-code-python)
- **TypeScript** — [clean-code-typescript](https://github.com/labs42io/clean-code-typescript)
- **PHP** — [clean-code-php](https://github.com/jupeter/clean-code-php)
- **Ruby** — [clean-code-ruby](https://github.com/uohzxela/clean-code-ruby)
- **Go** — [clean-go-article](https://github.com/Pungyeon/clean-go-article)

### Translations of this guide

Translations into other natural languages are welcome. If you would like to translate these notes:

1. Fork the repository.
2. Copy `README.md` to `README.<language-code>.md` (for example `README.sw.md` for Kiswahili).
3. Translate the prose. **Leave the code identifiers in English** — code in the wild is written in English,
   and a reader who learns `pesa_taslimu` here will not recognise `cash` in a real codebase.
4. Add your language to the list below and open a pull request.

| Language | Link | Status |
| --- | --- | --- |
| English | [README.md](README.md) | Complete |

Contributions of any size are welcome — a fixed typo, a clearer example, or a whole chapter. Add yourself to
`authors.json` when you open your first pull request.

**[⬆ back to top](#table-of-contents)**
