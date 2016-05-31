# fixcli

## Overview

`fixcli` is FIX (Financial Information Exchange) Command-Line Client.

It supports Logon, Resend Request, and Logout message types.
The purpose of the client is to test basix FIX connectibity by logging into
a FIX session, creating a new order, receiving execution report, if any,
requesting retransmission of previously received message, and logging out
of the session.

## Usage

```
usage: fixcli [-h] --host HOST [-p PORT] -s SENDER_COMP_ID -t TARGET_COMP_ID
              [-e EXCHANGE] [-n SEQNUM] [-u USERNAME] [-m MODE] [-l LEVEL]

fixcli - FIX client application with limited functionality

optional arguments:
  -h, --help            show this help message and exit
  -l LEVEL, --log-level LEVEL
                        Log level (default: 0, max: 2)

arguments:
  --host HOST           FIX API Endpoint IP address
  -p PORT, --port PORT  FIX API Endpoint port number
  -s SENDER_COMP_ID, --sender SENDER_COMP_ID
                        Sending firm
  -t TARGET_COMP_ID, --target TARGET_COMP_ID
                        Receiving firm
  -e EXCHANGE, --exchange EXCHANGE
                        Trading Venue/Exchange
  -n SEQNUM, --seqnum SEQNUM
                        Starting Sequence Number
  -u USERNAME, --username USERNAME
                        Username
  -m MODE, --mode MODE  Mode of operation (default: dormant)

github: https://github.com/greenpau/fixcli
```

### Login and Receive Messages

```
fixcli --host 10.10.10.10 -p 23456 -s TRADER -t EXCHANGE -l 2
```

The above command accomplishes the following:

* Connects to FIX engine at IP address `10.10.10.10` over TCP port `23456`
* Identifies itself as `TRADER` via FIX tag `SenderCompID`
* Identifies a counterparty as `EXCHANGE` via FIX tag `TargetCompID` 
* Enables `DEBUG` logging level
* Sends `Logon` message and waits for incoming messages

Please note that the client will not respond to `Test Request` messages
by the FIX engine. Thus, at some point, the FIX engine will disconnect
the client.

### Login and Request Retransmission

```
fixcli --host 10.10.10.10 -p 23456 -s TRADER -t EXCHANGE -l 2 --mode gapfill
```

The above command uses `gapfill` mode (`--mode gapfill`). When used, the client
sends `Resend Request` message after it successfully authenticates to the
FIX engine.

### Login and Respond to Test Requests

```
fixcli --host 10.10.10.10 -p 23456 -s TRADER -t EXCHANGE -l 2 --mode keepalive
```

The `keepalive` mode enables the client to respond with `Heartbeat` message
in response to `Test Request` message by the FIX engine.

:warning: Please use `Ctrl + C` to close the client's session to the FIX engine.

## Docker

First, build the `greenpau/fixcli` image:

```
docker build -t greenpau/fixcli - < docker/alpine/Dockerfile
```

Then, run the tool directly:

```
docker run --rm greenpau/fixcli fixcli --host 10.10.10.10 -p 23456 -s TRADER -t EXCHANGE -l 2
```

Alternatively, create a container with a background process and execute commands.
This allows the preservation of the session data created by the tool.

```
docker run -d -t --name=fixcli_greenpau --privileged --cap-add all --net=host greenpau/fixcli /bin/bash
docker exec -it fixcli_greenpau /bin/bash
```

After that, run the tool:

```
bash$ fixcli --host 10.10.10.10 -p 23456 -s TRADER -t EXCHANGE -l 2
```
