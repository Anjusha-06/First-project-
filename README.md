The Expense Tracker System is a Java-based application designed to manage and 
record personal financial expenses efficiently. The project integrates JavaFX for 
building a graphical user interface and JavaDB (Apache Derby) as the backend 
database for storing expense details such as date, category, amount, and 
description.
This system allows users to perform the following operations:
Add new expenses.
View all recorded expenses.
View a summary of expenses by category.
It helps users track spending habits and analyze financial patterns through an 
intuitive interface and real-time database updates.
Unlike standalone applications, this system leverages database connectivity 
through JDBC, enabling persistent storage and retrieval of data. This ensures that 
expenses are not lost between sessions and provides a strong foundation for 
scalability and future enhancements.
The project demonstrates the integration of object-oriented programming, 
database handling, and GUI design, making it a complete end-to-end application 
suitable for real-world use.
SYSTEM DESIGN
The Expense Tracker System consists of four main modules that interact to 
provide a smooth user experience.
System Flow Overview
1. Display main menu (Add Expense, View Expenses, View Summary, Exit).
2. User selects a desired operation.
3. System validates user input.
4. Executes corresponding action (insert, query, or summarize data).
5. Displays results to the user.
6. Returns to the main menu.