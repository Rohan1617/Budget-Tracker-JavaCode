# Budget-Tracker-JavaCode
import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.io.*;
import java.util.*;

public class BudgetTracker {
    private JFrame frame;
    private JTextField amountField, categoryField;
    private JTextArea displayArea;
    private File file = new File("expenses.txt");

    public BudgetTracker() {
        frame = new JFrame("Budget Tracker");
        frame.setSize(400, 400);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);

        JPanel panel = new JPanel();
        panel.setLayout(new GridLayout(5, 2));

        panel.add(new JLabel("Amount:"));
        amountField = new JTextField();
        panel.add(amountField);

        panel.add(new JLabel("Category:"));
        categoryField = new JTextField();
        panel.add(categoryField);

        JButton addButton = new JButton("Add Expense");
        addButton.addActionListener(e -> addExpense());
        panel.add(addButton);

        JButton viewButton = new JButton("View Expenses");
        viewButton.addActionListener(e -> viewExpenses());
        panel.add(viewButton);

        JButton clearButton = new JButton("Clear Expenses");
        clearButton.addActionListener(e -> clearExpenses());
        panel.add(clearButton);

        displayArea = new JTextArea();
        displayArea.setEditable(false);

        frame.add(panel, BorderLayout.NORTH);
        frame.add(new JScrollPane(displayArea), BorderLayout.CENTER);

        frame.setVisible(true);
    }

    private void addExpense() {
        String amount = amountField.getText().trim();
        String category = categoryField.getText().trim();
        if (amount.isEmpty() || category.isEmpty()) {
            JOptionPane.showMessageDialog(frame, "Fields cannot be empty");
            return;
        }
        try (FileWriter writer = new FileWriter(file, true)) {
            writer.write(amount + " - " + category + "\n");
            JOptionPane.showMessageDialog(frame, "Expense Added!");
        } catch (IOException ex) {
            ex.printStackTrace();
        }
        amountField.setText("");
        categoryField.setText("");
    }

    private void viewExpenses() {
        displayArea.setText("");
        try (Scanner scanner = new Scanner(file)) {
            while (scanner.hasNextLine()) {
                displayArea.append(scanner.nextLine() + "\n");
            }
        } catch (FileNotFoundException ex) {
            displayArea.setText("No expenses recorded.");
        }
    }

    private void clearExpenses() {
        try (FileWriter writer = new FileWriter(file)) {
            writer.write("");
            displayArea.setText("");
            JOptionPane.showMessageDialog(frame, "Expenses Cleared!");
        } catch (IOException ex) {
            ex.printStackTrace();
        }
    }

    // *Main method*
    public static void main(String[] args) {
        SwingUtilities.invokeLater(BudgetTracker::new);
    }
}
