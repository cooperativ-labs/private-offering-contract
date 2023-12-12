# ERC1410Standard Smart Contract

## Overview

The `ERC1410Standard` smart contract is designed for tokenization and management of assets on a blockchain. It leverages the ERC1410 standard, a part of the security token standard, allowing for partitioned ownership of tokens, which can represent real-world assets or rights.

Live at: https://staging.syndicate.cooperativ.io/

Demo videos: https://www.youtube.com/playlist?list=PLdUGBxGRPWz_n-tWwlKt_o6phKlHsR6CC

## Key Features

- **Partitioned Ownership**: Tokens are divided into partitions, enabling distinct characteristics for different groups of tokens within the same contract.
- **Issuance and Redemption of Tokens**: Allows the issuance and redemption of tokens by partitions, providing flexibility in managing the token supply.
- **Role-Based Access Control**: Different roles (like owner, manager, operator) have specific permissions for managing tokens and partitions.
- **Whitelist Management**: Addresses must be whitelisted to participate, enhancing compliance and security.
- **Event Logging**: Key actions are logged as events (e.g., `IssuedByPartition`, `RedeemedByPartition`), enabling transparency and auditability.

## Functions

### Contract Initialization

- `contractVersion`: A string indicating the version of the contract.

### Role and Ownership Checks

- `isOwner(address _account)`: Checks if an address is the contract owner.
- `isManager(address _manager)`: Checks if an address is a manager.

### Token Issuance and Redemption

- `issueByPartition(bytes32 _partition, address _tokenHolder, uint256 _value)`: Issues tokens to a specific partition.
- `operatorIssueByPartition(...)`: Similar to `issueByPartition`, but can be called by an authorized operator.
- `redeemByPartition(bytes32 _partition, uint256 _value)`: Redeems tokens from a specific partition.
- `operatorRedeemByPartition(...)`: Redeems tokens from a specific partition on behalf of a token holder.

### Whitelist Management

- `addManager(address _manager)`: Adds a new manager and whitelists them.
- `removeManager(address _manager)`: Removes a manager and updates their whitelist status.
- `removeFromWhitelist(address account)`: Removes an address from the whitelist.
- `isWhitelisted(address _address)`: Checks if an address is whitelisted.

### Utility Functions

- `totalSupplyByPartition(bytes32 _partition)`: Returns the total supply of tokens in a specific partition.
  
## Events

- `RedeemedByPartition(bytes32 indexed partition, address indexed operator, address indexed from, uint256 value)`: Emitted when tokens are redeemed.
- `IssuedByPartition(bytes32 indexed partition, address indexed to, uint256 value)`: Emitted when tokens are issued.

## Test Cases

Included are several test cases for the contract, focusing on whitelist management and role-based functions. These tests verify the functionality of adding and removing addresses from the whitelist, managing managers, and ensuring only authorized users can perform specific actions.

## License

This project is licensed under the MIT License.

---

For further information and updates, refer to the GitHub repository or the contract documentation.
