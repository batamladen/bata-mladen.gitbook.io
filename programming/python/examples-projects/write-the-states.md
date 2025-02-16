# Write The States

{% tabs %}
{% tab title="main.py" %}
```python
import turtle
import pandas

data = pandas.read_csv("50_states.csv")
screen = turtle.Screen()
screen.title("U.S. States Game")
image = "blank_states_img.gif"
screen.addshape(image)
turtle.shape(image)

t = turtle.Turtle()
t.hideturtle()
t.penup()

guessed_states = []
states_list = data.state.to_list()

while len(guessed_states) < 50:
    answer = screen.textinput(title=f"{len(guessed_states)}/50 States Guessed", prompt="Name a State").title()

    if answer == "Exit":
        missing_states = []
        for state in states_list:
            if state not in guessed_states:
                missing_states.append(state)
        new_data = pandas.DataFrame(missing_states)
        new_data.to_csv("states_to_learn.csv")
        break

    if answer in states_list:
        if answer not in guessed_states:
            guessed_states.append(answer)
            check_answer = data[data.state == answer]
            x_axis, y_axis = int(check_answer.x), int(check_answer.y)
            t.goto(x_axis, y_axis)
            t.write(f"{answer}")
```


{% endtab %}

{% tab title="50_states.csv" %}
```csv
state,x,y
Alabama,139,-77
Alaska,-204,-170
Arizona,-203,-40
Arkansas,57,-53
California,-297,13
Colorado,-112,20
Connecticut,297,96
Delaware,275,42
Florida,220,-145
Georgia,182,-75
Hawaii,-317,-143
Idaho,-216,122
Illinois,95,37
Indiana,133,39
Iowa,38,65
Kansas,-17,5
Kentucky,149,1
Louisiana,59,-114
Maine,319,164
Maryland,288,27
Massachusetts,312,112
Michigan,148,101
Minnesota,23,135
Mississippi,94,-78
Missouri,49,6
Montana,-141,150
Nebraska,-61,66
Nevada,-257,56
New Hampshire,302,127
New Jersey,282,65
New Mexico,-128,-43
New York,236,104
North Carolina,239,-22
North Dakota,-44,158
Ohio,176,52
Oklahoma,-8,-41
Oregon,-278,138
Pennsylvania,238,72
Rhode Island,318,94
South Carolina,218,-51
South Dakota,-44,109
Tennessee,131,-34
Texas,-38,-106
Utah,-189,34
Vermont,282,154
Virginia,234,12
Washington,-257,193
West Virginia,200,20
Wisconsin,83,113
Wyoming,-134,90
```


{% endtab %}

{% tab title="blank_states_img.gif" %}
<figure><img src="../../../.gitbook/assets/image (8) (1).png" alt=""><figcaption></figcaption></figure>


{% endtab %}
{% endtabs %}

