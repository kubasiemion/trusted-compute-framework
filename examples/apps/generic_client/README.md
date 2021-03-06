# Generic Workload Client

This is a generic command line client intended to work with any
workload application. The intention is to get up to speed quickly
in application development by providing a generic client that works
with any worker.

## Using the Client

The command line client, `generic_client.py` sends a message to the worker.
For command line options, type `./generic_client.py -h` from this directory.

```
usage: generic_client.py [-h] [-c CONFIG] [-u URI | -a ADDRESS] [-m MODE]
                         [-w WORKER_ID] [-l WORKLOAD_ID] [-i IN_DATA]

optional arguments:
  -h, --help            show this help message and exit
  -c CONFIG, --config CONFIG
                        The config file containing the Ethereum contract
                        information
  -u URI, --uri URI     Direct API listener endpoint
  -a ADDRESS, --address ADDRESS
                        an address (hex string) of the smart contract (e.g.
                        Worker registry listing)
  -m MODE, --mode MODE  should be one of listing or registry
  -w WORKER_ID, --worker_id WORKER_ID
                        worker id (hex string) to use to submit a work order
  -l WORKLOAD_ID, --workload_id WORKLOAD_ID
                        workload id (hex string) for a given worker
  -i IN_DATA, --in_data IN_DATA
                        Input data
```

The `--mode` option is used only with an Ethereum smart contract address.
Currently only “listing” mode is supported.
This will fetch the uri from ethereum blockchain, do a worker lookup to fetch
worker details of first worker in the list, and submit work order.

If `--uri` is passed, `--mode` is not used. It will fetch the worker details
from the LMDB database and submit work order to the first available worker.
The TCF Listener uses TCP port 1947, so the URI is usually
`http://localhost:1947` .

## Examples

### Echo workload using a URI
```
./generic_client.py --uri "http://localhost:1947" \
    --workload_id "echo-result" --in_data "Hello"
```

### Heart disease eval workload using a URI
```
./generic_client.py --uri "http://localhost:1947" \
    --workload_id "heart-disease-eval" \
    --in_data "Data: 25 10 1 67  102 125 1 95 5 10 1 11 36 1"
```

### Echo workload using registry listing smart contract address
```
./generic_client.py \
    --address "0x9Be28B132aeE1b2c5A1C50529a636cEd807842cd" --mode "listing" \
    --workload_id "echo-result" --in_data "Hello"
```

