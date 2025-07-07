package XandO;

import javax.swing.*;
import java.awt.*;

public class XandOGame {
    JFrame frame = new JFrame("X and O Game");
    JPanel panel = new JPanel(new GridLayout(3, 3));
    JButton[] buttons = new JButton[9];
    Player playerOne = new Player("X");
    Player playerTwo = new Player("O");
    int turn = 0;
    WinChecker checker = new WinChecker();

    public void drawGrid() {
        for (int i = 0; i < 9; i++) {
            buttons[i] = new JButton();
            panel.add(buttons[i]);
            ButtonManager.setAction(buttons[i], i + 1, this);
        }

        frame.add(panel);
        frame.setSize(300, 300);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setVisible(true);
    }

    public void handleMove(JButton btn, int cell) {
        Player current = (turn % 2 == 0) ? playerOne : playerTwo;
        btn.setText(current.getSymbol());
        btn.setEnabled(false);
        current.addMove(cell);
        turn++;

        if (checker.checkWin(current)) {
            JOptionPane.showMessageDialog(frame, current.getSymbol() + " wins!");
            resetGame();
        } else if (turn == 9) {
            JOptionPane.showMessageDialog(frame, "It's a draw!");
            resetGame();
        }
    }

    private void resetGame() {
        for (JButton btn : buttons) {
            btn.setText("");
            btn.setEnabled(true);
        }
        playerOne.clear();
        playerTwo.clear();
        turn = 0;
    }
}
