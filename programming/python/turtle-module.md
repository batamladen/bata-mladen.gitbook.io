# Turtle Module



{% tabs %}
{% tab title="shape_and_color.py" %}
```python
#importing
import turtle

#create the turtle object
mladen = turtle.Turtle()

#change turtle shape (circle, triangle, turtle...)
mladen.shape("turtle")

#change lone and turtle color
mladen.color("blue") #changes line and turtle to blue
mladen.color("blue", "black") #blue line, black turtle

turtle.colormode(255) #change to use RGB colors
mladen.color(120,50,20)
```
{% endtab %}

{% tab title="screen.py" %}
```python
#import turtle

#create screen object
wn = turtle.Screen()

#change background
wn.bgcolor("black")

turtle.colormode(255) #change mode so we can use RGB
wn.bgcolor(200,40,20) #RGB color code

#change background as picture(image needs to be GIF extension, and needs to be in the same package as the py. file)
wn.bgpic("python.gif")

#change title
wn.title("Mladen The Turtle")
```
{% endtab %}

{% tab title="movement.py" %}
```python
#the turtle starts in HOME position whitch is 0,0. It also starts facing the east.

import turtle
mladen = turtle.Turtle

#change start position
mladen.setheading(90) #change the angle of the start position (90 - north | 180 - west | 270 - south | 360 - east)

#movement
#depending on the start position, when we use forward command, it will move in that direction

#forward (go in the direction the turtle is headed)
mladen.forward(100)
mladen.fd(100) #shorter

#backward(move in the opposite direction of the turtle position)
mladen.backward(100)
mladen.bk(100)
```
{% endtab %}

{% tab title="circle.py" %}
```python
import turtle

mladen = turtle.Turtle()

#dra a circle, add radius as an argument
mladen.circle(50)
mladen.circle(-50)

#clear picture and get turtle to home position
mladen.reset()

#semi-circle, redius + angle as arguemnts
mladen.circle(100,180)
mladen.reset()

#draw other shapes with "steps"
mladen.circle(100, steps=6)
```


{% endtab %}

{% tab title="goto, up_down.py" %}
```python
import turtle

mladen = turtle.Turtle()

#start drawing from a different position (put the turtles pen up lol)
mladen.goto(0, -100)

#get turtle pen up, away from the paper
mladen.up()
mladen.goto(0,200)

#get turtle pen down, back on the paper
mladen.down()
mladen.circle(-100, steps=6)

```


{% endtab %}

{% tab title="reset, undo, pause.py" %}
```python
import turtle

mladen = turtle. Turtle()

#undo a drawing process
mladen.undo()

#reset the screen ad get turtle to home position
mladen.reset()

#pause the screen when draing is done
turtle.done()
```


{% endtab %}

{% tab title="square, fillcolor, speed.py" %}
```python
import turtle

mladen = turtle.Turtle()

#draw a square manually
mladen.forward(100)
mladen.left(90)
mladen.forward(100)
mladen.left(90)
mladen.forward(100)
mladen.left(90)
mladen.forward(100)
mladen.left(90)

#draw a square woth a for statement and range() command
for i in range(4):
    mladen.forward(100)
    mladen.left(90)

#hide the turtle
mladen.hideturtle() #if we want to hide the turtle from begginging, use command before starting of the drawing
mladen.showturtle()

#chage speed of the turtle (0-fastest, 10 fast, 6 normal, 3 slow, 1 slowest)
mladen.speed(6)

#fill collor
mladen.begin_fill() #at the start of drawing a shape
mladen.end_fill() #at the end of drawing a shape
mladen.fillcolor("red")

mladen.begin_fill()
mladen.fillcolor("red")
for i in range(4):
    mladen.backward(100)
    mladen.left(90)
mladen.end_fill()




```


{% endtab %}
{% endtabs %}
