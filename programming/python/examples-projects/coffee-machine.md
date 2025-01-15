---
description: Order your coffee
---

# Coffee Machine

{% tabs %}
{% tab title="main.py" %}
```python
from menu import Menu, MenuItem
from coffee_maker import CoffeeMaker
from money_machine import MoneyMachine

#list of drinks
espresso = MenuItem("espresso", 50, 0, 18, 1.5)
latte = MenuItem("latte", 200, 150, 24, 2.5)
cappuccino = MenuItem("cappuccino", 250, 100, 24, 3.0)

#For ordering drinks and checking if drink is available
menu = Menu()

#Printing report of all resources, checking if resources are sufficien. making coffee
coffee_maker = CoffeeMaker()

#Printing report and taking payment
money_machine = MoneyMachine()

on = True
while on:
    choice = input(f"What would you like? Choose between espresso, latte or cappuccino: ")
    if choice == 'off':
        print("Have a nice day!")
        on = False
    elif choice == 'report':
        coffee_maker.report()
        money_machine.report()
    else:
        coffee_choice = menu.find_drink(choice)
        if coffee_maker.is_resource_sufficient(espresso) and money_machine.make_payment(espresso.cost):
            coffee_maker.make_coffee(coffee_choice)


```


{% endtab %}

{% tab title="coffee_maker.py" %}
```python
class CoffeeMaker:
    """Models the machine that makes the coffee"""
    def __init__(self):
        self.resources = {
            "water": 300,
            "milk": 200,
            "coffee": 100
        }

    def report(self):
        """Prints a report of all resources"""
        print(f"Water: {self.resources['water']}ml")
        print(f"Milk: {self.resources['milk']}ml")
        print(f"Coffee: {self.resources['coffee']}g")

    def is_resource_sufficient(self, drink):
        """Returns True when order can be made, False if ingredients are insufficient"""
        can_make = True
        for item in drink.ingredients:
            if drink.ingredients[item] > self.resources[item]:
                print(f"Sorry there is not enought {item}.")
                can_make = False
        return can_make

    def make_coffee(self,order):
        """Deducts the required ingredients from the resources"""
        for item in order.ingredients:
            self.resources[item] -= order.ingredients[item]
        print(f"Here is your {order.name} ☕️. Enjoy! ")
```


{% endtab %}

{% tab title="menu.py" %}
```python
class MenuItem:
    """Models each Menu Item."""
    def __init__(self, name, water, milk, coffee, cost):
        self.name = name
        self.cost = cost
        self.ingredients = {
            "water": water,
            "milk": milk,
            "coffee": coffee
        }

class Menu:
    """Models the Menu with drinks."""
    def __init__(self):
        self.menu = [
            MenuItem(name="latte", water=200, milk=150, coffee=24, cost=2.5),
            MenuItem(name="espresso", water=50, milk=0, coffee=18, cost=1.5),
            MenuItem(name="cappuccino", water=250, milk=50, coffee=24, cost=3),
        ]

    def get_items(self):
        """Returns all the names of the available menu items"""
        options = ""
        for item in self.menu:
            options += f"{item.name}/"
        return options

    def find_drink(self, order_name):
        """Searches the menu for a particular drink by name. Returns that item if it exists, othervise None"""
        for item in self.menu:
            if item.name == order_name:
                return item
        print("Sorry that item is not available")
```


{% endtab %}

{% tab title="money_machine.py" %}
```python
class MoneyMachine:

    CURRENCY = "$"

    COIN_VALUES = {
        "quarters": 0.25,
        "dimes": 0.10,
        "nickles": 0.05,
        "pennies": 0.01
    }

    def __init__(self):
        self.profit = 0
        self.money_recived = 0

    def report(self):
        """prints the current profit"""
        print(f"Money: {self.CURRENCY}{self.profit}")

    def process_coins(self):
        """Returns the total calculated from coins inserted"""
        print("Please insert coins.")
        for coin in self.COIN_VALUES:
            self.money_recived += int(input(f"How many {coin}?: ")) * self.COIN_VALUES[coin]
        return self.money_recived

    def make_payment(self,cost):
        """Returns True when payment is accepted, or False if insufficient"""
        self.process_coins()
        if self.money_recived > cost:
            change = round(self.money_recived - cost, 2)
            print(f"Here is {self.CURRENCY}{change} in change.")
            self.profit += cost
            self.money_recived = 0
            return True
        else:
            print("Sorry that's not enough money. Money refunded.")
            self.money_received = 0
            return False
```


{% endtab %}
{% endtabs %}

