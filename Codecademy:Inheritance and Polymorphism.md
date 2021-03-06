# Codecademy: Inheritance and Polymorphism
Notes from Learn Python3 lesson in Codecademy

## 1. Inheritance
If the bulk of a class’s definition is useful, but we have a new use case that is distinct from how the original class was used, we can _inherit_ from the original class. Think of inheritance as a remix — it sounds a lot like the original, but there’s something… different about it.
```py
class User:
  is_admin = False
  def __init__(self, username)
    self.username = username

class Admin(User):
  is_admin = True
```
Above we defined `User` as our _base class_. We want to create a new class that inherits from it, so we created the _subclass_ `Admin`. In the above example, `Admin` has the same constructor as `User`. Only the class variable `is_admin` is set differently between the two.

Sometimes a base class is called a _parent class_. In these terms, the class inheriting from it, the subclass, is also referred to as a _child class_.

## 2. Exceptions

There’s one very important family of class definitions built in to the Python language. An _Exception_ is a class that inherits from Python’s `Exception` class.

We can validate this ourselves using the `issubclass()` function. `issubclass()` is a Python built-in function that takes two parameters. `issubclass()` returns `True` if the first argument is a subclass of the second argument. It returns `False` if the first class is not a subclass of the second. `issubclass()` raises a `TypeError` if either argument passed in is not a class.
```py
issubclass(ZeroDivisionError, Exception)
# Returns True
```
Above, we checked whether `ZeroDivisionError`, the exception raised when attempting division by zero, is a subclass of `Exception`. It is, so `issubclass` returns `True`.

Why is it beneficial for exceptions to inherit from one another? Let’s consider an example where we create our own exceptions. What if we were creating software that tracks our kitchen appliances? We would be able to design a suite of exceptions for that need:

```py
class KitchenException(Exception):
  """
  Exception that gets thrown when a kitchen appliance isn't working
  """
class MicrowaveException(KitchenException):
  """
  Exception for when the microwave stops working
  """
class RefrigeratorException(KitchenException):
  """
  Exception for when the refrigerator stops working
  """
 ```

In this code, we define three exceptions. First, we define a `KitchenException` that acts as the parent to our other, specific kitchen appliance exceptions. `KitchenException` subclasses `Exception`, so it behaves in the same way that regular `Exception`s do. Afterward we define `MicrowaveException` and `RefrigeratorException` as subclasses.

Since our exceptions are subclassed in this way, we can catch any of `KitchenException`‘s subclasses by catching `KitchenException`. For example:

```py
def get_food_from_fridge():
  if refrigerator.cooling == False:
    raise RefrigeratorException
  else:
    return food

def heat_food(food):
  if microwave.working == False:
    raise MicrowaveException
  else:
    microwave.cook(food)
    return food

try:
  food = get_food_from_fridge()
  food = heat_food(food)
except KitchenException:
  food = order_takeout()
```

In the above example, we attempt to retrieve food from the fridge and heat it in the microwave. If either `RefrigeratorException` or `MicrowaveException` is raised, we opt to order takeout instead. We catch both `RefrigeratorException` and `MicrowaveException` in our try/except block because both are subclasses of `KitchenException`.

Explore Python’s exception hierarchy in the [Python documentation](https://docs.python.org/3/library/exceptions.html#exception-hierarchy)!

## 3. Overriding Methods

 What if one of the methods needs to be implemented differently? In Python, all we have to do to _override_ a method definition is to offer a new definition for the method in our subclass. An overridden method is one that has a different definition from its parent class.
```py
class User:
  def __init__(self, username, permissions):
    self.username = username
    self.permissions = permissions

  def has_permission_for(self, key):
    if self.permissions.get(key):
      return True
    else:
      return False
```
Above we defined a class `User` which takes a `permissions` parameter in its constructor. Let’s assume `permissions` is a `dict`. `User` has a method `.has_permission_for()` implemented, where it checks to see if a given key is in its `permissions` dictionary. We could then define our `Admin` user like this:
```py
class Admin(User):
  def has_permission_for(self, key):
    return True
```
Here we define an `Admin` class that subclasses `User`. It has all methods, attributes, and functionality that `User` has. However, if you call `has_permission_for` on an instance of `Admin`, it won’t check its `permissions` dictionary. Since this `User` is also an `Admin`, we just say they have permission to see everything!
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTczODQ5MDU5Niw1MjM2NjcxOV19
-->