# Snake Game (OOP)

{% tabs %}
{% tab title="main.py" %}
```python
import time

screen = Screen()
screen.setup(width=600, height=600)
screen.bgcolor("black")
screen.title("Snake Game")
screen.tracer(0)

snake = Snake()
food = Food()
scoreboard = Scoreboard()

screen.listen()
screen.onkey(snake.up, "Up")
screen.onkey(snake.down, "Down")
screen.onkey(snake.left, "Left")
screen.onkey(snake.right, "Right")
scoreboard.start_scorebord()


game_is_on = True
while game_is_on:
    screen.update()
    time.sleep(0.1)
    snake.move()

    if snake.head.distance(food) < 15:
        scoreboard.increase_scoreboard()
        snake.extend()
        food.refresh()

    if snake.head.xcor() > 290 or snake.head.xcor() < -295 or snake.head.ycor() > 295 or snake.head.ycor() < -295:
        game_is_on = False
        scoreboard.game_over()

    for segment in snake.segments[1:]:
        if snake.head.distance(segment) < 10:
            game_is_on = False
            scoreboard.game_over()

screen.exitonclick()
```


{% endtab %}

{% tab title="food.py" %}
```python
from turtle import Turtle
import random

class Food(Turtle):

    def __init__(self):
        super().__init__()
        self.shape("circle")
        self.penup()
        self.shapesize(stretch_len=0.5, stretch_wid=0.5)
        self.color("red")
        self.speed(0)
        self.refresh()

    def refresh(self):
        random_x = random.randint(-28, 28) * 10
        random_y = random.randint(-28, 28) * 10
        self.goto(random_x, random_y)
```


{% endtab %}

{% tab title="scoreboard.py" %}
```python
from turtle import Turtle

ALINGMENT = 'center'
FONT = ('Times New Roman', 24, 'bold')


class Scoreboard(Turtle):

    def __init__(self):
        super().__init__()
        self.score = 0
        self.hideturtle()
        self.penup()
        self.color("White")
        self.goto(0,260)

    def start_scorebord(self):
        self.write(arg=f"Score: {self.score}", align=ALINGMENT, font=FONT)

    def increase_scoreboard(self):
        self.clear()
        self.score +=1
        self.start_scorebord()

    def game_over(self):
        self.goto(0,0)
        self.write(arg="GAME OVER", align=ALINGMENT, font=FONT)

```


{% endtab %}

{% tab title="snake.py" %}
```python
from turtle import Turtle
START_POSITION = [(0,0), (-20, 0), (40,0)]
MOVE_DISTANCE = 20
UP = 90
DOWN = 270
LEFT = 180
RIGHT = 0


class Snake:

    def __init__(self):
        self.segments = []
        self.create_snake()
        self.head = self.segments[0]

    def create_snake(self):
        for position in START_POSITION:
            self.add_segment(position)

    def add_segment(self, position):
            new_segment = Turtle("square")
            new_segment.color("white")
            new_segment.penup()
            new_segment.goto(position)
            self.segments.append(new_segment)

    def extend(self):
        self.add_segment(self.segments[-1].position())

    def move(self):
        for seg_num in range(len(self.segments) - 1, 0 , -1):
            new_x = self.segments[seg_num - 1].xcor()
            new_y = self.segments[seg_num - 1].ycor()
            self.segments[seg_num].goto(new_x, new_y)
        self.head.forward(MOVE_DISTANCE)

    def up(self):
        if self.head.heading() != DOWN:
            self.head.setheading(UP)

    def down(self):
        if self.head.heading() != UP:
            self.head.setheading(DOWN)

    def left(self):
        if self.head.heading() != RIGHT:
            self.head.setheading(LEFT)

    def right(self):
        if self.head.heading() != LEFT:
            self.head.setheading(RIGHT)
```


{% endtab %}
{% endtabs %}

