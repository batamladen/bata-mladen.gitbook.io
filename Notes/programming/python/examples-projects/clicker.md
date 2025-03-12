# Clicker

{% tabs %}
{% tab title="main.py" %}
```python
import tkinter as tk

clicks = 0  # Global variable to store the click count

def sayHi():
    global clicks
    clicks += 1
    label.config(text=f"Clicks: {clicks}")

# Window
win = tk.Tk()
win.geometry("200x100")
win.title("Clicker")

# Label
label = tk.Label(win, text="Clicks: 0", font=("Arial", 12))
label.pack(pady=10)

# Button
b = tk.Button(win, text="Click here", command=sayHi)
b.pack(side=tk.BOTTOM)

win.mainloop()


```


{% endtab %}
{% endtabs %}

