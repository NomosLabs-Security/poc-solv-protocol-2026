# Solv Protocol — BRO Vault Double-Minting Reentrancy — PoC

> **Educational Purpose Only** — This PoC is created solely for security
> research and educational purposes. It runs against a fork, not mainnet.

## Quick Start

```bash
git clone https://github.com/NomosLabs-Security/poc-solv-protocol-2026
cd poc-solv-protocol-2026
forge install foundry-rs/forge-std --no-git
forge test -vvvv
```

## Details

- **Exploit Date:** 2026-03-06
- **Chain:** Ethereum
- **Loss:** ~$2.7M (38.0474 SolvBTC)
- **Expected Output:** Double-minted BRO tokens exchanged for SolvBTC
- **Full Analysis:** [NomosLabs Security Intelligence Archive](https://nomoslabs.io/archive/solv-protocol-2026)

## Vulnerability

The BitcoinReserveOffering (BRO) contract's `mint()` function had a
double-minting flaw caused by a reentrancy via ERC-3525/ERC-721 callback.
When a user mints by transferring a full ERC-3525 NFT, `doSafeTransferIn`
triggers the `onERC721Received` callback. The attacker's contract used this
callback to re-enter `mint()` before balances were updated, violating the
Checks-Effects-Interactions (CEI) pattern.

The attacker triggered the vulnerability 22 times, inflating 135 BRO into
~567 million BRO, then exchanged them for ~38 SolvBTC (~$2.7M).

## RPC Providers

This PoC uses local mock contracts. No RPC endpoint required.

For mainnet fork reproduction, use an Ethereum RPC:

| Provider | Free Tier | URL Format |
|----------|-----------|------------|
| [Alchemy](https://www.alchemy.com/) | 300M compute units/mo | `https://eth-mainnet.g.alchemy.com/v2/YOUR_KEY` |
| [QuickNode](https://www.quicknode.com/) | 10M API credits/mo | `https://YOUR_ENDPOINT.quiknode.pro/YOUR_KEY` |

## License

MIT — For educational use only.
