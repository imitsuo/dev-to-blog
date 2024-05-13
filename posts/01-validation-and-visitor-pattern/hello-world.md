---
title: "Validation and Visitor Pattern"
published: false # if you set this to false it will publish the page as a draft
description: "How to use visitor pattern to validate the content of class"
tags: python, beginners, visitor pattern, patterns
# cover_image: ./assets/github-octocat.jpg
canonical_url: null  # set this if you have a website you want to be promoted
---

Embrace our journey to create validators with visitor pattern, the main goal here is try to create a extensive structure and cohesive validators.

Let's look at our simple class Customer.

```python
class Customer:
    name: str
    age: int
    phones: list[str]
```

If we have to validate that:
 * has informed first and last name
 * age greater than 18 years
 * has at the least one phone

It's easy to thinking and write a function(s) like:

```python

def is_valid(customer: Customer) -> Tuple[bool, list[str]]:
    errors = []

    if len(customer.name.split()) < 2:
        errors.append("Invalid name")

    if customer.age <= 18:
        errors.append("Age must be greater than 18")
    
    if len(customer.phones) == 1:
        errors.append("Customer must have at the least 1 phone")

    return len(errors) == 0, errors
```
If you write a test function, you need to take care about the validations order, 
and function is_valid will increase if you need a new validation.

Let's try other approuch.

```python
class ValidatorInterface:

    def validate(self, customer: Customer) -> list[str]:
        raise NotImplemented


class NameValidator(ValidatorInterface):
    def validate(self, customer: Customer) -> list[str]:
        errors = []

        if len(customer.name.split()) < 2:
            errors.append("Invalid name")

        return errors


class AgeValidator(ValidatorInterface):
    def validate(self, customer: Customer) -> list[str]:
        errors = []

        if customer.age <= 18:
            errors.append("Age must be greater than 18")

        return errors

class PhoneValidator(ValidatorInterface):
    def validate(self, customer: Customer) -> list[str]:
        errors = []

    if len(customer.phones) == 1:
        errors.append("Customer must have at the least 1 phone")

    return errors
```

Now we could write a unit test to each validator without thinking about the validation order.

Let's use it.

```python
class CustomerValidator:
    # We don't use DI here to simplify the solution
    def __init__():
        self._validators : list[ValidatorInterface] = []
        self._validators.append(NameValidator())
        self._validators.append(AgeValidator())
        self._validators.append(PhoneValidator())

    def validate(self, customer: Customer) -> Tuple[bool, list[str]]:
        errors = []
        for validator in self._validators:
            errors.append(validator.validate(customer))
        
        return errors

```

Now we have a extensive CustomerValidator, you can write a new validator without modify the method validate, if we use DI or a CustomerValidator "creator", we don't need to modify the class CustomerValidator.

    

<!-- https://github.com/peterjgrainger/dev-to-posts/blob/master/blog-posts/monorepo-keeping-your-history-in-order/monorepo-keeping-your-history-in-order.md -->