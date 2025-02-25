# Supermarket

{% tabs %}
{% tab title="main.py" %}
```python
import csv

Chart = []
Price = []

def see_products():
    with open("products.csv", "r") as file:
        csv_reader = csv.reader(file)
        next(csv_reader)

        for line in csv_reader:
            print(line)

def add_to_chart():
    while True:
        user_product = input("Enter product: ")

        with open("products.csv", "r") as file:
            csv_reader = csv.reader(file)
            product_found = False

            for row in csv_reader:
                if row and len(row) >= 2 and user_product.lower() == row[1].lower():
                    quantity = int(input(f"How many {user_product}s do you want to add? "))
                    total_price = float(row[2].replace('$', '')) * quantity
                    Chart.append((user_product, quantity))
                    Price.append(total_price)
                    print(f"Successfully added {quantity} {user_product}s | Total Price: ${total_price:.2f}")
                    product_found = True
                    break

            if product_found:
                break
            else:
                print("Product not found")

def checkout():
    if not Chart:
        print("Your chart is empty. Add some products before checking out.")
        return
    total_price = sum(Price)

    print("\n===== Checkout =====")
    print("Your Chart:")
    for (product, quantity), price in zip(Chart, Price):
        print(f"{product} x{quantity}: ${price:.2f}")

    print("\nTotal Price: ${:.2f}".format(total_price))
    print("Thank you for shopping at Martin's supermarket!")
    exit()



print("\nWelcome to Mladen's supermarket \n---------------------------------")
print("To see available products write 'p' \n"
      "To add product to chart write 'add' \n"
      "To checkout write 'c'")

while True:
    user_input = input("What would you like to do? (p,add,c): ")

    if user_input.lower() == 'p':
        see_products()
    elif user_input.lower() == 'add':
        add_to_chart()
    elif user_input.lower() == 'c':
        checkout()

    else:
        print("Invalid input. Please try again.")

```
{% endtab %}

{% tab title="add_to_chart.py" %}
```python
import csv

Chart = []
user_product = input("Enter product: ")

with open("products.csv", "r") as file:
    csv_reader = csv.reader(file)

    for row in csv_reader:
        if row and len(row) >= 2 and user_product.lower() == row[1].lower():
            Chart.append(row)
            print("Successfully added product")
```
{% endtab %}

{% tab title="products.csv" %}
```csv
Category,Product,Price
Meat,Chicken,9$
Meat,Ham,14$
Meat,Salami,8.50$
Meat,Sausages,9.50$
Meat,Bacon,6$
Meat,Rib,13$
Bakery,Bread,1$
Bakery,Pizza,2$
Bakery,Salami Roll,0.60$
Bakery,Donut,1$
Bakery,Croasan,0.80$
Beer,Bira Moretti,3.20$
Beer,Bud Light,4$
Beer,Corona,3.50$
Beer,Stella Artois,4$
Beer,Heineken,2$
Beer,Budweiser,3.50$
Beer,Burgasko,1.50$
Juice,Coca-Cola,1$
Juice,Fanta,1$
Juice,Pepsi,1$
Juice,7 up,1$
Juice,Apple Juice,1$
Juice,Orange Juice,1$
Fruits,Apple,0.20$
Fruits,Avocado,0.60$
Fruits,Blueberrys,2.20$
Fruits,Banana,0.20$
Fruits,Grapefruit,0.50$
Fruits,Lemon,0.20$
Fruits,Strawberries,1.60$
Vegetables,Carrot,0.20$
Vegetables,Peas,2$
Vegetables,Broccoli,0.20$
Vegetables,Cucumber,0.30$
Vegetables,Tomato,0.30$
Vegetables,Mushrooms,3$
Hygiene,Shampoo,4$
Hygiene,Soap,1.20$
Hygiene,Toothbrush,2$
Hygiene,Hand sanitizer,1$
Hygiene,Deodorant,2$
Hygiene,Baby Wipes,2$
Hygiene,Toothpaste,2$

```
{% endtab %}
{% endtabs %}

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FUwIKQPujWqWSVFxQaX98%2Fuploads%2Fb9XH7UgQ2ORveXJjT5n9%2FSupermarket.mp4?alt=media&token=fb4a2188-63c2-40d6-8e13-b8633b997b41" %}
