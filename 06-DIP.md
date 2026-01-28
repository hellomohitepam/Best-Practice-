# Dependency Inversion Principle (DIP)

## Dependency Inversion Principle (DIP)

> **High-level modules should not depend on low-level modules.**  
> **the principle encourages you to rely on interfaces or abstract classes to decouple your code and make it easier to extend, maintain, and test.**

### What this means
- Business logic must not depend on technical details
- Technical details should depend on business rules
- Communication happens through **interfaces (abstractions)**

## High-Level vs Low-Level Modules

### High-Level Module
- Contains **business rules / use-case logic**
- Describes **what the system does**
- Should not care **how** things are implemented

**Example:**
- AccountService
- OrderProcessor
- PaymentUseCase

### Low-Level Module
- Handles **technical details**
- Interacts with **external systems**
- Describes **how** things are done

**Example:**
- SQL Repository
- File System
- Email / SMS Service
- External APIs

---

```java
class MySQLDatabase {  // Low-level module
    public void saveToSQL(String data) {
        System.out.println(
            "Executing SQL Query: INSERT INTO users VALUES('" 
            + data + "');"
        );
    }
}

class MongoDBDatabase {  // Low-level module
    public void saveToMongo(String data) {
        System.out.println(
            "Executing MongoDB Function: db.users.insert({name: '" 
            + data + "'})"
        );
    }
}

class UserService {  // High-level module (Tightly coupled)
    private final MySQLDatabase sqlDb = new MySQLDatabase();      
    private final MongoDBDatabase mongoDb = new MongoDBDatabase();

    public void storeUserToSQL(String user) {
        // MySQL-specific code
        sqlDb.saveToSQL(user);
    }

    public void storeUserToMongo(String user) {
        // MongoDB-specific code
        mongoDb.saveToMongo(user);
    }
}

public class DIPViolated {
    public static void main(String[] args) {
        UserService service = new UserService();
        service.storeUserToSQL("Aditya");
        service.storeUserToMongo("Rohit");
    }
}

```
in the above code if we need to use other database to store the data then we need to modify the class which breaks the OCP as well as DIP
but in the below code we are using an interface to interact between the HLM & LLM, just passing different refernce will make the things works.

Dependency injection: when we pass the object as variable in a class

if OCP is target then DIP is the solution

```java
// Abstraction (Interface)
interface Database {
    void save(String data);
}

// MySQL implementation (Low-level module)
class MySQLDatabase implements Database {
    @Override
    public void save(String data) {
        System.out.println(
            "Executing SQL Query: INSERT INTO users VALUES('" 
            + data + "');"
        );
    }
}

// MongoDB implementation (Low-level module)
class MongoDBDatabase implements Database {
    @Override
    public void save(String data) {
        System.out.println(
            "Executing MongoDB Function: db.users.insert({name: '" 
            + data + "'})"
        );
    }
}

// High-level module (Now loosely coupled via Dependency Injection)
class UserService {
    private final Database db;

    public UserService(Database database) {
        this.db = database;
    }

    public void storeUser(String user) {
        db.save(user);
    }
}

public class DIPFollowed {
    public static void main(String[] args) {
        MySQLDatabase mysql = new MySQLDatabase();
        MongoDBDatabase mongodb = new MongoDBDatabase();

        UserService service1 = new UserService(mysql);
        service1.storeUser("Aditya");

        UserService service2 = new UserService(mongodb);
        service2.storeUser("Rohit");
    }
}

```
