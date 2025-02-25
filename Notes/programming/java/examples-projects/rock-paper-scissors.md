# Rock Paper Scissors

```java
package RPS;

import java.awt.*;
import java.util.Random;
import javax.swing.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class RPSGUI implements ActionListener{
    private static JFrame frame;
    private static JPanel panel;
    private static JLabel result;
    private static JLabel your_move;
    private static JLabel comp_move;
    private static JLabel score;
    private static JButton button1;
    private static JButton button2;
    private static JButton button3;

    private static Random random = new Random();

    static ImageIcon rock1 = new ImageIcon("C:\\Users\\HP\\Downloads\\rock1.png");
    static ImageIcon rock2 = new ImageIcon("C:\\Users\\HP\\Downloads\\rock2.png");
    static ImageIcon paper1 = new ImageIcon("C:\\Users\\HP\\Downloads\\paper1.png");
    static ImageIcon paper2 = new ImageIcon("C:\\Users\\HP\\Downloads\\paper2.png");
    static ImageIcon sizors1 = new ImageIcon("C:\\Users\\HP\\Downloads\\scissors1.png");
    static ImageIcon sizors2 = new ImageIcon("C:\\Users\\HP\\Downloads\\scissors2.png");

    static ImageIcon resizeImage(ImageIcon icon, int width, int height) {
        Image img = icon.getImage();
        Image resizedImg = img.getScaledInstance(width, height, Image.SCALE_SMOOTH);
        return new ImageIcon(resizedImg);
    }

    static int human_choice;
    static int human_points = 0;
    static int comp_points = 0;

    public static void main(String[] args) {

        frame = new JFrame();
        panel = new JPanel();

        frame.setSize(250, 380);
        frame.setTitle("Rock, Paper, Sizors");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.add(panel);

        panel.setLayout(null);

        button1 = new JButton("Rock");
        button1.setBounds(69, 250, 100, 25);
        button1.addActionListener(new RPSGUI());
        panel.add(button1);

        button2 = new JButton("Paper");
        button2.setBounds(69, 280, 100, 25);
        button2.addActionListener(new RPSGUI());
        panel.add(button2);

        button3 = new JButton("scissors");
        button3.setBounds(69, 310, 100, 25);
        button3.addActionListener(new RPSGUI());
        panel.add(button3);

        your_move = new JLabel();
        your_move.setBounds(-106, 50, 250, 250);
        panel.add(your_move);

        comp_move = new JLabel();
        comp_move.setBounds(43, 50, 250, 250);
        panel.add(comp_move);

        result = new JLabel();
        result.setBounds(35, 70, 160, 40);
        result.setHorizontalAlignment(SwingConstants.CENTER);
        panel.add(result);

        score = new JLabel(human_points + ":" + comp_points);
        score.setBounds(35, 20, 160, 40);
        score.setHorizontalAlignment(SwingConstants.CENTER);
        score.setFont(new Font("Arial", Font.BOLD, 24));
        panel.add(score);

        frame.setVisible(true);

        }

    @Override
    public void actionPerformed(ActionEvent e) {
        if (e.getSource() == button1) {
            human_choice = 0;
        } else if (e.getSource() == button2) {
            human_choice = 1;
        } else if (e.getSource() == button3) {
            human_choice = 2;
        }

        if (human_choice != -1) {
            int comp_choice = random.nextInt(3);
            updateLabels(human_choice, comp_choice);
        }
    }

    private void updateLabels(int human_choice, int compChoice) {

        if (human_choice == 0) {
            your_move.setIcon(resizeImage(rock1, 300, 300));
        } else if (human_choice == 1) {
            your_move.setIcon(resizeImage(paper1, 300,300));
        } else if (human_choice == 2) {
            your_move.setIcon(resizeImage(sizors1,300,300));
        }

        if (compChoice == 0) {
            comp_move.setIcon(resizeImage(rock2,300,300));
        } else if (compChoice == 1) {
            comp_move.setIcon(resizeImage(paper2,300,300));
        } else if (compChoice == 2) {
            comp_move.setIcon(resizeImage(sizors2,300,300));
        }

        String resultText = getResultText(human_choice, compChoice);
        result.setText(resultText);

        if (resultText.equals("You win!")) {
            human_points++;
        }
        if (resultText.equals("Computer wins!")) {
            comp_points++;
        }
        score.setText(human_points + ":" + comp_points);
    }

    private String getResultText(int humanChoice, int compChoice) {
        String[] results = {"It's a draw!", "Computer wins!", "You win!"};
        int resultIndex = (compChoice - humanChoice + 3) % 3;
        return results[resultIndex];

    }
}
```

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FUwIKQPujWqWSVFxQaX98%2Fuploads%2FAZAF1J3EzSDrLgwIvnjb%2FROCK_P~1.MP4?alt=media&token=1d35e2dc-fb93-491a-b919-59127dc16c35" fullWidth="false" %}
