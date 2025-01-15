---
description: guess who has more insta followers
---

# Higher Lower

{% tabs %}
{% tab title="main.py" %}
```python
from replit import clear
import time




def play_game():
    print(art.logo)
    score = 0
    game_continue = True

    first_person = random.choice(list(people.items()))
    second_person = random.choice(list(people.items()))

    while game_continue:
        while first_person == second_person:
            second_person = random.choice(list(people.items()))

        clear()
        print(f"Score: {score}")
        time.sleep(2)
        print(f"\nCompare A: {first_person[0]}")
        print(art.vs)
        print(f"Compare B: {second_person[0]}")

        answer = input("Who do you think has more followers (A / B): ").upper()

        first_person_followers = first_person[1]
        second_person_followers = second_person[1]

        if (first_person_followers > second_person_followers and answer == 'A') or (
                second_person_followers > first_person_followers and answer == 'B'):
            score += 1
            print("Correct!")
            time.sleep(2)
            first_person = second_person
            second_person = random.choice(list(people.items()))
        else:
            game_continue = False
            print("Incorrect! Your score was:", score)
            time.sleep(2)


while True:
    play_game()
    play_again = input("Play again? (yes / no): ").lower()

    if play_again == 'no':
        break

```


{% endtab %}

{% tab title="art.py" %}
```python
logo = """    __  ___       __             
   / / / (_)___ _/ /_  ___  _____
  / /_/ / / __ `/ __ \/ _ \/ ___/
 / __  / / /_/ / / / /  __/ /    
/_/ ///_/\__, /_/ /_/\___/_/     
   / /  /____/_      _____  _____
  / /   / __ \ | /| / / _ \/ ___/
 / /___/ /_/ / |/ |/ /  __/ /    
/_____/\____/|__/|__/\___/_/     
"""

vs = """ _    __    
| |  / /____
| | / / ___/
| |/ (__  ) 
|___/____(_)
"""
```


{% endtab %}

{% tab title="game_data.py" %}
```python
people = {
    "Neymar, football player, from Brasil" : 223000000,
    "Ronaldo, football player, from Porugal" : 635000000,
    "Messi, football player, from Argentina" : 503000000,
    "Yamal, football player, from Spain" : 16800000,
    "Mbappe, football player, from France" : 120000000,
    "Erling Haaland, football player, from Norway" : 38500000,
    "Jude Belingham, football player, from England" : 35100000
}
```


{% endtab %}
{% endtabs %}



