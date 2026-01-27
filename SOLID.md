# SOLID Principles

SOLID principles are one of the object-oriented approaches used in software development, intended to create quality software.

## The Reason for SOLID Principles

Design principles encourage us to create more maintainable, understandable, and flexible software.

## The Five SOLID Principles

The following five concepts make up the SOLID principles:

- **Single Responsibility Principle (SRP)**
- **Open/Closed Principle (OCP)**
- **Liskov Substitution Principle (LSP)**
- **Interface Segregation Principle (ISP)**
- **Dependency Inversion Principle (DIP)**

---

## Single Responsibility Principle (SRP)

A class should only have **one responsibility**. Furthermore, it should only have **one reason to change**.

### How does this principle help us build better software?

- **Testing** – A class with one responsibility will have far fewer test cases.
- **Lower Coupling** – Less functionality in a single class results in fewer dependencies.
- **Organization** – Smaller, well-organized classes are easier to understand and maintain than monolithic ones.

---

```java
public class BankAccount {
  private double balance;
  private String accountNo;
  private String accountType;

  //constructor
  public BankAccount(double balance, String accountNo, String accountType) {
     this.balance = balance;
     this.accountNo = accountNo;
     this.accountType = accountType;
  }
  
  public void deposit() {
     //code to deposit amount
  }

  public void withdraw(double amount) {
     //code to withdraw amount
  }

  public double calculateInterest() {
     //code to calculate interest
  }

  public void saveBankAccountDetails() {
     //save account information to database
  }

  public void sendSmsNotification() {
    //code to send SMS notification to customer
  }
}
```

```java
// BankAccount class will handle account related responsibilities
public class BankAccount {
  private double balance;
  private String accountNo;
  private String accountType;
  private SQLBankAccountRepository sqlBankAccountRepository;
  private NotificationService notificationService;

  //constructor
  public BankAccount(double balance, String accountNo, String accountType, SQLBankAccountRepository sqlBankAccountRepository, NotificationService notificationService) {
     this.balance = balance;
     this.accountNo = accountNo;
     this.accountType = accountType;
     this.sqlBankAccountRepository = sqlBankAccountRepository;
     this.notificationService = notificationService;
  }

  public void deposit() {
    //code to deposit amount
  }

  public void withdraw(double amount) {
   //code to withdraw amount
  }

  public double calculateInterest() {
   //code to calculate interest
  }
}

// SQLBankAccountRepository class will handle database related responsibilities
public class SQLBankAccountRepository {
  public void saveBankAccountDetails(BankAccount bankAccount) {
   //save account information to database
  }
}

// NotificationService class will handle notification related responsibilities
public class NotificationService {
  public void sendSmsNotification(BankAccount bankAccount) {
    //code to send SMS notification to customer
  }
}
```




























