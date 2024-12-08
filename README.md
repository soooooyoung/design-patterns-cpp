# C++ Design Patterns

## Gamma Categorization

Software design patterns are typically split into three categories called `Gamma Categorization`. This categorization originates from the "Gang of Four" (GoF) book "Design Patterns: Elements of Reusable Object-Oriented Software", which categorizes design patterns into three groups: [Creational](#creational), [Structural](#structural), and [Behavioral](#behavioral).

### Creational

Creational patterns focus on how objects are created and managed efficiently, safely, and flexibly. These patterns aim to abstract and encapsulate object creation.

#### Explicit/Implicit Object Creation:

- Explicit: Using constructors or direct calls to creation methods.
- Implicit: Leveraging dependency injection, reflection, or other mechanisms

#### Wholesale/Piecewise Construction:

- Wholesale: Creating the object fully in a single step (e.g., Singleton, Factory).
- Piecewise: Constructing an object incrementally or in parts (e.g., Builder).
  - [GroovyStyleBuilder Example](/GroovyStyle_Builder.cpp)

<details>
<summary>Example using Wholesale Construction + Explicit Creation</summary>

<br/>

> Singleton Connection Pool

```cpp
#include <iostream>
#include <memory>
#include <mutex>
#include <vector>

class ConnectionPool {
public:
    static ConnectionPool& GetInstance() {
        static ConnectionPool instance; // Thread-safe in C++11+
        return instance;
    }

    void GetConnection() {
        std::lock_guard<std::mutex> lock(mutex_);
        //..
    }

private:
    // Explicit: The constructor is private,
    // GetInstance method explicitly creates ConnectionPool
    ConnectionPool() = default;
    std::mutex mutex_;
};

int main() {
    // Wholesale: Connection pool is initialized in one step.
    ConnectionPool& pool = ConnectionPool::GetInstance();
    pool.GetConnection();
}
```

</details>

### Structural

Structural design patterns focus on object composition and class relationships. They help ensure flexible and maintainable designs by addressing how objects are organized and interact to form larger systems.

#### Adaptor Pattern

The Adapter pattern serves as a bridge between incompatible interfaces, enabling seamless interaction between old and new components.

<details> 
<summary> Example of Adaptor pattern</summary>

<br/>

> Converting legacy logging system to modern inferface

```cpp
#include <iostream>
#include <memory>
#include <string>

// Legacy Logger (incompatible interface)
class LegacyLogger {
public:
    void logMessage(const std::string& msg) {
        std::cout << "LegacyLogger: " << msg << '\n';
    }
};

// Modern Logger Interface
class ILogger {
public:
    virtual void log(const std::string& msg) = 0;
    virtual ~ILogger() = default;
};

// Adapter
class LoggerAdapter : public ILogger {
    std::shared_ptr<LegacyLogger> legacyLogger_;

public:
    LoggerAdapter(std::shared_ptr<LegacyLogger> legacyLogger)
        : legacyLogger_(std::move(legacyLogger)) {}

    void log(const std::string& msg) override {
        legacyLogger_->logMessage(msg);
    }
};

int main() {
    auto legacyLogger = std::make_shared<LegacyLogger>();
    LoggerAdapter adapter(legacyLogger);
    adapter.log("This is an adapted log message.");
}
```

</details>

### Behavioral

Behavioral design patterns focus on how objects interact and communicate within a system.

#### Observer Pattern

The Observer pattern establishes a one-to-many relationship between objects, ensuring that when one object (the subject) changes state, all its dependents (observers) are notified and updated automatically. This pattern is particularly useful in event-driven systems.

The Observer pattern is prevalent in C++ applications, especially in GUI components and event-driven systems.
