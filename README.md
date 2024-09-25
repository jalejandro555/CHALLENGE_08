# CHALLENGE_08
I am presenting this challenge, for the next reasons, first, i forgot at the beggining of this year to upload my repository of the second challenge on the excel and, at the other hand i see this as a good opportunity to improve my skills. 
## WHAT WILL I DO?
For reference this is how my resturant code looked like 
```python
# Define the base class for menu items
class MenuItem:
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def calculate_total_price(self, quantity):
        return self.price * quantity

# Define subclasses for different types of menu items

class FoodItem:
    def __init__(self, name, price, category):
        self.name = name
        self.price = price
        self.category = category

class Starters(FoodItem):
    def __init__(self, name, price):
        super().__init__(name, price, "Starters")

class Soupes(FoodItem):
    def __init__(self, name, price):
        super().__init__(name, price, "Soupes")

class MainCourse(FoodItem):
    def __init__(self, name, price):
        super().__init__(name, price, "Main Course")

class Drinks(FoodItem):
    def __init__(self, name, price):
        super().__init__(name, price, "Drinks")

class Dessert(FoodItem):
    def __init__(self, name, price):
        super().__init__(name, price, "Dessert")

class OrderIterator:
    def __init__(self, items):
        self._items = items
        self._index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self._index < len(self._items):
            item = self._items[self._index]
            self._index += 1
            return item
        else:
            raise StopIteration

# Define the Order class
class Order:
    def __init__(self):
        self.items = []

    def add_item(self, item, quantity):
        self.items.append((item, quantity))

    def calculate_total_price(self):
        total = 0
        for item, quantity in self.items:
            total += item.price * quantity
        return total
    
    def ask_for_discount(self):
        categories = ["programmer", "mentally_ill", "student", "senior_citizen", "veteran", "unemployed", "homeless", "refugee", "disabled"]
        print("Please enter your population category. if you are not in any of these categories, please press enter")
        print("Available categories are:")
        for category in categories:
            print(category)
        user_category = input()
        if user_category in categories:
            return 10
        else:
            print("Sorry, you are not eligible for a discount.")
            return 0

    def apply_discount(self):
        discount = self.ask_for_discount()
        total_bill = self.calculate_total_price()
        return total_bill - (total_bill * discount / 100)
    
def __iter__(self):
        return OrderIterator(self.items)

# Create instances of menu items
menu_items = [
    Starters("Diazepam, ONLY_FOR_SALE_WITH_PRESCRIPTION", 1.99,),
    Starters("Xanax, ONLY_FOR_SALE_WITH_PRESCRIPTION", 0.99),
    Starters("Propanonol, ONLY_FOR_SALE_WITH_PRESCRIPTION", 0.99), 
    Soupes("Programmer's tears with Mexican tortilla", 5.99),
    Soupes("Spinach with tomato", 3.99),
    Soupes("Lentil with bacon", 4.99),
    MainCourse("Korean boy beef burger", 201.99),
    MainCourse("Chicken fetus crepe", 122.99),
    MainCourse("Vegan salad with tuna", 7.99),
    MainCourse("Turkey pizza", 100),
    MainCourse("Venezuelan Pasta", 0.99),
    MainCourse("Fish and Chips with cornflakes", 12.99),
    Drinks("Fanta", 1.99),
    Drinks("Taylor Swift's sweat", 900.99),
    Drinks("Coca Cola", 1.99),
    Drinks("Pepsi", 1.99),
    Drinks("Water from arctic glaciers", 300.99),
    Dessert("Ice cream with an american sausage", 2.99),
    Dessert("Fruit salad without fruit or transgenic fruit", 2.99),
    Dessert("Fruit salad with organic fruit", 200.99),
    Dessert("Snow white apple, REQUIRES_SIGNED_CONSENT", 6.99),
]
# User interaction functions
def show_menu():
    print("Menu:")
    for index, item in enumerate(menu_items):
        print(f"{index + 1}. {item.name} - ${item.price} - Category: {item.category}")
def user_order():
    show_menu()
    order = Order()
    while True:
        choice = input("Enter the menu item number (or 'done' to finish): ")
        if choice.lower() == 'done':
            break
        quantity = int(input("Enter the quantity: "))
        order.add_item(menu_items[int(choice) - 1], quantity)
    return order

# Main program
if __name__ == "__main__":
    print("WELCOME TO THE PYTHON PSYCHIATRIC RESTAURANT!")
    customer_order = user_order()
    print(f"Your total bill is: ${customer_order.calculate_total_price():.2f}")
    print(f"Your total bill after discount is: ${customer_order.apply_discount():.2f}")
    print("Thank you for dining with us!")

```
In the previous challenge the implementations that were add to the code allowed us to loop through the items in an Order instance using a for loop, accessing each item's attributes as needed.

This thanks to 

OrderIterator Class: init Method: Initializes the iterator with the list of items and sets the starting index to 0.

iter Method: Returns the iterator object itself.

next Method: Returns the next item in the list if available, otherwise raises StopIteration.

Order Class: iter Method: Returns an instance of OrderIterator initialized with the list of items in the order.

## Now 
What you will see in this challenge is the implemetation of decorators.
Decorators in Python are commonly used to modify or extend the behavior of functions or methods. In this case, we can consider using decorators for:

1. Validate input parameters in __init__ methods.
2. Log class instance creation.

HERE IS THE CODE 

```python
# Define the base class for menu items
def validate_input(func):
    def wrapper(self, name, price, *args, **kwargs):
        if not isinstance(price, (int, float)) or price <= 0:
            raise ValueError("Price must be a positive number")
        if not isinstance(name, str) or not name:
            raise ValueError("Name must be a non-empty string")
        return func(self, name, price, *args, **kwargs)
    return wrapper

def log_creation(func):
    def wrapper(self, *args, **kwargs):
        instance = func(self, *args, **kwargs)
        print(f"Created instance of {self.__class__.__name__} with name: {self.name} and price: {self.price}")
        return instance
    return wrapper

class MenuItem:
    @validate_input
    @log_creation
    def __init__(self, name, price):
        self.name = name
        self.price = price

    def calculate_total_price(self, quantity):
        return self.price * quantity

# Define subclasses for different types of menu items

class FoodItem(MenuItem):
    @validate_input
    @log_creation
    def __init__(self, name, price, category):
        super().__init__(name, price)
        self.category = category

class Starters(FoodItem):
    @validate_input
    @log_creation
    def __init__(self, name, price):
        super().__init__(name, price, "Starters")

class Soupes(FoodItem):
    @validate_input
    @log_creation
    def __init__(self, name, price):
        super().__init__(name, price, "Soupes")

class MainCourse(FoodItem):
    @validate_input
    @log_creation
    def __init__(self, name, price):
        super().__init__(name, price, "Main Course")

class Drinks(FoodItem):
    @validate_input
    @log_creation
    def __init__(self, name, price):
        super().__init__(name, price, "Drinks")

class Dessert(FoodItem):
    @validate_input
    @log_creation
    def __init__(self, name, price):
        super().__init__(name, price, "Dessert")

# Menu items
menu_items = [
    MainCourse("Venezuelan Pasta", 0.99),
    MainCourse("Fish and Chips with cornflakes", 12.99),
    Drinks("Fanta", 1.99),
    Drinks("Taylor Swift's sweat", 900.99),
    Drinks("Coca Cola", 1.99),
    Drinks("Pepsi", 1.99),
    Drinks("Water from arctic glaciers", 300.99),
    Dessert("Ice cream with an american sausage", 2.99),
    Dessert("Fruit salad without fruit or transgenic fruit", 2.99),
    Dessert("Fruit salad with organic fruit", 200.99),
    Dessert("Snow white apple, REQUIRES_SIGNED_CONSENT", 6.99),
]

# User interaction functions
def show_menu():
    print("Menu:")
    for index, item in enumerate(menu_items):
        print(f"{index + 1}. {item.name} - ${item.price} - Category: {item.category}")

class Order:
    def __init__(self):
        self.items = []

    def add_item(self, item, quantity):
        self.items.append((item, quantity))

    def calculate_total_price(self):
        return sum(item.calculate_total_price(quantity) for item, quantity in self.items)

    def apply_discount(self):
        total = self.calculate_total_price()
        discount = 0.1 * total if total > 100 else 0
        return total - discount

def user_order():
    show_menu()
    order = Order()
    while True:
        choice = input("Enter the menu item number (or 'done' to finish): ")
        if choice.lower() == 'done':
            break
        quantity = int(input("Enter the quantity: "))
        order.add_item(menu_items[int(choice) - 1], quantity)
    return order

# Main program
if __name__ == "__main__":
    print("WELCOME TO THE PYTHON PSYCHIATRIC RESTAURANT!")
    customer_order = user_order()
    print(f"Your total bill is: ${customer_order.calculate_total_price():.2f}")
    print(f"Your total bill after discount is: ${customer_order.apply_discount():.2f}")
    print("Thank you for dining with us!")
```

