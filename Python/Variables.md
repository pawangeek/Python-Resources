# Variables Guide

## Variables and code quality

As the first article in the "Python Craftsman" series, I want to talk about "Variables" first. Because how to define and use variables has always been one of the first skills to learn in any programming language.

Whether variables are used well or not is very important to the quality of the code. Among the many questions about variables, it is especially important to give them a good name.

## How to name variables

In the field of computer science, there is a famous motto (playful saying):

> There are only two hard things in Computer Science: cache invalidation and naming things.
> There are only two difficult things in the field of computer science: cache expiration and naming things
> 
> - Phil Karlton

Needless to say, the difficulty of the first "cache expiration problem" will be understood by anyone who has used the cache. As for the difficulty of the second "Name a thing", I also understand it deeply. One of the darkest afternoons I have spent in my career was sitting in front of the monitor and scratching my head to give a suitable name for a new project.

The most names from programming, also count various variables. It is important to give a good name to a variable, because good variable naming can greatly improve the overall readability of the code. **

The following points are the basic principles that I should summarize when naming variables.

### 1. Variable names must be descriptive, not too broad

Within the **acceptable length range**, the variable name can describe the content it points to as accurately as possible. So, try not to use those too broad words as your variable names:

- **BAD**: `day`, `host`, `cards`, `temp`
- **GOOD**: `day_of_week`, `hosts_to_reboot`, `expired_cards`

### 2. Variable names are best for people to guess the type

Everyone who learns Python knows that Python is a dynamically typed language, and it (at least before [PEP 484](https://www.python.org/dev/peps/pep-0484/) appeared) has no variable types statement. So when you see a variable, you can't easily know what type it is except for guessing through the context.

However, people usually have some intuitive conventions for the relationship between variable names and variable types. I summarize them below.

#### "What kind of name will be treated as bool?"

The biggest characteristic of a Boolean variable is that it only has two possible values ​​**『Yes』** or **『No』**. Therefore, variable names modified with non-black and white words such as `is` and `has` would be a good choice. The principle is: **Make people who read the variable name think that this variable will only have "yes" or "no" two values**.

Here are a few good examples:

- `is_superuser`: "Whether superuser", there will only be two values: yes/no
- `has_error`: "There is no error", there will only be two values: yes/no
- `allow_vip`: "Whether VIP is allowed", there will only be two values: allowed/not allowed
- `use_msgpack`: "Whether to use msgpack", there will only be two values: use/not use
- `debug`: "Whether to enable debug mode" is regarded as bool mainly because of convention

#### What kind of names will be treated as int/float?

When people see names related to numbers, they all assume that they are of type int/float. The following are more common:

- All words interpreted as numbers, such as: `port (port number)`, `age (age)`, `radius (radius)`, etc.
- Use words ending in _id, for example: `user_id`, `host_id`
- Use words starting or ending with length/count, for example: `length_of_username`, `max_length`, `users_count`

**Note**: Do not use ordinary complex numbers to represent an int type variable, such as `apples`, `trips`, it is better to use `number_of_apples`, `trips_count` instead.

#### Other types

For complex types such as str, list, tuple, and dict, it is difficult to have a unified rule that allows us to guess the variable type by name. For example, `headers` may be either a list of header information or a dict containing header information.

For these types of variable names, the most recommended way is to write a canonical document, using the sphinx format in the document string of the function and method ([Document tool used by the official Python documentation](http://www.sphinx-doc.org/en/stable/)) to label all variable types. 

### 3. Proper use of "Hungarian Nomenclature"

The first time I knew [Hungarian Nomenclature](https://en.wikipedia.org/wiki/Hungarian_notation) was in [Joel on Software's blog post](http://www.joelonsoftware.com/articles/Wrong.html). In short, the Hungarian nomenclature is to abbreviate the "type" of the variable and put it at the front of the variable name.

The key is that the variable "type" mentioned here does not refer to the traditional int/str/list type, but refers to those types that are related to the business logic of your code.

For example, there are two variables in your code: `students` and `teachers`, and they all point to a list containing Person objects. After using "Hungarian Nomenclature", these two names can be rewritten as follows:

students -> `pl_students`
teachers -> `pl_teachers`

Where pl is the acronym for **person list**. When the variable name is prefixed, if you see the variable that starts with `pl_`, you can know the type of value it points to.

In many cases, using the "Hungarian nomenclature" is a good idea, because it can improve the readability of your code, especially when those variables are many and the same type appears multiple times. Just be careful not to abuse it.

### 4. Variable names should be as short as possible, but never too short

Earlier, we mentioned that variable names should be descriptive. If you do not put any restrictions on this principle, then you are likely to write such a highly descriptive variable name: `how_much_points_need_for_level2`. If the code is full of such long variable names, it is a disaster for code readability.

A good variable name, the length should be controlled at **two to three words**. For example, the above name can be abbreviated as `points_level2`.

**In most cases, you should avoid short names with only one or two letters**, such as array index three Musketeers `i`, `j`, `k`, use names with clear meaning, such as person_index It’s always better to replace them.

#### Exceptions using short names

Sometimes, there are some exceptions to the above principle. When some variable names with clear meanings but longer appear repeatedly, in order to make the code more concise, it is entirely possible to use short abbreviations. But in order to reduce the cost of understanding, it is best not to use too many such short names in the same piece of code.

For example, when importing modules in Python, short names are often used as aliases. For example, the commonly used `gettext` method in Django i18n translation is usually abbreviated to `_` to use * (from django.utils.translation import ugettext as _)*

### 5. Other considerations

Some other considerations for naming variables:

- Do not use too similar variable names in the same piece of code, such as the sequence of `users`, `users1`, `user3`
- Do not use variable names with negative meanings, use `is_special` instead of `is_not_normal`

## Better-use-of-variables

Earlier I talked about how to give a good name to a variable. Let's talk about some small details that should be paid attention to when using variables in daily life.

### 1. Maintain consistency

If you call the picture variable `photo` in a method, don’t change it to `image` in other places. This will only confuse the readers of the code: Are `image` and `photo` really? The same thing? 』

In addition, although Python is a dynamically typed language, that does not mean that you can use the same variable name to represent the str type for a while, and then replace it with a list. **The variable types referred to by the same variable name also need to be consistent. **

### 2. Try not to use globals()/locals()

Maybe you first discovered globals()/locals(), which was very exciting for the built-in functions, and can’t wait to write the following extremely “simple” code:

```python
def render_trip_page(request, user_id, trip_id):
    user = User.objects.get(id=user_id)
    trip = get_object_or_404(Trip, pk=trip_id)
    is_suggested = is_suggested(user, trip)
    # Using locals() to save three lines of code, I am a genius!
    return render(request,'trip.html', locals())
```

Don't do this, it will only make people who read this code (including yourself after three months) hate you, because he needs to remember all the variables defined in this function (think this function grows to two What about Baixing?), not to mention that locals() will also pass out some unnecessary variables.

Not to mention, [The Zen of Python](https://www.python.org/dev/peps/pep-0020/) said clearly: **Explicit is better than implicit. Is better than implicit)**. So, let's honestly write the code like this:

```python
    return render(request,'trip.html', {
        'user': user,
        'trip': trip,
        'is_suggested': is_suggested
    })
```

### 3. Variable definitions are used as close as possible

This principle is commonplace. Many people (including me) have a habit when they first start learning programming. It is to write all the variable definitions together and put them at the front of the function or method.

```python
def generate_trip_png(trip):
    path = []
    markers = []
    photo_markers = []
    text_markers = []
    marker_count = 0
    point_count = 0
    ...
```

Doing so will only make your code "look neat", but it will not help to improve the readability of the code.

A better approach is to **use variable definitions as close to use as possible**. So when you read the code, you can better understand the logic of the code, rather than trying to think about what and where this variable is defined?

### 4. Reasonably use namedtuple/dict to make the function return multiple values

Python functions can return multiple values:

```python
def latlon_to_address(lat, lon):
    return country, province, city

# Use multiple return values to unpack and define multiple variables at once
country, province, city = latlon_to_address(lat, lon)
```

However, this usage will create a small problem: what if the `latlon_to_address` function needs to return "District" on a certain day?

If it is written as above, you need to find all the places where `latlon_to_address` is called, and make up the extra variable, otherwise *ValueError: too many values ​​to unpack* will find you:

```python
country, province, city, district = latlon_to_address(lat, lon)

# Or use _ to ignore the extra return value
country, province, city, _ = latlon_to_address(lat, lon)
```

For this potentially variable multi-return value function, it is more convenient to use namedtuple/dict. When you add a return value, it will not have any destructive effect on the previous function call:

```python
# 1. Use dict
def latlon_to_address(lat, lon):
    return {
        'country': country,
        'province': province,
        'city': city
    }

addr_dict = latlon_to_address(lat, lon)

# 2. Use namedtuple
from collections import namedtuple

Address = namedtuple("Address", ['country','province','city'])

def latlon_to_address(lat, lon):
    return Address(
        country=country,
        province=province,
        city=city
    )

addr = latlon_to_address(lat, lon)
```

However, there are disadvantages to doing this because the compatibility of the code with the changes has improved, but you can no longer continue to unpack and define multiple variables at once with the method of `x, y = f()`. The choice is yours.

### 5. Control the number of variables in a single function

The ability of the human brain is limited. Studies have shown that human short-term memory can only remember no more than ten names at a time. So, when one of your functions is too long (in general, a function with more than one screen will be considered a bit too long) and contains too many variables. Please split it into multiple small functions in time.

### 6. Delete those useless variables in time

This principle is very simple and easy to do. But if it is not followed, it will be a devastating blow to the quality of your code. It will make people who read your code feel fooled.

```python
def fancy_func():
    # Reader Psychology: Well, a fancy_vars is defined here
    fancy_vars = get_fancy()
    ... (after a lot of code)

    # Reader Psychology: Is this the end? Where did the previous fancy_vars go? Was it eaten by the cat?
    return result
```

Therefore, please open the smart prompt of the IDE and clean up the variables that are defined but not used in time.

### 7. Define temporary variables to improve readability

Sometimes, some complex expressions will appear in our code, like this:

```python
# Distribute 10,000 gold coins for all active users whose gender is female or whose level is greater than 3
if user.is_active and (user.sex =='female' or user.level> 3):
    user.add_coins(10000)
    return
```

See the long list behind `if`? It's a little hard to read, right? But if we assign it to a temporary variable,
Can give readers a psychological buffer and improve readability:

```
# Distribute 10,000 gold coins for all active users whose gender is female or whose level is greater than 3
user_is_eligible = user.is_active and (user.sex =='female' or user.level> 3):

if user_is_eligible:
    user.add_coins(10000)
    return
```

Defining temporary variables can improve readability. But sometimes, assigning unnecessary things to temporary variables can make the code look verbose:

```python
def get_best_trip_by_user_id(user_id):

    # Psychological activity: "Well, this value may be modified/reused in the future", let us first define it as a variable!
    user = get_user(user_id)
    trip = get_best_trip(user_id)
    result = {
        'user': user,
        'trip': trip
    }
    return result
```

In fact, the "future" you think will never come. The three temporary variables in this code can be completely removed and become like this:

```python
def get_best_trip_by_user_id(user_id):
    return {
        'user': get_user(user_id),
        'trip': get_best_trip(user_id)
    }
```

There is no need to sacrifice the current readability of the code for possible changes. If there is a need to define variables in the future, then add it later.
