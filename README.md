# todo-list
A simple yet powerful desktop To-Do List application built using Java.  It allows users to manage tasks efficiently with features like sorting tasks.  The project demonstrates OOP concepts such  as inheritance and encapsulation, along with  exception handling and multithreading for better performance and user experience.


import javax.swing.*;
import java.awt.*;
import java.awt.event.*;
import java.util.ArrayList;
import java.util.Collections;
import java.util.Comparator;

// Base class - Encapsulates task data
class Task {
    private String name;
    private boolean completed;

    public Task(String name) {
        this.name = name;
        this.completed = false;
    }

    public String getName() {
        return name;
    }

    public boolean isCompleted() {
        return completed;
    }

    public void markCompleted() {
        this.completed = true;
    }

    @Override
    public String toString() {
        return (completed ? "[✔ ] " : "[ ] ") + name;
    }
}

// Subclass (Inheritance example)
class CompletedTask extends Task {
    public CompletedTask(String name) {
        super(name);
        markCompleted(); // Inherited method
    }
}

// GUI Application
public class TaskManagerGUI extends JFrame {
    private DefaultListModel<Task> taskListModel = new DefaultListModel<>();
    private JList<Task> taskJList = new JList<>(taskListModel);
    private JTextField taskField = new JTextField(20);

    public TaskManagerGUI() {
        super("Task Manager");
        setLayout(new BorderLayout());

        // Top Panel - Add Task
        JPanel topPanel = new JPanel();
        JButton addButton = new JButton("Add Task");
        topPanel.add(taskField);
        topPanel.add(addButton);

        // Center Panel - Task List
        taskJList.setSelectionMode(ListSelectionModel.SINGLE_SELECTION);
        JScrollPane scrollPane = new JScrollPane(taskJList);

        // Bottom Panel - Action Buttons
        JPanel bottomPanel = new JPanel();
        JButton completeButton = new JButton("Mark as Completed");
        JButton deleteButton = new JButton("Delete Task");
        JButton sortButton = new JButton("Sort Tasks");
        bottomPanel.add(completeButton);
        bottomPanel.add(deleteButton);
        bottomPanel.add(sortButton);

        // Add Panels to Frame
        add(topPanel, BorderLayout.NORTH);
        add(scrollPane, BorderLayout.CENTER);
        add(bottomPanel, BorderLayout.SOUTH);

        // Event Handlers
        addButton.addActionListener(e -> {
            try {
                String text = taskField.getText().trim();
                if (text.isEmpty()) {
                    throw new IllegalArgumentException("Task name cannot be empty!");
                }
                taskListModel.addElement(new Task(text)); // ➤ add task
                taskField.setText("");
            } catch (Exception ex) {
                JOptionPane.showMessageDialog(this, "Error: " + ex.getMessage(), "Input Error", JOptionPane.ERROR_MESSAGE);
            }
        });

        completeButton.addActionListener(e -> {
            try {
                int index = taskJList.getSelectedIndex();
                if (index == -1) {
                    throw new Exception("No task selected to mark as completed.");
                }
                taskListModel.get(index).markCompleted(); // ➤ mark as completed
                taskJList.repaint();
            } catch (Exception ex) {
                JOptionPane.showMessageDialog(this, "Error: " + ex.getMessage(), "Action Error", JOptionPane.WARNING_MESSAGE);
            }
        });

        deleteButton.addActionListener(e -> {
            try {
                int index = taskJList.getSelectedIndex();
                if (index == -1) {
                    throw new Exception("No task selected to delete.");
                }
                taskListModel.remove(index); // ➤ delete task
            } catch (Exception ex) {
                JOptionPane.showMessageDialog(this, "Error: " + ex.getMessage(), "Action Error", JOptionPane.WARNING_MESSAGE);
            }
        });

        sortButton.addActionListener(e -> {
            // Run sort in a new thread to keep GUI responsive
            new Thread(() -> {
                try {
                    sortTasks(); // ➤ sort task (Thread-safe call)
                } catch (Exception ex) {
                    SwingUtilities.invokeLater(() -> 
                        JOptionPane.showMessageDialog(this, "Error during sorting: " + ex.getMessage(), "Sort Error", JOptionPane.ERROR_MESSAGE)
                    );
                }
            }).start();
        });

        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setSize(400, 300);
        setVisible(true);
    }

    // Method to sort tasks
    private void sortTasks() {
        ArrayList<Task> tasks = Collections.list(taskListModel.elements());
        tasks.sort(Comparator.comparing(Task::isCompleted).thenComparing(Task::getName));
        SwingUtilities.invokeLater(() -> {
            taskListModel.clear();
            for (Task task : tasks) {
                taskListModel.addElement(task);
            }
        });
    }

    public static void main(String[] args) {
        SwingUtilities.invokeLater(TaskManagerGUI::new);
    }
}
