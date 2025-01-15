# Coffee Shop

{% tabs %}
{% tab title="main.java" %}
```java
package CoffeeShop;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.Scanner;

public class CoffeeShopGUI implements ActionListener {
    private JFrame frame;
    private JPanel panel;
    private static JLabel label;
    private JButton viewMenuButton;
    private JButton orderButton;
    private JButton seeBasketButton;
    private JButton seeBillButton;
    private JButton payAndExitButton;

    public CoffeeShopGUI() {
        frame = new JFrame();
        panel = new JPanel();

        viewMenuButton = new JButton("View Menu");
        orderButton = new JButton("Order");
        seeBasketButton = new JButton("See Basket");
        seeBillButton = new JButton("See Bill");
        payAndExitButton = new JButton("Pay and Exit");
        label = new JLabel("");

        viewMenuButton.addActionListener(this);
        orderButton.addActionListener(this);
        seeBasketButton.addActionListener(this);
        seeBillButton.addActionListener(this);
        payAndExitButton.addActionListener(this);

        panel.setBorder(BorderFactory.createEmptyBorder(50, 50, 50, 50));
        panel.setLayout(new GridLayout(0, 1));
        panel.add(viewMenuButton);
        panel.add(orderButton);
        panel.add(seeBasketButton);
        panel.add(seeBillButton);
        panel.add(payAndExitButton);

        frame.setSize(400,200);
        frame.add(panel, BorderLayout.CENTER);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setTitle("Coffee Shop");
        frame.pack();

        frame.setVisible(true);
    }

    public static void main(String[] args) {
        new CoffeeShopGUI();
    }

    public void actionPerformed(ActionEvent e) {
        String action = e.getActionCommand();

        switch (action) {
            case "View Menu":
                displayMenu();
                break;
            case "Order":
                order();
                break;
            case "See Basket":
                displayBasket();
                break;
            case "See Bill":
                displayTotalPrice();
                break;
            case "Pay and Exit":
                payAndExit();
                break;
            default:
                break;
        }
    }

    private void displayMenu() {
        CoffeeShop.displayMenu();
    }

    private void order() {
        Scanner input = new Scanner(System.in);
        CoffeeShop.order(input);
        input.close();
    }

    private void displayBasket() {
        CoffeeShop.displayBasket();
    }

    private void displayTotalPrice() {
        CoffeeShop.displayTotalPrice();
    }

    private void payAndExit() {
        JOptionPane.showMessageDialog(frame, "Payment received! Thank you for visiting Mladen's Coffee Shop!");
        System.exit(0);
    }
}
```
{% endtab %}
{% endtabs %}

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FUwIKQPujWqWSVFxQaX98%2Fuploads%2FYoaZJDzCTCqgHNbTcoXX%2Fcoffee%20shop.mp4?alt=media&token=88b17910-1c1a-42fd-82c8-7042494b825b" %}
