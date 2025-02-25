---
description: A rulette game without the green zero.
---

# Simple Rulette

{% tabs %}
{% tab title="main.py" %}
```python
import csv
import random

start_money = int(input("How much money do you have?: $"))
numbers_bet = []
colors_bet = []
number_money_in_bet = []
color_money_in_bet = []

def place_bet():
    global start_money
    global numbers_bet
    global colors_bet
    global number_money_in_bet

    print("\nPlace your bet:")
    print("1. Bet on numbers")
    print("2. Bet on red")
    print("3. Bet on black")

    bet_option = int(input("Choose an option (1-3): "))

    if bet_option == 1:
        number_bet = int(input("Enter the number you want to bet on (1-36): "))
        numbers_bet.append(number_bet)
        number_bet_amount = int(input("Enter the amount you want to bet: $"))
        number_money_in_bet.append(number_bet_amount)
        sum(number_money_in_bet)

        if start_money - number_bet_amount <0:
            print("Not enough money")

        elif 1 <= number_bet <= 36:
            start_money -= number_bet_amount
            print("--------------------")
            print(f"You placed a bet of ${number_bet_amount} on number {number_bet}.")
            print(f"You have ${start_money} left")
        else:
            print("Invalid number. Please choose a number between 0 and 36.")

    elif bet_option == 2 or bet_option == 3:
        color_bet = "red" if bet_option == 2 else "black"
        colors_bet.append(color_bet)
        color_bet_amount = int(input("Enter the amount you want to bet: $"))
        color_money_in_bet.append(color_bet_amount)

        if start_money - color_bet_amount <0:
            print("Not enough money")
        else:
            start_money -= color_bet_amount
            print("--------------------")
            print(f"You placed a bet of ${color_bet_amount} on {color_bet}.")
            print(f"You have ${start_money} left")

    else:
        print("Invalid option. Please choose a valid option (1-3).")


def spin_wheel():
    global start_money
    global numbers_bet
    global colors_bet
    global number_money_in_bet
    global color_money_in_bet

    print("\nSpinning the wheel, no more bets.")

    with open('Board.csv', 'r') as file:
        reader = csv.DictReader(file)
        rows = list(reader)

    random_row = random.choice(rows)
    print("The ball landed on:")
    print(random_row)

    if int(random_row["number"]) in numbers_bet:
        start_money = start_money + (number_money_in_bet[numbers_bet.index(int(random_row["number"]))] * 35) #i used chatgpt for this line,
        print(f"Congratulations! Your {random_row['number']} bet wins!")                                     #i did not know how to multiply the numbers we bet on and 35, because the numbers are in a list
    elif random_row["color"] in colors_bet:
        start_money = start_money + (color_money_in_bet[colors_bet.index(random_row["color"])] * 2)          #also for this one line, i have no clue what [colors_bet.index(random_row["color"])] does but the code works :)
        print(f"Congratulations! Your {random_row['color']} bet wins!")
    else:
        print("Unlucky")

    print(f"You have ${start_money} left")
    numbers_bet.clear()
    colors_bet.clear()
    number_money_in_bet.clear()
    color_money_in_bet.clear()


def leave_table():
    print("\n===================")
    print(f"You are leaving the table with ${start_money}, \nsee you next time!")
    exit()


print("Welcome to the table!")
while True:
    user_input = input("\noptions: \n1. place bet "
                                  "\n2. spin wheel "
                                  "\n3. leave the table\n")

    if user_input == "1":
        place_bet()
    elif user_input == "2":
        spin_wheel()
    elif user_input == "3":
        leave_table()
    else:
        print("Not a valid option")
```
{% endtab %}

{% tab title="Board.csv" %}
```csv
number,color
1,red
2,black
3,red
4,black
5,red
6,black
7,red
8,black
9,red
10,black
11,black
12,red
13,black
14,red
15,black
16,red
17,black
18,red
19,red
20,black
21,red
22,black
23,red
24,black
25,red
26,black
27,red
28,black
29,black
30,red
31,black
32,red
33,black
34,red
35,black
36,red
```
{% endtab %}
{% endtabs %}

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FUwIKQPujWqWSVFxQaX98%2Fuploads%2FcVzhFCXJ8put0tOWjYU2%2F0511%20(1).mp4?alt=media&token=553437b2-a3a8-4143-b149-b3d0232c7939" %}
