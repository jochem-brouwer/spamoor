# EOA Bloater

Bloat EOA state by sending incrementing wei amounts to sequential addresses. This creates a large number of unique EOA accounts with non-zero balances in a deterministic address range.

## Usage

```bash
spamoor eoa_bloater [flags]
```

## How It Works

Given a starting address and a count, the scenario sends one transaction per target address:
- Address `start + 0` receives 1 wei
- Address `start + 1` receives 2 wei
- Address `start + 2` receives 3 wei
- ...
- Address `start + N-1` receives N wei

Each target gets a unique non-zero balance, ensuring distinct EOA entries in the state trie.

## Configuration

### Base Settings (required)
- `--privkey` - Private key of the funding wallet
- `--rpchost` - RPC endpoint(s) to send transactions to

### Address Range
- `--start-address` - Starting address for sequential EOA creation (default: `0x0000000000000000000000000000000000001000`)
- `--address-count` - Number of sequential addresses to send to (default: 150000)

### Volume Control
- `-c, --count` - Total number of transactions to send (defaults to address-count)
- `-t, --throughput` - Transactions to send per slot (default: 200)
- `--max-pending` - Maximum number of pending transactions
- `--max-wallets` - Maximum number of child wallets to use

### Transaction Settings
- `--basefee` - Max fee per gas in gwei (default: 20)
- `--tipfee` - Max tip per gas in gwei (default: 2)
- `--basefee-wei` - Max fee per gas in wei (overrides --basefee for L2 sub-gwei fees)
- `--tipfee-wei` - Max tip per gas in wei (overrides --tipfee for L2 sub-gwei fees)
- `--rebroadcast` - Enable reliable rebroadcast (default: 1)
- `--timeout` - Timeout for the scenario (e.g. '1h', '30m')

### Client Settings
- `--client-group` - Client group to use for sending transactions

### Debug Options
- `--log-txs` - Log all submitted transactions

## Examples

Create 150,000 EOAs starting at address 0x1000:
```bash
spamoor eoa_bloater -p "<PRIVKEY>" -h http://rpc-host:8545 \
  --start-address 0x0000000000000000000000000000000000001000 \
  --address-count 150000
```

Create 10,000 EOAs with lower throughput:
```bash
spamoor eoa_bloater -p "<PRIVKEY>" -h http://rpc-host:8545 \
  --start-address 0x0000000000000000000000000000000000010000 \
  --address-count 10000 -t 50
```
