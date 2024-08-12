

https://architect.salesforce.com/1/asset/immutable/s/e6cf2ac/assets/images/Salesforce-Order-Of-Execution-Diagram.png

In Salesforce, Apex's execution context and transaction are closely related but not the same thing. Here's how they differ:

1. Execution Context:
Definition: The execution context in Salesforce refers to the environment in which your Apex code runs. It encompasses everything that occurs when a trigger, Visualforce page, batch job, or any other piece of Apex code executes.
    - Scope: An execution context may involve multiple transactions. For example, if a trigger fires multiple times due to bulk operations or recursive triggers, each trigger execution could represent a different transaction within the same overall execution context.
    - State: The state (such as static variables, governor limits, and heap size) is maintained throughout the entire execution context. This means that if multiple transactions are executed within a single execution context, they share the same governor limits.
2. Transaction:
Definition: A transaction in Salesforce is a unit of work that is either fully completed or not performed at all. It follows the ACID principles (Atomicity, Consistency, Isolation, Durability).
    - Scope: A transaction typically corresponds to a single operation or series of operations that Salesforce treats as a single, indivisible action. For example, a DML operation like an insert or update is part of a transaction.
    - State: Within a transaction, Salesforce ensures that all operations either succeed or fail together. If a transaction fails, all changes are rolled back.

Relationship:
Within an Execution Context: Multiple transactions can occur, especially in cases like recursive triggers or batch processing. Each transaction will be treated independently in terms of commit/rollback, but they share the same execution context.
    - Governor Limits: Limits such as SOQL queries, DML statements, and CPU time are shared across the entire execution context, even if multiple transactions are involved.
Example:
Single Execution Context, Multiple Transactions:
Imagine you have a trigger that runs on an update operation. If the trigger logic causes another update (like a recursive trigger), this could result in multiple transactions within the same execution context.
- Key Takeaway:
    - Execution context is the broader environment where Apex code runs, potentially encompassing multiple transactions.
    - Transaction is a specific unit of work within that context, ensuring that operations either succeed or fail as a group.


```mermaid
graph TD
    A[Execution Context] --> B[Transaction 1]
    A --> C[Transaction 2]
    A --> D[Transaction 3]

    B -->|Operation 1| E[Database Update]
    B -->|Operation 2| F[Trigger Execution]
    
    C -->|Operation 1| G[Database Insert]
    C -->|Operation 2| H[Workflow Rule]

    D -->|Operation 1| I[Database Delete]
    D -->|Operation 2| J[Email Notification]

    style A fill:#f9f,stroke:#333,stroke-width:2px;
    style B fill:#bbf,stroke:#333,stroke-width:2px;
    style C fill:#bbf,stroke:#333,stroke-width:2px;
    style D fill:#bbf,stroke:#333,stroke-width:2px;
```
