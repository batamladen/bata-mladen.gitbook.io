# Login Page

Below is a simple login page code that prompts if login is successful or not based on the user input.

```java
import javax.swing.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;

public class loginPage implements ActionListener {

    private static JFrame frame;
    private static JPanel panel;
    private static JLabel userLabel;
    private static JTextField userText;
    private static JLabel passLabel;
    private static JPasswordField passText;
    private static JButton loginButton;
    private static JLabel success;

    public static void main(String[] args) {

        frame = new JFrame();
        panel = new JPanel();

        frame.setSize(350,200);
        frame.setTitle("Login Page");
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.add(panel);

        panel.setLayout(null);

        userLabel = new JLabel("User");
        userLabel.setBounds(20, 20, 80, 25);
        panel.add(userLabel);

        userText = new JTextField();
        userText.setBounds(100, 20, 165, 25);
        panel.add(userText);

        passLabel = new JLabel("Password");
        passLabel.setBounds(20, 50, 80, 25);
        panel.add(passLabel);

        passText = new JPasswordField();
        passText.setBounds(100, 50, 165, 25);
        panel.add(passText);

        loginButton = new JButton("Login");
        loginButton.setBounds(140, 90, 80, 25);
        loginButton.addActionListener(new loginPage());
        panel.add(loginButton);

        success = new JLabel("");
        success.setBounds(57, 120, 250, 25);
        success.setHorizontalAlignment(SwingConstants.CENTER);
        panel.add(success);

        frame.setVisible(true);

    }

    @Override
    public void actionPerformed(ActionEvent e) {
        String user = userText.getText();
        String pass = passText.getText();
        System.out.println(user + ", " + pass);

        if (user.equals("Mladen") && pass.equals("mladen123")) {
            success.setText("Login Successful");
        } else {
            success.setText("Invalid User or Password");
        }
    }
}
```

{% embed url="https://files.gitbook.com/v0/b/gitbook-x-prod.appspot.com/o/spaces%2FUwIKQPujWqWSVFxQaX98%2Fuploads%2FXP6Tp1EsbZjxUxTG6Fnl%2Flogin.mp4?alt=media&token=aaa0cef3-1183-4679-9947-2f9cfdae22cd" %}
