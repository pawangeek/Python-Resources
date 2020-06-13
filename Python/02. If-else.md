# If-Else Guide

## Preface

Writing conditional branch code is an integral part of the coding process.

If we use roads as a metaphor, the code in the real world is never a straight highway, but more like a map of an urban area composed of countless forks. Our coder is like a driver, we need to tell our program whether we need to turn left or right at the next intersection.

Writing good conditional branch code is very important, because bad, complicated branch handling is very confusing, which reduces the quality of the code. Therefore, this article will focus on the points that should be noted when writing branch code in Python.

### Branch code in Python

Python supports the most common `if/else` conditional branch statement, but it lacks the `switch/case` statement common in other programming languages.

In addition, Python also provides the else branch for `for/while` loops and `try/except` statements. In some special scenarios, they can show their talents.

Below I will talk about how to write excellent conditional branch code from three aspects: `best practice`, `common tips` and `common traps`.

## Best Practices

### 1. Avoid nesting multiple branches

If this article can only be cut down to a sentence and then ended, then that sentence must be **"We must do our best to avoid branch nesting"**.

Too deep branch nesting is one of the easiest mistakes for many novice programmers. If a novice JavaScript programmer wrote a lot of branches and nesting, then you might see layers of braces: `if {if {if {... }}}`. Commonly known as **Nested If Statement Hell**.

But because Python uses indentation instead of `{}`, nested branches that are too deep can have more serious consequences than other languages. For example, too many indentation levels can easily cause the code to exceed the word limit per line specified in [PEP8](https://www.python.org/dev/peps/pep-0008/). Let's look at this code:

```Python
def buy_fruit(nerd, store):
    """
    - Go to the fruit shop to buy apples    
    - First check if the store is open
    - If you have an apple, buy one
    - If you don’t have enough money, go home and get the money again
    """
    
    if store.is_open():
        if store.has_stocks("apple"):
            if nerd.can_afford(store.price("apple", amount=1)):
                nerd.buy(store, "apple", amount=1)
                return
            else:
                nerd.go_home_and_get_money()
                return buy_fruit(nerd, store)
        else:
            raise MadAtNoFruit("no apple in store!")
    else:
        raise MadAtNoFruit("store is closed!")
```

The biggest problem with the above code is that the original conditional branch requirements are too directly translated, resulting in just a dozen lines of code containing three levels of nested branches.

Such code is poorly readable and maintainable. But we can use a very simple trick: **"end early"** to optimize this code:

```python
def buy_fruit(nerd, store):
    if not store.is_open():
        raise MadAtNoFruit("store is closed!")

    if not store.has_stocks("apple"):
        raise MadAtNoFruit("no apple in store!")

    if nerd.can_afford(store.price("apple", amount=1)):
        nerd.buy(store, "apple", amount=1)
        return
    else:
        nerd.go_home_and_get_money()
        return buy_fruit(nerd, store)
```

**"End early"** means: Use `return` or `raise` statements in the function to end the function in the branch in advance. **For example, in the new `buy_fruit` function, when the branch condition is not met, we directly throw an exception and end this code branch. Such code has no nested branches and is more straightforward and easier to read.

### 2. Encapsulate logic judgments that are too complex

If the expression in the conditional branch is too complicated and there are too many `not/and/or`, the readability of this code will be greatly reduced, such as the following code:

```
# If the event is still open and the remaining places in the event are greater than 10, it is for all genders or women, or the level is greater than 3
# Active users issue 10,000 coins
if activity.is_active and activity.remaining> 10 and \
        user.is_active and (user.sex =='female' or user.level> 3):
    user.add_coins(10000)
    return
```

For such code, we can consider encapsulating the specific branch logic into a function or method to achieve the purpose of simplifying the code:

```
if activity.allow_new_user() and user.match_activity_condition():
    user.add_coins(10000)
    return
```

In fact, after rewriting the code, the previous comment text can actually be removed. **Because the following code has reached the purpose of self-explanation. As for specific **What kind of users meet the conditions of the activity?** This kind of question should be answered by the specific `match_activity_condition()` method.

> **Hint:** Proper encapsulation not only directly improves the readability of the code, in fact, if the above activity judgment logic appears more than once in the code, encapsulation is even more necessary. Otherwise, repeating the code will greatly destroy the maintainability of this logic.

### 3. Pay attention to the repeated code under different branches

Repeated code is the natural enemy of code quality, and conditional branch statements can easily become the hardest hit by repeated code. Therefore, when we write conditional branch statements, we need to pay special attention not to produce unnecessary repetitive code.

Let's take a look at this example:

```python
# For new users, create a new user profile, otherwise update the old profile
if user.no_profile_exists:
    create_user_profile(
        username=user.username,
        email=user.email,
        age=user.age,
        address=user.address,
        # For new users, set the user's points to 0
        points=0,
        created=now(),
    )
else:
    update_user_profile(
        username=user.username,
        email=user.email,
        age=user.age,
        address=user.address,
        updated=now(),
    )
```

In the above code, we can see at a glance that under different branches, the program calls different functions and does different things. However, because of the existence of repetitive code, it is difficult for us to easily distinguish the difference between the two.

In fact, thanks to the dynamic nature of Python, we can simply rewrite the above code, so that the readability can be significantly improved:

```python
if user.no_profile_exists:
    profile_func = create_user_profile
    extra_args = {'points': 0,'created': now()}
else:
    profile_func = update_user_profile
    extra_args = {'updated': now()}

profile_func(
    username=user.username,
    email=user.email,
    age=user.age,
    address=user.address,
    **extra_args
)
```

When you write branch code, please pay attention to **repetitive code blocks produced by branches**, if you can simply eliminate them, then don’t hesitate.

### 4. Use ternary expressions with caution

The ternary expression is a syntax supported only after Python 2.5 version. Before that, the Python community once thought that ternary expressions were unnecessary, and we needed to use x and a or b to simulate it.

The fact is that in many cases, code that uses ordinary `if/else` statements is indeed more readable. Blindly pursuing ternary expressions can easily entice you to write complex, poorly readable code.

So, remember to use simple ternary expressions to handle simple logical branches.

```python
language = "python" if you.favor("dynamic") else "golang"
```

For the vast majority of cases, use ordinary `if/else` statements.

## Common Tips

### 1. Use "De Morgan's Law"

When doing branch judgments, we sometimes write code like this:

```python
# If the user is not logged in or the user is not using chrome, refuse to provide services
if not user.has_logged_in or not user.is_from_chrome:
    return "our service is only available for chrome logged in user"
```

When you first see the code, do you need to think about it for a while to understand what it wants to do? This is because two `not` and one `or` appear in the above logical expression. And we humans are just not good at handling too many logical relations of "negation" and "or".

At this time, it should be [De Morgan's Law](https://en.wikipedia.org/wiki/De_Morgan%27s_laws) played. In layman's terms, De Morgan's law is that `not A or not B` is equivalent to `not (A and B)`. Through such conversion, the above code can be rewritten as follows:

```python
if not (user.has_logged_in and user.is_from_chrome):
    return "our service is only available for chrome logged in user"
```

How is the code a lot easier to read? Remember De Morgan's Law, which is very useful for simplifying the logic of code in conditional branches.

### 2. "Boolean true and false" of custom objects

We often say that in Python, "everything is an object". In fact, not only "everything is an object", we can also use many magic methods (called in the document: [user-defined method](https://docs.python.org/3/reference/datamodel.html)) , To customize various behaviors of objects. We can affect the execution of the code in many magical ways that cannot be done in other languages.

For example, all objects in Python have their own "Boolean true and false":

- Objects with false boolean values: `None`, `0`, `False`, `[]`, `()`, `{}`, `set()`, `frozenset()`, ... ...
- Objects with boolean true values:  `0`, `True`, non-empty sequences, tuples, ordinary user class instances,...

Through the built-in function `bool()`, you can easily check the boolean true or false of an object. And Python uses this value when making conditional branch judgments:

```python
>>> bool(object())
True
```

The point is coming, although the boolean values ​​of all user class instances are true. But Python provides a way to change this behavior: **`__bool__` magic methods of custom classes** * (`__nonzero__` in Python 2.X version)*. When the class defines the `__bool__` method, its return value will be treated as the boolean value of the class instance.

In addition, `__bool__` is not the only way to affect the boolean truth of an instance. If the class does not define the `__bool__` method, Python will also try to call the `__len__` method* (that is, call the `len` function on any sequence object)*, and judge whether the instance is true or false based on whether the result is `0`.

So what's the use of this feature? Look at the following code:

```python
class UserCollection(object):

    def __init__(self, users):
        self._users = users


users = UserCollection([piglei, raymond])

if len(users._users)> 0:
    print("There's some users in collection!")
```

In the above code, the length of `users._users` is used to determine whether there is content in `UserCollection`. In fact, by adding `__len__` magic method to `UserCollection`, the above branch can be made simpler:

```python
class UserCollection:

    def __init__(self, users):
        self._users = users

    def __len__(self):
        return len(self._users)


users = UserCollection([piglei, raymond])

# After defining the __len__ method, the UserCollection object itself can be used for Boolean judgment
if users:
    print("There's some users in collection!")
```

By defining the magic methods `__len__` and `__bool__`, we can let the class control the boolean true and false values ​​we want to show, and make the code more pythonic.

### 3. Use all() / any() in conditional judgment

The two functions `all()` and `any()` are very suitable for use in conditional judgment. These two functions accept an iterable object and return a Boolean value, where:

- `all(seq)`: return `True` only if all objects in `seq` are boolean, otherwise return `False`
- `any(seq)`: as long as any object in `seq` is boolean, return `True`, otherwise return `False`

Suppose we have the following code:

```python
def all_numbers_gt_10(numbers):
    """ returns True only if all numbers in the sequence are greater than 10
    """
    if not numbers:
        return False

    for n in numbers:
        if n <= 10:
            return False
    return True
```

If you use the `all()` built-in function, combined with a simple generator expression, the above code can be written like this:

```python
def all_numbers_gt_10_2(numbers):
    return bool(numbers) and all(n> 10 for n in numbers)
```

Simple, efficient, and without loss of availability.

### 4. Use try/while/for in else branch

Let's look at this function:

```python
def do_stuff():
    first_thing_successed = False
    try:
        do_the_first_thing()
        first_thing_successed = True
    except Exception as e:
        print("Error while calling do_some_thing")
        return

    # Only when first_thing completes successfully, do the second thing
    if first_thing_successed:
        return do_the_second_thing()
```

In the function `do_stuff`, we want to continue to make the second function call only after `do_the_first_thing()` is successfully called* (that is, no exception is thrown)*. In order to do this, we need to define an additional variable `first_thing_successed` as a marker.

In fact, we can achieve the same effect with a simpler method:

```python
def do_stuff():
    try:
        do_the_first_thing()
    except Exception as e:
        print("Error while calling do_some_thing")
        return
    else:
        return do_the_second_thing()
```

After appending the `else` branch at the end of the `try` statement block, `do_the_second_thing()` under the branch will only be executed normally after all statements under **try (that is, no exceptions, no return, break, etc.) are completed carried out**.

Similarly, the `for/while` loop in Python also supports the addition of the `else` branch, which means that the else branch is executed only after the iteration object used by the loop is exhausted normally, or the condition variable used by the while loop becomes False Code.

## Common pitfalls

### 1. Comparison with None

In Python, there are two ways to compare variables: `==` and `is`, which are fundamentally different in meaning:

- `==`: indicates whether the **value** pointed by the two is consistent
- `is`: indicates whether the two refer to the same content in memory, that is, if id(x) is equal to id(y)

`None` is a singleton object in the Python language. If you want to determine whether a variable is None, remember to use `is` instead of `==`, because only `is` can represent a strict one. Whether the variable is None.

Otherwise, the following situation may occur:

```python
>>> class Foo(object):
... def __eq__(self, other):
... return True
...
>>> foo = Foo()
>>> foo == None
True
```

In the above code, the Foo class easily satisfies the condition of `== None` by customizing the magic method of `__eq__`.

**So, when you want to judge whether a variable is None, please use `is` instead of `==`.**

### 2. Pay attention to the operation priority of and and or

Take a look at the following two expressions and guess whether they have the same value?

```python
>>> (True or False) and False
>>> True or False and False
```

The answer is: different, their values are `False` and `True`, did you guess right?

The key to the problem is: **The `and` operator has a higher priority than `or`**. Therefore, the second expression above actually appears to Python as `True or (False and False)`. So the result is `True` instead of `False`.

When writing expressions that contain multiple `and` and `or`, please pay extra attention to the arithmetic priority of `and` and `or`. Even if the execution priority is exactly what you need, you can add extra brackets to make the code clearer.
