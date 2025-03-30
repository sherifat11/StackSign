# StackSign - Multi-Signature Wallet Smart Contract

## Overview
This is a **multi-signature wallet smart contract** written in **Clarity**, designed to enhance security by requiring multiple owners to sign off on transactions before execution. This ensures decentralization and mitigates risks associated with a single point of failure.

## Features
- **Multi-signature Approval:** Transactions require a minimum number of approvals before execution.
- **Proposal System:** Wallet owners can propose transactions.
- **Signature Collection:** Owners can sign proposals to approve them.
- **Automatic Execution:** Transactions are executed once the required number of signatures is met.
- **Owner Verification:** Only designated owners can propose or approve transactions.
- **Read-Only Functions:** Retrieve transaction details, signatures, and execution status.

## Contract Details
### **Constants (Errors)**
- `ERR-NOT-OWNER`: Unauthorized access.
- `ERR-INSUFFICIENT-SIGNATURES`: Not enough signatures to execute.
- `ERR-PROPOSAL-NOT-FOUND`: Proposal does not exist.
- `ERR-ALREADY-SIGNED`: Prevents duplicate signatures.
- `ERR-TRANSACTION-ALREADY-EXECUTED`: Proposal was already executed.
- `ERR-TRANSACTION-FAILED`: Transaction execution failure.

### **Data Structures**
- `owners`: A map storing wallet owners (`principal` → `bool`).
- `proposals`: A map storing proposed transactions:
  ```clarity
  (define-map proposals
    uint
    {
      signatures: (list 10 principal),
      executed: bool,
      to: principal,
      amount: uint
    }
  )
  ```

### **State Variables**
- `next-proposal-id`: Stores the next proposal ID.
- `required-signatures`: Defines the number of approvals required to execute transactions.

## Functions
### **1. Initialize Wallet**
```clarity
(define-public (initialize-wallet (wallet-owners (list 10 principal)) (min-signatures uint))
```
- Sets up wallet owners.
- Defines the required number of signatures.
- Ensures valid inputs (at least one owner and proper signature count).

### **2. Propose Transaction**
```clarity
(define-public (propose-transaction (to principal) (amount uint))
```
- Allows an owner to propose a transaction.
- Creates a new entry in `proposals`.
- Increments `next-proposal-id`.

### **3. Sign Proposal**
```clarity
(define-public (sign-proposal (proposal-id uint))
```
- Allows an owner to approve a transaction.
- Adds the owner's signature.
- If the required signatures are met, the transaction is executed automatically.

### **4. Execute Transaction (Private Function)**
```clarity
(define-private (execute-transaction (proposal-id uint))
```
- Transfers tokens once approvals reach the required threshold.
- Marks the proposal as executed.

### **5. Read-Only Functions**
Retrieve proposal details:
- `get-proposal-recipient`
- `get-proposal-amount`
- `get-proposal-signatures`
- `get-proposal-executed-status`

## Usage Example
1. **Deploy the contract.**
2. **Initialize the wallet** with owners and required signatures:
   ```clarity
   (initialize-wallet [(principal 'SP...') (principal 'SP...')] u2)
   ```
3. **Propose a transaction:**
   ```clarity
   (propose-transaction (principal 'SP...') u1000)
   ```
4. **Sign the transaction:**
   ```clarity
   (sign-proposal u0)
   ```
5. **Once enough signatures are collected, the transaction executes automatically.**

## Future Improvements
- Implement event logging.
- Add owner management (e.g., adding/removing owners).
- Introduce proposal expiration.
- Develop a frontend for interaction.

## License
This contract is open-source and can be used under the **MIT License**.
