---
description: Calculates how much money should everyone give + tip for waiter
---

# Tip Calculator

{% tabs %}
{% tab title="main.py" %}
```python
print("Welcome to the tip calculator")

total_bill = float(input("What was the total bill?: "))
tip_percentage = int(input("What percentage tip would you like to give?: "))
amount_of_people = int(input("How many people to split the bill?: "))

result = (total_bill + (total_bill * tip_percentage/100)) / amount_of_people

print(result)
```
{% endtab %}

{% tab title="Second Tab" %}

{% endtab %}
{% endtabs %}

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FUwIKQPujWqWSVFxQaX98%2Fuploads%2FbTqiiDRWLmv3fJxQh7Y5%2Fday2.mp4?alt=media&token=240e4d00-3ab5-4ffd-8428-7a452df79aca" %}
