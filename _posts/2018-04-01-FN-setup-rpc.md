---
title:  "Setting up the Stratis Full Node for development, RPC"
date:   2018-04-01 16:16:01 -0600
permalink: /install_fn/
categories: learning
post_importance: 3
---
# Setting up the Stratis Full Node for development in OSX

Clone the StratisBitcoinFullNode code from https://github.com/stratisproject/StratisBitcoinFullNode

Open the full node solution in Visual Studio Community edition.

Use the "Build all" command in Visual Studio.

Go to the Stratis.StratisD "project" within the full node solution

Go to Program.cs. This is a simple project that just starts a full node.

Go to launchsettings.json. This is a configuration file that shows several possible "dotnet run" profiles.

Let's run the StratisD full node.

Open terminal window and navigate to the where the code is stored. (Navigating in a terminal window is done with "cd" commands. Use Google to find out more if you are unfamiliar with how to use a terminal.) Go to /src/Stratis.StratisD folder within the full node code's folder. Run the command "dotnet restore", followed by "dotnet run -help". All the possible command line arguments for this applications are shown:

```
dotnet run -help
Using launch settings from /Users/user/Documents/GitHub/StratisBitcoinFullNode/src/Stratis.StratisD/Properties/launchSettings.json...
info: Stratis.Bitcoin.Configuration.NodeSettings[0]
      Usage:
       dotnet exec <Stratis.StratisD/BitcoinD.dll> [arguments]

      Command line arguments:

      -help/--help              Show this help.
      -conf=<Path>              Path to the configuration file. Default /Users/user/.stratisnode/bitcoin/Main/bitcoin.conf.
      -datadir=<Path>           Path to the data directory. Default /Users/user/.stratisnode/bitcoin/Main.
      -testnet                  Use the testnet chain.
      -regtest                  Use the regtestnet chain.
      -acceptnonstdtxn=<0 or 1> Accept non-standard transactions. Default True.
      -maxtipage=<number>       Max tip age. Default 7200.
      -connect=<ip:port>        Specified node to connect to. Can be specified multiple times.
      -addnode=<ip:port>        Add a node to connect to and attempt to keep the connection open. Can be specified multiple times.
      -whitebind=<ip:port>      Bind to given address and whitelist peers connecting to it. Use [host]:port notation for IPv6. Can be specified multiple times.
      -externalip=<ip>          Specify your own public address.
      -synctime=<0 or 1>        Sync with peers. Default 1.
      -mintxfee=<number>        Minimum fee rate. Defaults to network specific value.
      -fallbackfee=<number>     Fallback fee rate. Defaults to network specific value.
      -minrelaytxfee=<number>   Minimum relay fee rate. Defaults to network specific value.
      -bantime=<number>         Number of seconds to keep misbehaving peers from reconnecting (Default 24-hour ban).

info: Stratis.Bitcoin.Configuration.NodeSettings[0]
      -checkpoints=<0 or 1>     Use checkpoints. Default 1.
      -assumevalid=<hex>        If this block is in the chain assume that it and its ancestors are valid and potentially skip their script verification (0 to verify all). Defaults to 8c2cf95f9ca72e13c8c4cdf15c2d7cc49993946fb49be4be147e106d502f1869.

info: Stratis.Bitcoin.Configuration.NodeSettings[0]
      -txindex=<0 or 1>         Enable to maintain a full transaction index.
      -reindex=<0 or 1>         Rebuild chain state and block index from block data files on disk.
      -prune=<0 or 1>           Enable pruning to reduce storage requirements by enabling deleting of old blocks.

info: Stratis.Bitcoin.Configuration.NodeSettings[0]
      -maxmempool=<megabytes>   Maximal size of the transaction memory pool in megabytes. Defaults to 300.
      -mempoolexpiry=<hours>    Maximum number of hours to keep transactions in the mempool. Defaults to 336.
      -relaypriority=<0 or 1>   Enable high priority for relaying free or low-fee transactions.
      -limitfreerelay=<kB/minute>  Number of kB/minute at which free transactions (with enough priority) will be accepted. Defaults to 0.
      -limitancestorcount=<count>  Maximum number of ancestors of a transaction in mempool (including itself). Defaults to 25.
      -limitancestorsize=<kB>   Maximal size in kB of ancestors of a transaction in mempool (including itself). Defaults to 101.
      -limitdescendantcount=<count>  Maximum number of descendants any ancestor can have in mempool (including itself). Defaults to 25.
      -limitdescendantsize=<kB> Maximum size in kB of descendants any ancestor can have in mempool (including itself). Defaults to 101.
      -mempoolreplacement=<0 or 1>  Enable transaction replacement in the memory pool.
      -maxorphantx=<kB>         Maximum number of orphan transactions kept in memory. Defaults to 100.
      -blocksonly=<0 or 1>      Enable bandwidth saving setting to send and received confirmed blocks only. Defaults to False.
      -whitelistrelay=<0 or 1>  Enable to accept relayed transactions received from whitelisted peers even when not relaying transactions. Defaults to True.

info: Stratis.Bitcoin.Configuration.NodeSettings[0]
      -savetrxhex=<0 or 1>            Save the hex of transactions in the wallet file.

info: Stratis.Bitcoin.Configuration.NodeSettings[0]
      -mine=<0 or 1>            Enable POW mining.
      -stake=<0 or 1>           Enable POS.
      -mineaddress=<string>     The address to use for mining (empty string to select an address from the wallet).
      -walletname=<string>      The wallet name to use when staking.
      -walletpassword=<string>  Password to unlock the wallet.

info: Stratis.Bitcoin.Configuration.NodeSettings[0]
      -apiuri=<string>          URI to node's API interface. Defaults to 'http://localhost'.
      -apiport=<0-65535>        Port of node's API interface. Default: 37221.
      -keepalive=<seconds>      Keep Alive interval (set in seconds). Default: 0 (no keep alive).

info: Stratis.Bitcoin.Configuration.NodeSettings[0]
      -server=<0 or 1>          Accept command line and JSON-RPC commands. Default false.
      -rpcuser=<string>         Username for JSON-RPC connections
      -rpcpassword=<string>     Password for JSON-RPC connections
      -rpcport=<0-65535>        Listen for JSON-RPC connections on <port>. Default: 16174 or (reg)testnet: 18332
      -rpcbind=<ip:port>        Bind to given address to listen for JSON-RPC connections. This option can be specified multiple times. Default: bind to all interfaces
      -rpcallowip=<ip>          Allow JSON-RPC connections from specified source. This option can be specified multiple times.
```

Let's run the full node with default settings. Enter the command "dotnet run".

The node starts up and logs various things to the terminal about it's progress. Wait a minute for it to finish starting up, and then go to the swagger web interface to the node's API at http://localhost:37221/swagger

Click on the GET /api/Node/status to expand it, and then click on the "Try it out!" button. This interface is used to manually test the API exposed by the full node. Explore the available endpoints - you can learn a lot about what the API can do from this screen.

Go back to the terminal window and hit ctrl+C several times to cancel the running of the full node.

Let's run it again, but this time with the RPC server enabled. Run the command:

```
dotnet run -server=1 -rpcuser=user -rpcpassword=pass
```

Now the RPC server is active. Depending on how you develop your application you can decide whether to use the RPC server or the API. The API has more commands, and allows for RPC calls directly, so for many use cases it is better to use the API.