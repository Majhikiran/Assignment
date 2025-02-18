# Assignment 1
import time
from django.db import models
from django.db.models.signals import post_save
from django.dispatch import receiver

class Book(models.Model):
    title = models.CharField(max_length=255)

# Signal function
@receiver(post_save, sender=Book)
def book_saved_signal(sender, instance, **kwargs):
    print("Signal started execution")
    time.sleep(2)  # Simulate a delay
    print("Signal finished execution")

# Testing
if __name__ == "__main__":
    print("Before saving book")
    book = Book(title="Django Signals")
    book.save()
    print("After saving book")

class Rectangle:
    def __init__(self, length: int, width: int):
        self.length = length
        self.width = width

    def __iter__(self):
        yield {'length': self.length}
        yield {'width': self.width}

# Example usage
rectangle = Rectangle(10, 5)
for attr in rectangle:
    print(attr)

    Assignment 2

    import threading
from django.db import models
from django.db.models.signals import post_save
from django.dispatch import receiver

class Book(models.Model):
    title = models.CharField(max_length=255)

@receiver(post_save, sender=Book)
def book_saved_signal(sender, instance, **kwargs):
    print(f"Signal running in thread: {threading.get_ident()}")

# Testing
if __name__ == "__main__":
    print(f"Main thread: {threading.get_ident()}")
    book = Book(title="Django Signals Threading")
    book.save()

class Rectangle:
    def __init__(self, length: int, width: int):
        self.length = length
        self.width = width

    def __iter__(self):
        yield {'length': self.length}
        yield {'width': self.width}

# Example usage
rectangle = Rectangle(10, 5)
for attr in rectangle:
    print(attr)

Assignment 3

from django.db import models, transaction
from django.db.models.signals import post_save
from django.dispatch import receiver

class Author(models.Model):
    name = models.CharField(max_length=255)

class Log(models.Model):
    message = models.CharField(max_length=255)

@receiver(post_save, sender=Author)
def author_saved_signal(sender, instance, **kwargs):
    print("Signal triggered - Creating Log entry")
    Log.objects.create(message=f"Author {instance.name} saved.")

# Testing
if __name__ == "__main__":
    try:
        with transaction.atomic():
            author = Author.objects.create(name="J.K. Rowling")
            print("Author saved")
            raise Exception("Forcing rollback")  # This should roll back everything

    except Exception as e:
        print(f"Exception caught: {e}")

    # Check if the Log entry was rolled back
    logs = Log.objects.all()
    print(f"Log entries count: {logs.count()}")

class Rectangle:
    def __init__(self, length: int, width: int):
        self.length = length
        self.width = width

    def __iter__(self):
        yield {'length': self.length}
        yield {'width': self.width}

# Example usage
rectangle = Rectangle(10, 5)
for attr in rectangle:
    print(attr)

