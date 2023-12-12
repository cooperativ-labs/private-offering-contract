# Private Offering Smart Contracts

### Overview

Syndicate, developed by Cooperativ Labs Inc., is a solution for launching and managing private security offerings on Ethereum and Polygon. It streamlines the investment process, enabling efficient management of syndication deals. Learn more at Cooperativ.io.

Live at: https://staging.syndicate.cooperativ.io/

Demo videos: https://www.youtube.com/playlist?list=PLdUGBxGRPWz_n-tWwlKt_o6phKlHsR6CC

## ERC1410Standard Smart Contract

The `ERC1410Standard` smart contract is designed for tokenization and management of assets on a blockchain. It leverages the ERC1410 standard, a part of the security token standard, allowing for partitioned ownership of tokens, which can represent real-world assets or rights.

### Key Features

- **Partitioned Ownership**: Tokens are divided into partitions, enabling distinct characteristics for different groups of tokens within the same contract.
- **Issuance and Redemption of Tokens**: Allows the issuance and redemption of tokens by partitions, providing flexibility in managing the token supply.
- **Role-Based Access Control**: Different roles (like owner, manager, operator) have specific permissions for managing tokens and partitions.
- **Whitelist Management**: Addresses must be whitelisted to participate, enhancing compliance and security.
- **Event Logging**: Key actions are logged as events (e.g., `IssuedByPartition`, `RedeemedByPartition`), enabling transparency and auditability.

### Functions

#### Contract Initialization

- `contractVersion`: A string indicating the version of the contract.

#### Role and Ownership Checks

- `isOwner(address _account)`: Checks if an address is the contract owner.
- `isManager(address _manager)`: Checks if an address is a manager.

#### Token Issuance and Redemption

- `issueByPartition(bytes32 _partition, address _tokenHolder, uint256 _value)`: Issues tokens to a specific partition.
- `operatorIssueByPartition(...)`: Similar to `issueByPartition`, but can be called by an authorized operator.
- `redeemByPartition(bytes32 _partition, uint256 _value)`: Redeems tokens from a specific partition.
- `operatorRedeemByPartition(...)`: Redeems tokens from a specific partition on behalf of a token holder.

#### Whitelist Management

- `addManager(address _manager)`: Adds a new manager and whitelists them.
- `removeManager(address _manager)`: Removes a manager and updates their whitelist status.
- `removeFromWhitelist(address account)`: Removes an address from the whitelist.
- `isWhitelisted(address _address)`: Checks if an address is whitelisted.

#### Utility Functions

- `totalSupplyByPartition(bytes32 _partition)`: Returns the total supply of tokens in a specific partition.
  
### Events

- `RedeemedByPartition(bytes32 indexed partition, address indexed operator, address indexed from, uint256 value)`: Emitted when tokens are redeemed.
- `IssuedByPartition(bytes32 indexed partition, address indexed to, uint256 value)`: Emitted when tokens are issued.

## SwapContract for ERC1410 Shares with ERC20 or ETH Payments

### Overview

The `SwapContract` smart contract facilitates the trading of ERC1410 shares, with either ERC20 tokens or Ether (ETH) as the payment method. It allows users to create ask and bid orders for specific partitions of ERC1410 shares and manage the trading process including approvals, cancellations, and claiming proceeds.

### Key Features

- **Order Management**: Create and manage ask and bid orders for ERC1410 shares.
- **ERC20 and ETH Payments**: Support for trading with ERC20 tokens or Ether.
- **Whitelisting and Approval Controls**: Features to ensure regulatory compliance through whitelisting of participants and managerial approvals.
- **Proceeds Management**: Handling of proceeds from trades and their withdrawal.
- **Event Logging**: Key actions in the trading process are logged for transparency.

### Structs

- `Order`: Represents a trade order with details like initiator, partition, amount, price, and status.
- `Proceeds`: Tracks unclaimed ETH and ERC20 proceeds for each user.
- `status`: Indicates the current status of an order (approved, cancelled, etc.).
- `orderType`: Specifies the nature of the order (share issuance, ask/bid order, etc.).

### Public Variables

- `contractVersion`: Version of the contract.
- `shareToken`: The ERC1410 token involved in trades.
- `paymentToken`: The ERC20 token used for payment (if applicable).
- `nextOrderId`: The ID for the next order to be created.
- `swapApprovalsEnabled`: Flag to enable/disable swap approvals.
- `txnApprovalsEnabled`: Flag to enable/disable transaction approvals.

### Events

- `ProceedsWithdrawn`: Emitted when a user withdraws their proceeds.
- `OrderReset`: Emitted when an order is reset by an owner or manager.

## Modifiers

- `onlyOwnerOrManager`: Ensures that only the owner or manager of the share token can perform certain actions.
- `onlyWhitelisted`: Restricts functions to only whitelisted addresses.

### Key Functions

#### Order Management

- `initiateOrder`: Creates a new order for trading shares.
- `approveOrder`: Approves a given order, required for the order to be executable.
- `managerResetOrder`: Allows a manager to reset an order.
- `acceptOrder`: Accepts a given order, marking it ready for execution.
- `cancelAcceptance`: Cancels the acceptance of an order.
- `canFillOrder`: Checks if an order can be filled.
- `fillOrder`: Executes an order by filling it.
- `cancelOrder`: Cancels an order by its initiator.

#### Proceeds and Withdrawals

- `claimProceeds`: Allows users to claim their unclaimed proceeds.
- `UnsafeWithdrawAllProceeds`: Allows the owner to withdraw all proceeds from the contract.

#### Administrative Functions

- `toggleSwapApprovals`: Toggles the swap approval functionality.
- `toggleTxnApprovals`: Toggles the transaction approval functionality.
- `banAddress`: Prevents an address from initiating bid orders or accepting ask orders.


### Test Cases

Included are several test cases for the contract, focusing on whitelist management and role-based functions. These tests verify the functionality of adding and removing addresses from the whitelist, managing managers, and ensuring only authorized users can perform specific actions.


---

For further information and updates, refer to the GitHub repository or the contract documentation.
