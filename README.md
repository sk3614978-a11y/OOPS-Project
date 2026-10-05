# OOPS-Project


#include <iostream>
#include <string>
#include <vector>
#include <iomanip>

using namespace std;

// 1. Abstraction & Encapsulation: Base class hides data and provides a public interface.
class BankAccount {
protected: 
    string accountNumber;
    string accountHolder;
    double balance;

public:
    BankAccount(string accNum, string holder, double initialBalance) {
        accountNumber = accNum;
        accountHolder = holder;
        balance = initialBalance;
    }

    // Virtual destructor ensures proper cleanup of derived objects
    virtual ~BankAccount() {} 

    void deposit(double amount) {
        if (amount > 0) {
            balance += amount;
            cout << "Deposited $" << amount << " into " << accountNumber << ".\n";
        }
    }

    double getBalance() const { return balance; }

    // Pure virtual functions make this an Abstract Class
    virtual void withdraw(double amount) = 0;
    virtual void display() const = 0;
};

// 2. Inheritance: SavingsAccount inherits from BankAccount
class SavingsAccount : public BankAccount {
private:
    double interestRate;

public:
    SavingsAccount(string accNum, string holder, double initialBalance, double rate)
        : BankAccount(accNum, holder, initialBalance), interestRate(rate) {}

    // 3. Polymorphism: Overriding the base class method
    void withdraw(double amount) override {
        if (amount > balance) {
            cout << "Denied: Insufficient funds in Savings (" << accountNumber << ").\n";
        } else if (amount > 0) {
            balance -= amount;
            cout << "Withdrew $" << amount << " from " << accountNumber << ".\n";
        }
    }

    void addInterest() {
        double interest = balance * (interestRate / 100);
        balance += interest;
        cout << "Applied $" << interest << " interest to " << accountNumber << ".\n";
    }

    void display() const override {
        cout << "[Savings]  " << accountNumber << " | " << accountHolder 
             << " | Balance: $" << fixed << setprecision(2) << balance 
             << " | Rate: " << interestRate << "%\n";
    }
};

// 2. Inheritance: CheckingAccount inherits from BankAccount
class CheckingAccount : public BankAccount {
private:
    double overdraftLimit;

public:
    CheckingAccount(string accNum, string holder, double initialBalance, double limit)
        : BankAccount(accNum, holder, initialBalance), overdraftLimit(limit) {}

    // 3. Polymorphism: Overriding the base class method with different logic
    void withdraw(double amount) override {
        if (amount > (balance + overdraftLimit)) {
            cout << "Denied: Exceeds overdraft limit on Checking (" << accountNumber << ").\n";
        } else if (amount > 0) {
            balance -= amount;
            cout << "Withdrew $" << amount << " from " << accountNumber << ".\n";
        }
    }

    void display() const override {
        cout << "[Checking] " << accountNumber << " | " << accountHolder 
             << " | Balance: $" << fixed << setprecision(2) << balance 
             << " | Overdraft Limit: $" << overdraftLimit << "\n";
    }
};

int main() {
    // Storing multiple account types in a single vector using base class pointers
    vector<BankAccount*> accounts;

    // Dynamically allocating memory for objects
    accounts.push_back(new SavingsAccount("SAV-1001", "Alice", 1500.0, 3.5));
    accounts.push_back(new CheckingAccount("CHK-2001", "Bob", 500.0, 300.0));

    cout << "--- Initial Accounts ---\n";
    for (BankAccount* acc : accounts) {
        acc->display();
    }

    cout << "\n--- Performing Transactions ---\n";
    accounts[0]->deposit(500);
    accounts[0]->withdraw(2500); // Fails: exceeds balance
    accounts[1]->withdraw(700);  // Succeeds: uses $200 of the $300 overdraft limit

    // Safely downcasting to apply interest (which is specific to SavingsAccount)
    SavingsAccount* savings = dynamic_cast<SavingsAccount*>(accounts[0]);
    if (savings) {
        savings->addInterest();
    }

    cout << "\n--- Final Accounts ---\n";
    for (BankAccount* acc : accounts) {
        acc->display();
    }

    // Freeing dynamically allocated memory
    for (BankAccount* acc : accounts) {
        delete acc;
    }
    accounts.clear();

    return 0;
}
