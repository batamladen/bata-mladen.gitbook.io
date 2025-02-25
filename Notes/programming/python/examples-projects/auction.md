# Auction

{% tabs %}
{% tab title="main" %}
```python
from art import ITEMS
import random


item = random.choice(ITEMS)
print(item)


play = True
bidders = {}
while play:

    name = input("What is your name?: ")
    bid_amount = input("What is your bid?: ")
    more_bidders = input("Are there any more bidders?: ")
    bidders[name] = bid_amount


    if more_bidders == 'no':
        play = False
    else:
        continue

highest_bidder = max(bidders, key=bidders.get)
highest_bid = bidders[highest_bidder]
print(f"\nThe highest bidder is {highest_bidder} with a bid of ${highest_bid}.")



```
{% endtab %}

{% tab title="art" %}
```python
ITEMS = [
    '''
                                                        
                                                    
                                                    
                       :=**#######****++=--:.       
                   :*##+-----====----==+%%%%%#.     
               .-*#%%%%%%%%%%@@@%%%%%%@@@@%*--#+    
         ..:----::::::::.......:.::::::--------:.   
    ::----:::-======--------============-:.::--::.  
   ...:--#%%%%*.----------------------.=#%%##=.::.  
  :--::=%***=*##:::::::::::::::::::::.*#**=+*##.:.  
  =%#::%#+=::-*%:.:.................:-%**:::=#@-:.  
   .:::=#***=*#+.                     =#**+=*#:     
         :*#*+.                        .+#**=       
                                                    ''',
    '''
                                                                                      
                                                            
       !
       !
       ^
      / \
     /___\
    |=   =|
    |     |
    |     |
    |     |
    |     |
    |     |
    |     |
    |     |
    |     |
    |     |
   /|##!##|\
  / |##!##| \
 /  |##!##|  \
|  / ^ | ^ \  |
| /  ( | )  \ |
|/   ( | )   \|
    ((   ))
   ((  :  ))
   ((  :  ))
    ((   ))
     (( ))
      ( )
       .           
                                                            ''',
    '''
                 _
           /(_))
         _/   /
        //   /
       //   /
       /\__/
       \O_/=-.
   _  / || \  ^o
   \\/ ()_) \.
    ^^ <__> \()
      //||\\
     //_||_\\  ds
    // \||/ \\
   //   ||   \\
  \/    |/    \/
  /     |      \
 /      |       \
 '''
]
```
{% endtab %}
{% endtabs %}
