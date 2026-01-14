# Intro
- Who will need this documentation
- What TVM Solidity is
# 1. Differences from Solidity
## 1.1 Language extensions
- Attributes
- Keywords
## 1.2 Message handling
- Message headers
- External messages
- Internal messages
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

External messages

By default, the contract does not receive external messages. To receive external messages, use ExternalMessage attribute and an attribute for replay protection. Use externalMsg modifier for functions that receive external messages.
The `#[ExternalMessage]` attribute is used to define headers for external messages in TVM-compatible Solidity contracts. These headers are required to enable message validation, replay protection, and expiration handling.

This attribute is applied at the contract level and specifies which headers are expected for incoming external messages.

---

### Syntax

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

Recommendations

Always define the expire header for external messages to prevent replay attacks. Use pubkey when message signature validation is required. Combine #[ExternalMessage] with an appropriate replay protection attribute to ensure message integrity.
Warnings

Missing or incorrectly configured headers may cause external messages to be rejected.

Using external messages without replay protection may lead to replay attacks.
