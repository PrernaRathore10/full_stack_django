# Django Models - Comprehensive Notes

## Introduction to Models
- **Models** are the source of truth for data in a Django app.
- They define the structure of your database, the data types, and the relationships between data.

### Key Points
- Each model is a Python class that subclasses `django.db.models.Model`.
- Each attribute of the model represents a database field.

---

## Basic Model Structure
```python
from django.db import models

class ExampleModel(models.Model):
    field_name = models.FieldType(parameters)
```
**basic libraries to install** 
```python
from django.db import models
from django.utils import timezone
from django.contrib.auth.models import User
```
---

## Common Field Types

| Field Type         | Description                                    | Parameters                      |
|--------------------|------------------------------------------------|---------------------------------|
| `CharField`        | String (requires `max_length`).               | `max_length=255`               |
| `TextField`        | Large text.                                   | `blank=True, default=''`       |
| `IntegerField`     | Integer value.                                | `default=0`                    |
| `FloatField`       | Decimal value.                                | `default=0.0`                  |
| `BooleanField`     | Boolean (True/False).                         | `default=False`                |
| `DateField`        | Date value.                                   | `auto_now`, `auto_now_add`     |
| `DateTimeField`    | Date and time.                                | `default=timezone.now`         |
| `ImageField`       | Upload image files.                           | `upload_to='path/'`            |
| `FileField`        | Upload general files.                         | `upload_to='path/'`            |
| `ForeignKey`       | Many-to-one relationship.                     | `to`, `on_delete`              |
| `ManyToManyField`  | Many-to-many relationship.                    | `to`                           |
| `OneToOneField`    | One-to-one relationship.                      | `to`, `on_delete`              |
| `SlugField`        | URL-friendly strings.                         | `unique=True`                  |
| `EmailField`       | Email validation field.                       | `max_length=255`               |

---

## Field Parameters

| Parameter             | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| `null`                | Whether the database column can store NULL. Default: `False`.              |
| `blank`               | Whether the field is allowed to be empty in forms. Default: `False`.        |
| `default`             | Default value for the field.                                               |
| `unique`              | Ensures all values are unique.                                             |
| `choices`             | Provides dropdown options in forms.                                         |
| `verbose_name`        | Human-readable name for the field.                                          |
| `help_text`           | Text displayed as help for forms.                                           |
| `db_index`            | Creates an index for faster lookups.                                        |
| `primary_key`         | Marks the field as the primary key. Defaults to `id` if not specified.      |
| `editable`            | Excludes the field from the admin or form. Default: `True`.                 |
| `auto_now`            | Updates the field with the current time on every save (for Date/Time fields).|
| `auto_now_add`        | Sets the field to the current time when an object is created.               |
| `upload_to`           | Specifies the directory for uploaded files (for `FileField`/`ImageField`).  |

---

## Meta Class
- The `Meta` class is used to define model-level configurations.

```python
class ExampleModel(models.Model):
    name = models.CharField(max_length=100)

    class Meta:
        ordering = ['name']  # Order by name
        verbose_name = 'Example'
        verbose_name_plural = 'Examples'
        db_table = 'example_table'  # Custom table name
```

| Option              | Description                                                      |
|---------------------|------------------------------------------------------------------|
| `ordering`          | Default ordering for the model.                                  |
| `verbose_name`      | Singular human-readable name.                                    |
| `verbose_name_plural`| Plural human-readable name.                                     |
| `db_table`          | Custom database table name.                                      |
| `unique_together`   | Enforces unique constraints on multiple fields.                 |
| `indexes`           | Defines custom database indexes.                                |

---

## Relationships

### One-to-Many
```python
class Parent(models.Model):
    name = models.CharField(max_length=50)

class Child(models.Model):
    parent = models.ForeignKey(Parent, on_delete=models.CASCADE)
    name = models.CharField(max_length=50)
```

### Many-to-Many
```python
class Author(models.Model):
    name = models.CharField(max_length=50)

class Book(models.Model):
    authors = models.ManyToManyField(Author)
    title = models.CharField(max_length=100)
```

### One-to-One
```python
class UserProfile(models.Model):
    user = models.OneToOneField(User, on_delete=models.CASCADE)
    bio = models.TextField()
```
---
#### Why we use `on_delete=models.CASCADE`?
 - on_delete=models.CASCADE is used to automatically delete related objects when the referenced object is deleted. It ensures that if a parent object is removed, all its dependent child objects are also deleted from the database.
- Overusing it can lead to unintended data loss if parent records are deleted accidentally.

---

## Functions in Models

### Common Methods
| Method        | Description                                                      |
|---------------|------------------------------------------------------------------|
| `__str__()`   | Returns a string representation of the object.                   |
| `save()`      | Override to customize save behavior.                             |
| `delete()`    | Override to customize delete behavior.                           |
| `get_absolute_url()` | Returns the canonical URL for the object.                  |

Example:
```python
class ExampleModel(models.Model):
    name = models.CharField(max_length=100)

    def __str__(self):
        return self.name

    def get_absolute_url(self):
        return f"/example/{self.id}/"
```

---

## QuerySet API

- **Create:**
  ```python
  ExampleModel.objects.create(name="Example")
  ```
- **Retrieve All:**
  ```python
  ExampleModel.objects.all()
  ```
- **Filter:**
  ```python
  ExampleModel.objects.filter(name="Example")
  ```
- **Get:**
  ```python
  ExampleModel.objects.get(id=1)
  ```
- **Update:**
  ```python
  obj = ExampleModel.objects.get(id=1)
  obj.name = "Updated Name"
  obj.save()
  ```
- **Delete:**
  ```python
  obj = ExampleModel.objects.get(id=1)
  obj.delete()
  ```

---

## Migration Commands

| Command                  | Description                            |
|--------------------------|----------------------------------------|
| `makemigrations`         | Detects changes in models and prepares migrations. |
| `migrate`                | Applies migrations to the database.   |
| `sqlmigrate <name>`      | Displays raw SQL for a migration.      |
| `showmigrations`         | Lists migrations and their status.     |

---

## Best Practices

1. Use `__str__` to define a human-readable representation.
2. Use `related_name` in relationships for better reverse lookups.
3. Add constraints using `unique`, `unique_together`, or `validators`.
4. Always specify `on_delete` for `ForeignKey` and `OneToOneField`.
5. Use `Meta` for model-specific options.
6. Create migrations after modifying models and apply them consistently.

---
