# Hangman

{% tabs %}
{% tab title="Hangman.java" %}
```java
package Hangman;

import javax.swing.*;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.util.ArrayList;
import java.util.List;
import java.util.Random;

import static Hangman.Wordlist.WORD_LIST;

public class Hangman implements ActionListener {

    static final Random RANDOM = new Random();

    static String chosenWord;
    int wordLength;
    static int lives = 6;

    static List<Character> display = new ArrayList<>();
    List<Character> wrongGuesses = new ArrayList<>();

    static JFrame frame;
    static JPanel panel;
    static JLabel wordDisplay;
    static JLabel hangmanArt;
    static JLabel hearths;
    static JLabel input_message;
    static JTextField input;

    public Hangman() {
        chosenWord = WORD_LIST.get(RANDOM.nextInt(WORD_LIST.size()));
        wordLength = chosenWord.length();
        lives = 6;

        frame = new JFrame();
        frame.setTitle("Hangman");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(330, 350);
        frame.setLocationRelativeTo(null);


        panel = new JPanel();
        panel.setLayout(null);
        panel.setBackground(Color.WHITE);

        wordDisplay = new JLabel();
        wordDisplay.setBounds(200, 70, 300, 50);

        StringBuilder startDisplay = new StringBuilder();
        for (int i = 0; i < chosenWord.length(); i++) {
            startDisplay.append("_ ");
        }
        wordDisplay.setText(startDisplay.toString());
        panel.add(wordDisplay);

        hangmanArt = new JLabel();
        hangmanArt.setBounds(20, 20, 150, 200);
        hangmanArt.setIcon(Art.hangman0);
        panel.add(hangmanArt);

        hearths = new JLabel();
        hearths.setText("Lives: " + String.valueOf(lives));
        hearths.setBounds(200, 20, 50, 20);
        panel.add(hearths);

        input = new JTextField();
        input.setBounds(104, 257, 20, 20);
        input.addActionListener(this);
        panel.add(input);

        input_message = new JLabel("Enter a letter : ");
        input_message.setBounds(20, 250, 100, 30);
        panel.add(input_message);

        JOptionPane.showMessageDialog(null, "To win, guess the word before the person is hung.");

        frame.add(panel);
        frame.setVisible(true);


    }


    public static void main(String[] args) {
        new Hangman();
    }

    public static void updateLabel(char guess) {
        if (chosenWord.contains(String.valueOf(guess))) {
            // Update wordDisplay
            StringBuilder displayString = new StringBuilder();
            for (int i = 0; i < chosenWord.length(); i++) {
                char c = chosenWord.charAt(i);
                if (display.contains(c)) {
                    displayString.append(c).append(' ');
                } else if (c == guess) {
                    displayString.append(c).append(' ');  // Print the guessed letter
                    display.add(c);  // Add the guessed letter to the display list
                } else {
                    displayString.append("_ ");
                }
            }
            wordDisplay.setText(displayString.toString());

            if (displayString.indexOf("_") == -1) {
                JOptionPane.showMessageDialog(null, "Congratulations. you guessed the word: " + chosenWord);
            }

        } else {
            lives--;
            hearths.setText("Lives: " + lives);

            if (lives == 6) {
                hangmanArt.setIcon(Art.hangman0);
            } else if (lives == 5) {
                hangmanArt.setIcon(Art.hangman1);
            } else if (lives == 4) {
                hangmanArt.setIcon(Art.hangman2);
            } else if (lives == 3) {
                hangmanArt.setIcon(Art.hangman3);
            } else if (lives == 2) {
                hangmanArt.setIcon(Art.hangman4);
            } else if (lives == 1) {
                hangmanArt.setIcon(Art.hangman5);
            } else {
                hangmanArt.setIcon(Art.hangman6);
                JOptionPane.showMessageDialog(null, "You lost. The correct word was: " + chosenWord);
            }
        }
    }

    @Override
    public void actionPerformed(ActionEvent e) {
        String guess = input.getText();
        if (guess.length() == 1 && Character.isLetter(guess.charAt(0))) {
            char guessedLetter = guess.charAt(0);
            updateLabel(guessedLetter);
            input.setText(""); // Clear the input field after each guess
        } else {
            JOptionPane.showMessageDialog(null, "Please enter a single letter.");
            input.setText(""); // Clear the input field if invalid input
        }
    }
}

```
{% endtab %}

{% tab title="Wordlist.java" %}
```java
package Hangman;

import java.util.List;
import java.util.Arrays;
public class Wordlist {

    public static final List<String> WORD_LIST = Arrays.asList(
            "абонамент", "багажник", "валута", "галактика", "деветнадесет", "експеримент", "жълт", "заплата",
            "изпратен", "квартира", "лаборатория", "минимум", "наблюдение", "обектив", "парти", "резерват",
            "същество", "таблетка", "университет", "философия", "химия", "централен", "червено", "шест",
            "щастие", "ъгълник", "юноша", "яйце", "автомат", "брошура", "вестник", "глобус", "дърво",
            "език", "живот", "змия", "играч", "колело", "лилаво", "момче", "надпис", "очила", "пешеходец",
            "робот", "синьо", "телефон", "улица", "фотоапарат", "хотел", "цвете", "черно", "шапка", "щура",
            "ъглов", "юниор", "як", "автор", "бар", "вятър", "град", "диаметър", "ежедневен", "земя",
            "илюстрация", "книга", "лист", "мозък", "напред", "осветление", "пари", "ръка", "свят", "тъмно",
            "учебник", "филм", "храна", "цар", "човек", "шкаф", "щанд", "ъгъл", "юли", "яхта", "автобус",
            "баланс", "вълк", "галерия", "джоб", "езда", "жълъд", "завод", "изграден", "клуб", "лампа",
            "музика", "набор", "образ", "пари", "рецепт", "свещ", "трактор", "успех", "филтър", "хранене",
            "цел", "чаша", "шпагат", "щастлив", "ълцер", "ювелир", "якор", "авторитет", "бариера", "вълна",
            "гама", "диамант", "ежедневие", "защита", "идеал", "колега", "линейка", "модел", "награда", "облик",
            "парад", "ракия", "святлина", "такси", "услуга", "финал", "хаос", "център", "чек", "шуба", "щур",
            "ъгълът", "юла", "ябълка", "авиация", "барут", "вълшебник", "газ", "диверсия", "ежегоден", "звезда",
            "избор", "коледа", "лира", "май", "надежда", "облачно", "парк", "ракета", "сграда", "тайна",
            "убийство", "фактор", "хвърляне", "център", "чиния", "шампион", "щат", "ърнест", "юни", "ярост",
            "август"


    );
}
```
{% endtab %}

{% tab title="Art.java" %}
```java
package Hangman;

import javax.swing.ImageIcon;

public class Art {
    static ImageIcon hangman0 = new ImageIcon("C:\\path\\to\\file\\hangman0.PNG");
    static ImageIcon hangman1 = new ImageIcon("C:\\path\\to\\file\\hangman1.PNG");
    static ImageIcon hangman2 = new ImageIcon("C:\\path\\to\\file\\hangman2.PNG");
    static ImageIcon hangman3 = new ImageIcon("C:\\path\\to\\file\\hangman3.PNG");
    static ImageIcon hangman4 = new ImageIcon("C:\\path\\to\\file\\hangman4.PNG");
    static ImageIcon hangman5 = new ImageIcon("C:\\path\\to\\file\\hangman5.PNG");
    static ImageIcon hangman6 = new ImageIcon("C:\\path\\to\\file\\hangman6.PNG");


}


```
{% endtab %}
{% endtabs %}

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FUwIKQPujWqWSVFxQaX98%2Fuploads%2FAojgdfIZUcLDjsDRzdrT%2F%D1%85%D0%B0%D0%BD%D0%B3%D0%BC%D0%B0%D0%BD.mp4?alt=media&token=683f1095-bede-4069-b07f-8aa7603d6d69" %}
