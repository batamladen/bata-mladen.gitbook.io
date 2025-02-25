---
description: Turtles racing. Bet on 'em
---

# Turtle Race

{% tabs %}
{% tab title="main.py" %}
```python
import turtle
import random

wn = turtle.Screen()
wn.setup(width=800, height=600)

COLORS = ["blue", "red", "yellow", "green", "black", "orange", "pink", "purple"]
SPEED = [0, 1, 3, 6, 10]
NUM_TURTLES = 8
user_bet = wn.textinput(title="Make your bet", prompt="Which turtle will win the race? Enter a color: ")

def create_turtle(color, y_position):
    t = turtle.Turtle()
    t.speed(0)
    t.hideturtle()
    t.shape("turtle")
    t.color(color)
    t.penup()
    t.goto(-350, y_position)
    t.showturtle()
    return t

# Initialize turtles
turtles = []
start_y = -250
for i in range(NUM_TURTLES):
    color_picker = COLORS[i]
    speed_picker = random.choice(SPEED)
    t = create_turtle(color_picker, start_y + i * 50)
    t.speed(speed_picker)
    turtles.append(t)

# Start the race
race_on = True
while race_on:
    for t in turtles:
        t.forward(random.randint(1, 20))

        if t.xcor() >= 350:
            race_on = False
            winning_turtle = t.color()[0]
            if winning_turtle == user_bet:
                print(f"You've won! The {winning_turtle} turtle is the winner!")
            else:
                print(f"You've lost! The {winning_turtle} turtle is the winner!")
            break

wn.exitonclick()

```


{% endtab %}

{% tab title="racers.py" %}
```python
import turtle
import random

COLORS = ["blue", "red", "yellow", "green", "black", "orange", "pink"]
SPEED = [0, 1, 3, 6, 10]
random_color = random.choice(COLORS)
random_speed = random.choice(SPEED)

def turtles(t, number):

    for racer in range(number):
        y_axis = -250
        t.shape("turtle")
        t.up()
        t.color(random_color)
        t.goto(-250, y_axis)
        t.speed(random_speed)
        t.forward(1000)
        for range in number:
            y_axis += 50

```


{% endtab %}
{% endtabs %}

