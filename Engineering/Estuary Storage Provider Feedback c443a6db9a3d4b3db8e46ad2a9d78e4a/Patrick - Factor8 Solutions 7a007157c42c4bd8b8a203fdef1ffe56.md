# Patrick - Factor8 Solutions

not doing estuary anymore - to much fails at the beginning of the year ---> too much work to clean it up on the miner side

Failures that were encountered the most

- most are connected to the push transfers estuary uses. transfers not starting, transfers stopping/stalling half way, transfers completing and then missing something on estuaries side to move on... you know your problems... - please note that i do not say "transfers failing" as there is no way to tell if a stalled/stopped transfer is actually failed, might/will restart/just on hold, miner or estuary sides. that leads us to:

cleaning up on miner side

- there is no way to automate this properly without going into guessing games of "this looks stalled" and applying made up metrics to make a decision ("17 minutes no data send --> declare failed" etc.). what would help to know what exactly to expect from estuary - for example how log the window is between "making the deal, initiating the transfer" and "data is started being send" (in this particular case we normally killed every transfer that didn't send any data after 6h - no idea if thats a smart move or if estuarys queues are just full ?!?! )
that leads to me (or someone i pay) sitting there at least twice a day to clean up "failed" transfers, files that never got used, etc. - in the end it costs me money. textiles dealbot i can run for weeks without looking at it at all for example, way "cheaper" to operate

to sum it up: the biggest problem is not knowing if a deal/transfer is ultimately failed.

- this is all no real problem while we are alpha testing filecoin and tools around it - but at some point this will get more serious and then this is a situation that is not acceptable. deal handling is a terrifying blank spot in our miner control software stack. not exclusively for estuary - but for online deals in general.
- if we had a "estuary behavioral" document that we could feed a control software - a way to know if something is out of the limits to then act on. (edited)
- gimme an API call to check against if estuary considers a deal dead or not - would be an epic step forward already. at this point anyhting that helps to make a non guessed decision if a deal can be declared dead would help

well, you get the drift. this still all terribly random from the miner perspective, that makes is costly