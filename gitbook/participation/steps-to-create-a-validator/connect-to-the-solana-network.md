# Connect to the Solana network

## Create Vote Account

Once you’ve confirmed the network is running, it’s time to connect your validator to the network.

If you haven’t already done so, create a vote-account keypair and create the vote account on the network. If you have completed this step, you should see the “validator-vote-keypair.json” in your Solana runtime directory:

```bash
solana-keygen new -o ~/validator-vote-keypair.json
```

Create your vote account on the blockchain:

```bash
solana create-vote-account ~/validator-vote-keypair.json ~/validator-keypair.json
```

## Connect Your Validator

Connect to the Tour de SOL cluster by running:

```bash
export SOLANA_METRICS_CONFIG="host=https://tds-metrics.solana.com:8086,db=tds,u=tds_writer,p=dry_run"
```

```bash
solana-validator --identity ~/validator-keypair.json --voting-keypair ~/validator-vote-keypair.json \
    --ledger ~/validator-ledger --rpc-port 8899 --entrypoint tds.solana.com:8001 \
    --limit-ledger-size
```

To capture the console logging to a file, append the following string to the previous command when launching your validator. Be aware that this will consume disk space in the current directory.

```bash
 2>&1 | tee solana_tds-$(date --utc +%Y%m%d-%H%M%S).log
```

Confirm your validator connected to the network by running:

```bash
solana-gossip --entrypoint tds.solana.com:8001 spy
```

This command will display all the nodes that are visible to the TdS network’s entrypoint. If your validator is connected, its public key and IP address will appear in the list.

