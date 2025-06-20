Personal Finance Management
-----
## Targets
The main goal of this organization and its repo with the following goals in mind
  - Build a usable application
  - practical and simple usecase
  - simple and easy to use
a system to manage personal finance and solve / answer these questions.
  1. whats the monthly spend?
  2. whats the savings done towards the kid?
  3. whats the income  vs spend vs saving ratio? 
  4. How soon can i save funds for a target expenditure?
  5. Can i set some goals, like buying a car/bike etc and save towards it?
  6. Whats my monthly burn based on cateogries like
     - food
     - travel
     - home
     - entertainment
     - education

## Goals:
  - **V1** - Simple cli app to solve the above problem 

  - **V2** - Same app to support a web interface to be able to interact over the web

  - **V3** - Support for the web.app + mobile ( only if above two are successfull and have > 2 users)

  - **V4** - Polish the application, monitor it have plugin support to send out emails, alerts, etc when targets are met. Basically  have plugins as a support, ex. emails, reports, charts etc.

  - **V5** - Market it for others to use and get feedback on architecture and learn.



### Architecture for BE
Collecting workspace information# Understanding the Flow with Examples

Let's break down the architecture using two use cases:

## Use Case 1: Creating an Expense Transaction

### Flow:
1. **CLI Input** (`cli/internal/ports/cmd/`) - User enters:
```sh
finance-cli expense add -amount 50 -category food -from wallet
```

2. **Port Layer** (`cli/internal/ports/handlers/`)
- Validates input
- Converts CLI args to domain objects

3. **Application Layer** (`cli/internal/app/service/`)
- Orchestrates the flow
- Calls domain services
- Handles transaction atomicity

4. **Domain Layer** (`domain/`)
- Contains business logic
- Validates business rules
- Example: Checks if wallet has sufficient balance

5. **Adapter Layer** (`cli/internal/adapters/`)
- Implements storage interface
- Saves transaction to persistent storage

## Use Case 2: Savings Transfer

### Flow:
```sh
finance-cli transfer -from checking -to savings -amount 1000
```

### Interface Examples:

```go
// domain/repository/interfaces.go
type TransactionRepository interface {
    SaveTransaction(Transaction) error
    GetBalance(accountID string) (decimal.Decimal, error)
}

// domain/service/finance.go
type TransactionService interface {
    CreateExpense(amount, category, wallet string) error
    TransferBetweenAccounts(from, to, amount string) error
}
```

### Key Points:
1. **Domain Layer**:
   - Define core entities (Transaction, Account, Category)
   - Define repository interfaces
   - Contains business rules

2. **Ports Layer**:
   - CLI command definitions
   - Input validation
   - Request/Response DTOs

3. **Application Layer**:
   - Uses domain services
   - Orchestrates workflow
   - Transaction management

4. **Adapters Layer**:
   - Implements repository interfaces
   - Handles storage details (file/DB)
   - Converts domain objects to storage format