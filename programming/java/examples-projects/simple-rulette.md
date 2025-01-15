# Simple Rulette

{% tabs %}
{% tab title="rulette.java" %}
```java
package Roulette;

import java.util.ArrayList;
import java.util.List;
import java.util.Random;
import java.util.Scanner;

public class Roulette {
    private static int startMoney = 100;
    private static List<Integer> numbers = new ArrayList<>();
    private static List<String> colors = new ArrayList<>();
    private static List<Integer> bets = new ArrayList<>();
    private static List<Integer> amounts = new ArrayList<>();

    public static void main(String[] args) {
        setupWheel();

        System.out.println("Welcome to the table!");
        while (true) {
            String userInput = getUserInput("\noptions: \n1. place bet\n2. spin wheel\n3. leave the table\n");

            switch (userInput) {
                case "1":
                    placeBet();
                    break;
                case "2":
                    spinWheel();
                    break;
                case "3":
                    leaveTable();
                    break;
                default:
                    System.out.println("Not a valid option");
                    break;
            }
        }
    }

    private static void setupWheel() {
        for (int i = 0; i <= 36; i++) {
            numbers.add(i);
        }


        colors.add("red");
        colors.add("black");
    }

    private static void placeBet() {
        int bet = getIntegerInput("Enter the number you want to bet on (0-36): ");
        if (bet < 0 || bet > 36) {
            System.out.println("Invalid number. Please choose a number between 0 and 36.");
            return;
        }

        int amount = getIntegerInput("Enter the amount you want to bet: ");
        if (amount > startMoney) {
            System.out.println("Not enough money.");
            return;
        }

        bets.add(bet);
        amounts.add(amount);
        startMoney -= amount;
        System.out.println("Bet placed on " + bet + " with amount $" + amount + ". You have $" + startMoney + " left.");
    }

    private static void spinWheel() {
        Random random = new Random();
        int winningNumber = numbers.get(random.nextInt(numbers.size()));
        String winningColor = winningNumber % 2 == 0 ? "red" : "black";

        System.out.println("The ball landed on " + winningNumber + " (" + winningColor + ")");

        for (int i = 0; i < bets.size(); i++) {
            int bet = bets.get(i);
            int amount = amounts.get(i);
            if (bet == winningNumber) {
                startMoney += amount * 35;
                System.out.println("Congratulations! You won $" + amount * 35 + ".");
            } else if (bet % 2 == 0 && winningNumber % 2 == 0 || bet % 2 != 0 && winningNumber % 2 != 0) {
                startMoney += amount * 2;
                System.out.println("Congratulations! You won $" + amount * 2 + ".");
            } else {
                System.out.println("You lost $" + amount + ".");
            }
        }

        bets.clear();
        amounts.clear();
        System.out.println("You have $" + startMoney + " left.");
    }

    private static void leaveTable() {
        System.out.println("\n===================");
        System.out.println("You are leaving the table with $" + startMoney + ", \nsee you next time!");
        System.exit(0);
    }

    private static String getUserInput(String message) {
        Scanner scanner = new Scanner(System.in);
        System.out.print(message);
        return scanner.nextLine();
    }

    private static int getIntegerInput(String message) {
        while (true) {
            try {
                return Integer.parseInt(getUserInput(message));
            } catch (NumberFormatException e) {
                System.out.println("Invalid input. Please enter a valid number.");
            }
        }
    }
}

```


{% endtab %}

{% tab title="board.csv" %}
```csv
number,color
1,red
2,black
3,red
4,black
5,red
6,black
7,red
8,black
9,red
10,black
11,black
12,red
13,black
14,red
15,black
16,red
17,black
18,red
19,red
20,black
21,red
22,black
23,red
24,black
25,red
26,black
27,red
28,black
29,black
30,red
31,black
32,red
33,black
34,red
35,black
36,red
```


{% endtab %}
{% endtabs %}

