## Liskov Substitution Principle (LSP)

- **LSP states that objects of a derived class should be able to replace objects of the base class without affecting the correctness of the program.**
- This principle ensures that inheritance relationships are well-designed and that the derived class respects the contract of the base class.

---

## LSP Violation Example

As we know, an **FD (Fixed Deposit) account** restricts the withdrawal feature until the maturity date.  
If we replace an `Account` reference with an `FDAccount` object and attempt to call `withdraw()`, it may result in an error or unexpected behavior.

This indicates that the derived class (`FDAccount`) cannot properly replace the base class (`Account`), which violates the Liskov Substitution Principle.

### Problematic Design (LSP Violation)
```java
// Account interface
interface Account {
    void deposit(double amount);
    void withdraw(double amount);
}

class SavingAccount implements Account {
    private double balance;

    //constructor
    //deposit
    //withdraw
}

class CurrentAccount implements Account {
    private double balance;

    //constructor
    //deposit
    //withdraw
}

class FixedTermAccount implements Account {
    private double balance;

    //constructor
    //deposit

    @Override
    public void withdraw(double amount) {
        throw new UnsupportedOperationException("Withdrawal not allowed in Fixed Term Account!");
    }
}

class BankClient {
    private List<Account> accounts;

    public BankClient(List<Account> accounts) {
        this.accounts = accounts;
    }

    public void processTransactions() {
        for (Account acc : accounts) {
            acc.deposit(1000);  // All accounts allow deposits

            // Assuming all accounts support withdrawal (LSP Violation)
            try {
                acc.withdraw(500);
            } catch (UnsupportedOperationException e) {
                System.out.println("Exception: " + e.getMessage());
            }
        }
    }
}

public class LSPViolated {
    public static void main(String[] args) {
        List<Account> accounts = new ArrayList<>();
        accounts.add(new SavingAccount());
        accounts.add(new CurrentAccount());
        accounts.add(new FixedTermAccount());

        BankClient client = new BankClient(accounts);
        client.processTransactions(); // Throws exception when withdrawing from FixedTermAccount
    }
}

```
To resolve this LSP violation, you should restructure the class hierarchy and ensure that derived classes confirm to the contract of the base class. 
Now we make the separate interface so that parent class can be replace by subclass

```java

import java.util.ArrayList;
import java.util.List;

// 1. DepositOnlyAccount interface: only allows deposits
interface DepositOnlyAccount {
    void deposit(double amount);
}

// 2. WithdrawableAccount interface: allows deposits and withdrawals
interface WithdrawableAccount extends DepositOnlyAccount {
    void withdraw(double amount);
}

class SavingAccount implements WithdrawableAccount {
    private double balance;

    //constructor
    //deposit
    //withdraw
}

class CurrentAccount implements WithdrawableAccount {
    private double balance;

    //constructor
    //deposit
    //withdraw
}

class FixedTermAccount implements DepositOnlyAccount {
    private double balance;

    //constructor
    //deposit
}

class BankClient {
    private List<WithdrawableAccount> withdrawableAccounts;
    private List<DepositOnlyAccount> depositOnlyAccounts;

    public BankClient(List<WithdrawableAccount> withdrawableAccounts,
                      List<DepositOnlyAccount> depositOnlyAccounts) {
        this.withdrawableAccounts = withdrawableAccounts;
        this.depositOnlyAccounts = depositOnlyAccounts;
    }

    public void processTransactions() {
        for (WithdrawableAccount acc : withdrawableAccounts) {
            acc.deposit(1000);
            acc.withdraw(500);
        }
        for (DepositOnlyAccount acc : depositOnlyAccounts) {
            acc.deposit(5000);
        }
    }
}

public class LSPFollowed {
    public static void main(String[] args) {
        List<WithdrawableAccount> withdrawableAccounts = new ArrayList<>();
        withdrawableAccounts.add(new SavingAccount());
        withdrawableAccounts.add(new CurrentAccount());

        List<DepositOnlyAccount> depositOnlyAccounts = new ArrayList<>();
        depositOnlyAccounts.add(new FixedTermAccount());

        BankClient client = new BankClient(withdrawableAccounts, depositOnlyAccounts);
        client.processTransactions();
    }
}

```

## Liskov Substitution Principle (LSP) – Behavioral Rules

**Child class should behave like a parent class.**

To ensure this, LSP defines three main guidelines:

- Signature Rule  
- Property Rule  
- Method Rule  

---

## 1. Signature Rule

### Method Arguments
- Subtype method arguments can be **identical to or wider** than the supertype.
- Java enforces this by requiring the **same method signature** for method overrides.

### Return Type
- Subtype overridden method return type should be:
  - **Identical**, or
  - **Narrower** than the parent method’s return type (**covariant return type**).
- Java supports covariant return types out of the box.

### Exceptions
- Subtype methods should **not throw broader or new checked exceptions** than the parent method.
- Subtypes may:
  - Throw the same exceptions
  - Throw narrower (subclass) exceptions
  - Throw no exceptions at all

---

## 2. Property Rule

### Class Invariant
- A **class invariant** of a parent class must not be broken by the child class.
- Example:
  - `balance` should never be negative.
- A child class may:
  - Maintain the invariant, or
  - Strengthen the invariant
- A child class must **never weaken or violate** the invariant.

### History Constraint
- Subclass methods should **not allow state changes** that the base class never allowed.
- Example:
  - `withdraw()` is allowed in `Account`
  - `withdraw()` is **not allowed** in `FDAccount`
- Allowing such a change breaks the expected behavior and violates LSP.

---

## 3. Method Rule

### Precondition
- A **precondition** must be satisfied before a method can be executed.
- Example:
  - Parent class: password length `<= 8`
  - Child class: password length `<= 6`
- Rule:
  - Subclasses can **weaken** the precondition

### Postcondition
- A **postcondition** must be satisfied after a method is executed.
- Rule:
  - Subclasses can **strengthen** the postcondition
- Example:
  - Parent method: on break method just break the car
  - Child method: increase battery on break

---

## Summary

To comply with the **Liskov Substitution Principle**:
- Child classes must be substitutable for parent classes
- Contracts defined by the parent must be preserved
- Behavior consistency is more important than code reuse

Following these rules ensures safe inheritance and robust object-oriented design.


















