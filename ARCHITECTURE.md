# PORTLG Security Monitor — Architecture

## 1. Overview

PORTLG Security Monitor is an experimental smart-contract security monitoring project designed for the Portaldot V3 Testnet.

The project combines a web dashboard, a blockchain RPC connection, and a Node.js alert server to help users review suspicious transaction patterns and monitor the status of the deployed PORTLGuardLite contract.

## 2. System Architecture

```text
                 PORTLG Security Monitor
                           |
             +-------------+-------------+
             |                           |
        Web Dashboard              Alert Server
          index.html                Node.js
             |                           |
        MetaMask Wallet           Portaldot RPC
             |                           |
             +-------------+-------------+
                           |
                  Portaldot V3 Testnet
                           |
                    PORTLGuardLite
                           |
               Telegram / Email Alerts
```

## 3. Main Components

### Web Dashboard

The dashboard provides a browser interface for connecting a wallet, viewing the contract status, and interacting with the available contract functions.

### PORTLGuardLite Smart Contract

The deployed contract provides the following functions:

- `owner()` — returns the contract owner's address.
- `paused()` — reports whether the contract is paused.
- `pause()` — allows the authorized owner to pause the contract.
- `unpause()` — allows the authorized owner to resume operations.
- `protectedAction()` — emits a security event when called while the contract is not paused.

The current `protectedAction()` function is a demonstration function. It does not execute a token swap or transfer funds.

### Node.js Alert Server

The alert server monitors blocks available through the configured Portaldot RPC endpoint. It applies configurable rules to identify patterns such as large native-token transfers, repeated transactions, watchlist activity, and certain contract interactions.

When a configured alert condition is met, the server can send a notification through Telegram or email.

### Notification Channels

Telegram and email are optional notification channels. They require their own credentials and configuration.

## 4. Data Flow

1. The dashboard connects to the user's browser wallet when requested.
2. The wallet can submit authorized transactions to the deployed contract.
3. The alert server independently polls the configured blockchain RPC for new blocks.
4. The server evaluates transactions against its configured monitoring rules.
5. Matching conditions generate alerts that may be sent to Telegram or email.
6. Users should verify important alerts against available blockchain transaction records before taking action.

## 5. Network Configuration

- Network: Portaldot V3 Testnet
- RPC endpoint: `https://test-evm-rpc-v3.portaldot.dev`
- Deployed contract: `PORTLGuardLite`
- Contract address: `0x44406CD3e7ccE420c316D015FEE74303F16629E5`
- Testnet explorer: https://scanner-testnet-v3.portaldot.dev/

## 6. Security Boundaries

The current monitoring rules provide indicators, not proof of malicious activity.

The monitor does not prevent transactions, guarantee attack detection, or automatically pause the deployed contract. The server and smart contract operate independently unless additional integration is implemented and tested.

The contract's pause mechanism only affects functions governed by its own pause logic. It does not pause unrelated applications or protocols.

## 7. Future Improvements

- Add persistent monitoring state and recovery after server restarts.
- Improve transaction and event decoding.
- Add verified on-chain event monitoring.
- Add reliable notification delivery and incident history.
- Evaluate integrations with actual protected swap or DeFi contracts.
- Add automated tests and independent security review.

## 8. Disclaimer

PORTLG Security Monitor is an experimental testnet project. It is not audited security software and must not be treated as a guarantee against financial loss or blockchain attacks.
