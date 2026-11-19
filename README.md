# -OOPS-EXP.10
C++ program to implement a CSV file reader / writer using file I/O (fstream) . Storing and retrieving student records ,using file pointer ( seekg ,seekp ,tellg ,tellp ) 

#include <iostream>
#include <exception>
#include <string>
using namespace std;

class BankingException : public exception {
string msg;
public:

    BankingException(string m) : msg(m) {}
    const char* what() const noexcept override { return msg.c_str(); }};
    
    struct InsufficientFundsException : public BankingException {
    InsufficientFundsException(double d) : BankingException("Money short by ₹" + to_string(d)) {}};
    
    struct InvalidAmountException : public BankingException {
    InvalidAmountException(double a) : BankingException("Invalid amount: ₹" + to_string(a)) {}};
    
    class BankAccount {
    double balance;
    
public:

    BankAccount(double b) : balance(b) {}
    void withdraw(double amt) {
        if (amt <= 0) throw InvalidAmountException(amt);
        if (amt > balance) throw InsufficientFundsException(amt - balance);
        balance -= amt;
        cout << "Withdrawn: ₹" << amt << " | Balance: ₹" << balance << "\n";
    }};

int main() {

    BankAccount acc(5000.0);
    try {
        acc.withdraw(8000.0); 
    } catch (const BankingException& e) {
        cout << "[Caught Error] " << e.what() << "\n";
    }
    return 0;
}
