# OOPS-Project

# Bank Account Management System

A console-based banking application built in C++ to demonstrate core Object-Oriented Programming (OOP) principles. 

## 🚀 Features
* **Account Handling:** Supports multiple account types (Savings and Checking).
* **Secure Transactions:** Deposit and withdraw funds with built-in validation.
* **Interest Processing:** Calculate and apply interest specifically for savings accounts.
* **Overdraft Limits:** Checking accounts support configured overdraft limits, rejecting transactions that exceed them.

## 🧠 OOP Concepts Demonstrated
* **Encapsulation:** Sensitive data like `balance` and `accountNumber` are kept private/protected and modified only via secure public methods.
* **Inheritance:** `SavingsAccount` and `CheckingAccount` inherit core variables and methods from the `BankAccount` base class.
* **Polymorphism:** The `withdraw()` method is overridden to behave differently depending on the account type at runtime.
* **Abstraction:** The base `BankAccount` class is abstract, ensuring generic accounts cannot be created and forcing derived classes to implement specific behaviors.

## 📂 File Structure
This project is organized into modular header and source files for clean architecture:
* `main.cpp` - The entry point and transaction simulation.
* `BankAccount.h` & `BankAccount.cpp` - The abstract base class.
* `SavingsAccount.h` & `SavingsAccount.cpp` - Derived class handling interest.
* `CheckingAccount.h` & `CheckingAccount.cpp` - Derived class handling overdrafts.

## 🛠️ How to Build and Run

### Prerequisites
* A C++ compiler (like `g++` via MinGW)
* Visual Studio Code with a terminal (like Git Bash)

### Compilation
Open your terminal in the project directory and run the following command to link and compile all the source files:
```bash
g++ main.cpp BankAccount.cpp SavingsAccount.cpp CheckingAccount.cpp -o bank_system
