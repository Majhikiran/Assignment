# Assignment
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
