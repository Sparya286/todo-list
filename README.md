# todo-list
A simple yet powerful desktop To-Do List application built using Java.  It allows users to manage tasks efficiently with features like sorting tasks.  The project demonstrates OOP concepts such  as inheritance and encapsulation, along with  exception handling and multithreading for better performance and user experience.

####Functionality of the To-Do List Program
1. Add Task
Users can type a task in the text field and click "Add Task".

The task is added to the list and shown with a [ ] symbol (not completed).

2. View Tasks
Tasks appear in a scrollable list (JList), each with a checkbox-like symbol:

[ ] Task Name → incomplete

[✔ ] Task Name → completed

3. Mark as Completed
Select a task and click "Mark as Completed".

The task status changes, visually updating to [✔ ].

4. Delete Task
Select a task and click "Delete Task" to remove it from the list.

5. Sort Tasks
Clicking "Sort Tasks" organizes the list:

Incomplete tasks first

Then completed ones

Both sorted alphabetically


###Programming Concepts Used
OOP Principles:

Encapsulation: Task class holds data with getters/setters.

Inheritance: CompletedTask class extends Task.

Swing GUI Components:

JFrame, JPanel, JList, JScrollPane, JTextField, JButton

Event-Driven Programming:

Each button has an ActionListener that defines what happens when it’s clicked.

Exception Handling:

Prevents crashes and shows user-friendly messages via JOptionPane.

List Model Management:

DefaultListModel<Task> is used to dynamically update the task list.
