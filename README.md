# Private Offering Smart Contracts

### Overview

Syndicate, developed by Cooperativ Labs Inc., is a solution for launching and managing private security offerings on Ethereum and Polygon. It streamlines the investment process, enabling efficient management of syndication deals. Learn more at Cooperativ.io.
- Live at: https://staging.syndicate.cooperativ.io/
- Demo videos: https://www.youtube.com/playlist?list=PLdUGBxGRPWz_n-tWwlKt_o6phKlHsR6CC
- 
### Contents
- [ERC1410 Share Token](https://github.com/cooperativ-labs/private-offering-contract/tree/main?tab=readme-ov-file#erc1410standard-smart-contract)
- [Swap Contract](https://github.com/cooperativ-labs/private-offering-contract/tree/main?tab=readme-ov-file#swapcontract-for-erc1410-shares-with-erc20-or-eth-payments)
- [Distribution Contract](https://github.com/cooperativ-labs/private-offering-contract/blob/main/README.md#dividends-distribution-contract)

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

## Dividends Distribution Contract

### Overview

`DividendsDistribution` is a smart contract designed for distributing dividends among shareholders of an ERC1410 token. It supports dividend distribution in both ERC20 tokens and Ether (ETH), ensuring a flexible and efficient process for dividend allocation and claiming.

### Features

- **Dividend Management**: Create and manage dividend payouts for different partitions of ERC1410 shares.
- **Flexible Payout Options**: Support for ERC20 token or ETH dividends.
- **Claiming Process**: Shareholders can claim their dividends after the payout date.
- **Dividend Recycling**: Unclaimed dividends can be reclaimed by the owner after a specified time.
- **Shareholder Tracking**: Tracks shareholders' claims and balances.

### Structs

- `Dividend`: Stores data about each dividend, including partition, payout details, amount, and claim status.

### Public Variables

- `contractVersion`: Indicates the version of the contract.
- `sharesToken`: Reference to the ERC1410 token whose shareholders are eligible for dividends.
- `reclaim_time`: Time after which unclaimed dividends can be reclaimed.
- `balances`: Tracks the balance of each payout token within the contract.
- `claimedAmount`: Records the amount of dividends claimed by each shareholder.
- `dividends`: An array of all dividends.

### Events

- `DividendDeposited`: Emitted when a dividend is deposited.
- `DividendClaimed`: Emitted when a dividend is claimed.
- `DividendRecycled`: Emitted when unclaimed dividends are recycled.

### Modifiers

- `onlyOwnerOrManager`: Ensures that only the owner or manager can perform certain actions.

### Key Functions

#### Dividend Deposit

- `depositDividend`: Deposits a dividend for a specific partition. Returns the index of the created dividend.

#### Dividend Claiming

- `claimDividend`: Allows shareholders to claim their portion of a dividend.
- `reclaimDividend`: Enables the owner to reclaim unclaimed dividends after the reclaim time.

#### Utility Functions

- `getClaimableAmount`: Returns the amount of dividend claimable by a specific address.
- `hasClaimedDividend`: Checks if an address has claimed a specific dividend.


### Test Cases

Included are several test cases for the contract, focusing on whitelist management and role-based functions. These tests verify the functionality of adding and removing addresses from the whitelist, managing managers, and ensuring only authorized users can perform specific actions.


---

For further information and updates, refer to the GitHub repository or the contract documentation.
