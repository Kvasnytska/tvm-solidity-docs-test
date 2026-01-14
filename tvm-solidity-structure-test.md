# Intro
- Who will need this documentation
- What TVM Solidity is
# 1. Differences from Solidity
## 1.1 Language extensions
- Attributes
- Keywords
## 1.2 Message handling
### 1.2.1 External Messages

By default, TVM contracts do not receive external messages. To enable external message handling, the following elements are required:

- the `#[ExternalMessage]` contract-level attribute  
- a replay protection attribute  
- the `externalMsg` modifier on functions that handle external messages  

> **Note**  
> The `external` and `public` keywords define only function visibility and do not enable external message handling.

---

#### `#[ExternalMessage]` Attribute

The `#[ExternalMessage]` attribute defines which headers are expected in incoming external messages. These headers are required for message validation, replay protection, and expiration handling.

The attribute is applied at the **contract level** and configures how the contract processes incoming external messages.

---

#### Example

```solidity
#[ExternalMessage(time, expire)]
#[TimeReplayProt]
contract ExampleContract {

    function receive() public externalMsg {
        // Handle external message
    }
}


In this example:

- the contract accepts external messages with time and expire headers

- replay protection is enabled using the TimeReplayProt mechanism

- expired messages are automatically rejected

#### Headers

The following headers can be specified in the #[ExternalMessage] attribute:
| Header   | Type      | Description                                                                    |
| -------- | --------- | ------------------------------------------------------------------------------ |
| `time`   | `uint64`  | Local time when the message was created. Used for replay protection.           |
| `expire` | `uint32`  | Time after which the message is considered expired and automatically rejected. |
| `pubkey` | `bytes32` | Optional public key used to verify the message signature.                      |

#### Replay protection attributes
Replay protection attributes define how the contract prevents replay attacks for external messages.
| Attribute             | Description                                                                      | Reference                                 |
| --------------------- | -------------------------------------------------------------------------------- | ----------------------------------------- |
| `#[TimeReplayProt]`   | Uses the `time` header as a timestamp for replay protection.                     | `__timeReplayProtection` in `stdlib.sol`  |
| `#[SeqnoReplayProt]`  | Uses the `time` header as a sequence number for replay protection.               | `__seqnoReplayProtection` in `stdlib.sol` |
| `#[CustomReplayProt]` | Uses the `afterSignatureCheck` hook to implement custom replay protection logic. | `afterSignatureCheck` in `stdlib.sol`     |

#### Warnings

- Missing or incorrectly configured headers may cause external messages to be rejected.
- Using external messages without replay protection may lead to replay attacks.
## 1.3 Security mechanisms
- Replay protection
- Message expiration
# 2. Limitations
## 2.1 Unsupported Solidity features
## 2.2 Errors
# 3. Migration
## 3.1 Key changes
## 3.2 Migration steps
## 3.3 Migration pitfalls
# 4. Warnings
