---
layout: "post"
title: "Up and Running Bitcoin"
---

# __Compilation__

I followed the steps listed in [Jonatack]'s bitcoin compilation steps to build `Bitcoin` from source in Linux Machine. 

Note: In `git checkout <TAG>` step, I used `git checkout b8593616dc2`


{% highlight ruby %}
git clone https://github.com/bitcoin/bitcoin.git
cd bitcoin
./contrib/install_db4.sh `pwd` # Note the output
git checkout b8593616dc2
export BDB_PREFIX='<PATH-TO>/db4' # Use the noted output
./autogen.sh
./configure BDB_LIBS="-L${BDB_PREFIX}/lib -ldb_cxx-4.8" BDB_CFLAGS="-I${BDB_PREFIX}/include"
make -j 4
{% endhighlight %}

# __Configuration__

Before exploring the main Bitcoin block chain, we shall work on `Signet` . [Signet] is a test network similar to the main bitcoin block chain. We can initally work with the signet to get familiar with the various operations that we can perform on blockchain and can later move to the main blockchain.

In order to make `segnet` as default network. We can modify `.bitcoin/bitcoin.conf` file to 

{% highlight ruby %}
chain=segnet
{% endhighlight %}


Add `path/to/the/src` in `$PATH` variable in `.bashrc` file, so you can start and use bitcoin commands from any folder.

# __Starting Bitcoin__

Command to start bitcoin

{% highlight ruby %}
bitcoind --daemon
{% endhighlight %}

Various attributes that can be sent with bitcoind command can be obtained by

{% highlight ruby %}
bitcoind --help
{% endhighlight %}

# __bitcoin-cli__

`bitcoin-cli` is the command line tool for Bitcoin Core. Here we can perform all operations related to Bitcoin such as creating a wallet, sending coins and others. The list of all operations possible through bitcoin-cli can be obtained by passing help attribute.


{% highlight ruby %}
bitcoin-cli help
{% endhighlight %}

The output of the help command is

{% highlight ruby %}
== Blockchain ==
getbestblockhash
getblock "blockhash" ( verbosity )
getblockchaininfo
getblockcount
getblockfilter "blockhash" ( "filtertype" )
getblockhash height
getblockheader "blockhash" ( verbose )
getblockstats hash_or_height ( stats )
getchaintips
getchaintxstats ( nblocks "blockhash" )
getdifficulty
getmempoolancestors "txid" ( verbose )
getmempooldescendants "txid" ( verbose )
getmempoolentry "txid"
getmempoolinfo
getrawmempool ( verbose mempool_sequence )
gettxout "txid" n ( include_mempool )
gettxoutproof ["txid",...] ( "blockhash" )
gettxoutsetinfo ( "hash_type" hash_or_height use_index )
preciousblock "blockhash"
pruneblockchain height
savemempool
scantxoutset "action" ( [scanobjects,...] )
verifychain ( checklevel nblocks )
verifytxoutproof "proof"

== Control ==
getmemoryinfo ( "mode" )
getrpcinfo
help ( "command" )
logging ( ["include_category",...] ["exclude_category",...] )
stop
uptime

== Generating ==
generateblock "output" ["rawtx/txid",...]
generatetoaddress nblocks "address" ( maxtries )
generatetodescriptor num_blocks "descriptor" ( maxtries )

== Mining ==
getblocktemplate ( "template_request" )
getmininginfo
getnetworkhashps ( nblocks height )
prioritisetransaction "txid" ( dummy ) fee_delta
submitblock "hexdata" ( "dummy" )
submitheader "hexdata"

== Network ==
addnode "node" "command"
clearbanned
disconnectnode ( "address" nodeid )
getaddednodeinfo ( "node" )
getconnectioncount
getnettotals
getnetworkinfo
getnodeaddresses ( count "network" )
getpeerinfo
listbanned
ping
setban "subnet" "command" ( bantime absolute )
setnetworkactive state

== Rawtransactions ==
analyzepsbt "psbt"
combinepsbt ["psbt",...]
combinerawtransaction ["hexstring",...]
converttopsbt "hexstring" ( permitsigdata iswitness )
createpsbt [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount,...},{"data":"hex"},...] ( locktime replaceable )
createrawtransaction [{"txid":"hex","vout":n,"sequence":n},...] [{"address":amount,...},{"data":"hex"},...] ( locktime replaceable )
decodepsbt "psbt"
decoderawtransaction "hexstring" ( iswitness )
decodescript "hexstring"
finalizepsbt "psbt" ( extract )
fundrawtransaction "hexstring" ( options iswitness )
getrawtransaction "txid" ( verbose "blockhash" )
joinpsbts ["psbt",...]
sendrawtransaction "hexstring" ( maxfeerate )
signrawtransactionwithkey "hexstring" ["privatekey",...] ( [{"txid":"hex","vout":n,"scriptPubKey":"hex","redeemScript":"hex","witnessScript":"hex","amount":amount},...] "sighashtype" )
testmempoolaccept ["rawtx",...] ( maxfeerate )
utxoupdatepsbt "psbt" ( ["",{"desc":"str","range":n or [n,n]},...] )

== Signer ==
enumeratesigners

== Util ==
createmultisig nrequired ["key",...] ( "address_type" )
deriveaddresses "descriptor" ( range )
estimatesmartfee conf_target ( "estimate_mode" )
getdescriptorinfo "descriptor"
getindexinfo ( "index_name" )
signmessagewithprivkey "privkey" "message"
validateaddress "address"
verifymessage "address" "signature" "message"

== Wallet ==
abandontransaction "txid"
abortrescan
addmultisigaddress nrequired ["key",...] ( "label" "address_type" )
backupwallet "destination"
bumpfee "txid" ( options )
createwallet "wallet_name" ( disable_private_keys blank "passphrase" avoid_reuse descriptors load_on_startup external_signer )
dumpprivkey "address"
dumpwallet "filename"
encryptwallet "passphrase"
getaddressesbylabel "label"
getaddressinfo "address"
getbalance ( "dummy" minconf include_watchonly avoid_reuse )
getbalances
getnewaddress ( "label" "address_type" )
getrawchangeaddress ( "address_type" )
getreceivedbyaddress "address" ( minconf )
getreceivedbylabel "label" ( minconf )
gettransaction "txid" ( include_watchonly verbose )
getunconfirmedbalance
getwalletinfo
importaddress "address" ( "label" rescan p2sh )
importdescriptors "requests"
importmulti "requests" ( "options" )
importprivkey "privkey" ( "label" rescan )
importprunedfunds "rawtransaction" "txoutproof"
importpubkey "pubkey" ( "label" rescan )
importwallet "filename"
keypoolrefill ( newsize )
listaddressgroupings
listdescriptors
listlabels ( "purpose" )
listlockunspent
listreceivedbyaddress ( minconf include_empty include_watchonly "address_filter" )
listreceivedbylabel ( minconf include_empty include_watchonly )
listsinceblock ( "blockhash" target_confirmations include_watchonly include_removed )
listtransactions ( "label" count skip include_watchonly )
listunspent ( minconf maxconf ["address",...] include_unsafe query_options )
listwalletdir
listwallets
loadwallet "filename" ( load_on_startup )
lockunspent unlock ( [{"txid":"hex","vout":n},...] )
psbtbumpfee "txid" ( options )
removeprunedfunds "txid"
rescanblockchain ( start_height stop_height )
send [{"address":amount,...},{"data":"hex"},...] ( conf_target "estimate_mode" fee_rate options )
sendmany "" {"address":amount,...} ( minconf "comment" ["address",...] replaceable conf_target "estimate_mode" fee_rate verbose )
sendtoaddress "address" amount ( "comment" "comment_to" subtractfeefromamount replaceable conf_target "estimate_mode" avoid_reuse fee_rate verbose )
sethdseed ( newkeypool "seed" )
setlabel "address" "label"
settxfee amount
setwalletflag "flag" ( value )
signmessage "address" "message"
signrawtransactionwithwallet "hexstring" ( [{"txid":"hex","vout":n,"scriptPubKey":"hex","redeemScript":"hex","witnessScript":"hex","amount":amount},...] "sighashtype" )
unloadwallet ( "wallet_name" load_on_startup )
upgradewallet ( version )
walletcreatefundedpsbt ( [{"txid":"hex","vout":n,"sequence":n},...] ) [{"address":amount,...},{"data":"hex"},...] ( locktime options bip32derivs )
walletdisplayaddress bitcoin address to display
walletlock
walletpassphrase "passphrase" timeout
walletpassphrasechange "oldpassphrase" "newpassphrase"
walletprocesspsbt "psbt" ( sign "sighashtype" bip32derivs )

== Zmq ==
getzmqnotifications
{% endhighlight %}

We can get information about one specific command by adding the command after help

{% highlight ruby %}
bitcoin-cli help createwallet
{% endhighlight %}

The output of the above command will be

{% highlight ruby %}
createwallet "wallet_name" ( disable_private_keys blank "passphrase" avoid_reuse descriptors load_on_startup external_signer )

Creates and loads a new wallet.

Arguments:
1. wallet_name             (string, required) The name for the new wallet. If this is a path, the wallet will be created at the path location.
2. disable_private_keys    (boolean, optional, default=false) Disable the possibility of private keys (only watchonlys are possible in this mode).
3. blank                   (boolean, optional, default=false) Create a blank wallet. A blank wallet has no keys or HD seed. One can be set using sethdseed.
4. passphrase              (string, optional) Encrypt the wallet with this passphrase.
5. avoid_reuse             (boolean, optional, default=false) Keep track of coin reuse, and treat dirty and clean coins differently with privacy considerations in mind.
6. descriptors             (boolean, optional, default=false) Create a native descriptor wallet. The wallet will use descriptors internally to handle address creation
7. load_on_startup         (boolean, optional) Save wallet name to persistent settings and load on startup. True to add wallet to startup list, false to remove, null to leave unchanged.
8. external_signer         (boolean, optional, default=false) Use an external signer such as a hardware wallet. Requires -signer to be configured. Wallet creation will fail if keys cannot be fetched. Requires disable_private_keys and descriptors set to true.

Result:
{                       (json object)
  "name" : "str",       (string) The wallet name if created successfully. If the wallet was created using a full path, the wallet_name will be the full path.
  "warning" : "str"     (string) Warning message if wallet was not loaded cleanly.
}

Examples:
> bitcoin-cli createwallet "testwallet"
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id": "curltest", "method": "createwallet", "params": ["testwallet"]}' -H 'content-type: text/plain;' http://127.0.0.1:8332/
> bitcoin-cli -named createwallet wallet_name=descriptors avoid_reuse=true descriptors=true load_on_startup=true
> curl --user myusername --data-binary '{"jsonrpc": "1.0", "id": "curltest", "method": "createwallet", "params": {"wallet_name":"descriptors","avoid_reuse":true,"descriptors":true,"load_on_startup":true}}' -H 'content-type: text/plain;' http://127.0.0.1:8332/

{% endhighlight %}


[Jonatack]: https://jonatack.github.io/articles/how-to-compile-bitcoin-core-and-run-the-tests
[Signet]: https://en.bitcoin.it/wiki/Signet