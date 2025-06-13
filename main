#include <iostream>
#include <memory>
#include <stdexcept>
#include <string>

// Custom exception classes
class NegativeDepositException : public std::runtime_error {
public:
    NegativeDepositException() : std::runtime_error("Cannot deposit a negative amount!") {}
};

class OverdrawException : public std::runtime_error {
public:
    OverdrawException() : std::runtime_error("Insufficient funds for withdrawal!") {}
};

class InvalidAccountOperationException : public std::runtime_error {
public:
    InvalidAccountOperationException() : std::runtime_error("Transaction not allowed on closed account!") {}
};

class BankAccount {
private:
    std::string accountNumber;
    double balance;
    bool isActive;

public:
    // Constructor
    BankAccount(std::string accNum, double initialBalance) 
        : accountNumber(accNum), balance(initialBalance), isActive(true) {}

    // Deposit method
    void deposit(double amount) {
        if (!isActive) throw InvalidAccountOperationException();
        if (amount < 0) throw NegativeDepositException();
        balance += amount;
    }

    // Withdraw method
    void withdraw(double amount) {
        if (!isActive) throw InvalidAccountOperationException();
        if (amount > balance) throw OverdrawException();
        balance -= amount;
    }

    // Get balance
    double getBalance() const {
        return balance;
    }

    // Close account
    void closeAccount() {
        isActive = false;
    }
};

void testEdgeCases() {
    auto account = std::make_unique<BankAccount>("789101", 500.0);

    try {
        std::cout << "\nTesting edge cases...\n";
        std::cout << "Depositing -100...\n";
        account->deposit(-100);
    } catch (const std::exception& e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }

    try {
        std::cout << "Attempting to withdraw $600 (more than balance)...\n";
        account->withdraw(600);
    } catch (const std::exception& e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }

    try {
        std::cout << "Closing account...\n";
        account->closeAccount();
        std::cout << "Attempting to deposit $50 after closure...\n";
        account->deposit(50);
    } catch (const std::exception& e) {
        std::cerr << "Exception: " << e.what() << std::endl;
    }
}

int main() {
    try {
        // Smart pointer managing BankAccount instance
        auto account = std::make_unique<BankAccount>("123456", 1000.0);
        std::cout << "Bank Account Created: #" << "123456" << std::endl;

        // Demonstrate deposit
        std::cout << "Depositing $1000..." << std::endl;
        account->deposit(1000);
        std::cout << "Current Balance: $" << account->getBalance() << std::endl;

        // Demonstrate withdrawal
        std::cout << "Withdrawing $500..." << std::endl;
        account->withdraw(500);
        std::cout << "Current Balance: $" << account->getBalance() << std::endl;

        // Test additional edge cases
        testEdgeCases();

    } catch (const std::exception& e) {
        std::cerr << "Error: " << e.what() << std::endl;
    }

    return 0;
}
