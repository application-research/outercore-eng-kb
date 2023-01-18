# Stephane

Hi Brenda,

As you mentioned on the arg channel, I wanted to reach out to give you some feedback on Estuary.

It seems that I'm still getting a lot of deals stuck on "StorageDealWaitingForData". It seems to be for the larger deals (32GiB) as the smaller ones (4GiB, 8GiB, 16GiB) are coming through without too much problems. I'm also having some transfer trouble, but I think that's an issue related with GraphSync, which I'm hoping will go away when boost takes over.At this stage, I have about 70% failure rate with a combination of both problems above (last 50 deals, 16 were successful).

Is the size of the deals hard to deal with too? Smaller deals not as ideal?

well, i guess we would all prefer larger deals (the larger the deal, the less publishing costs we have per GiB of data), but I don't mind lots of smaller deals as I have a LOT less problems with them in terms of transfers.

More often than not, a 32GiB deal will start transferring for hours and hours, and then fail. So while they are better economically, they are a lot less reliable

here are two other issues that I have encountered, but they are a lot more random/rare:

- "deal data verification failed: proposal CommP doesn't match calculated CommP"
- "graphsync request failed to complete: remote peer is missing block"

at this stage, there are very few deals coming through from Estuary (not sure why, might be because there are a lot more SPs registered). Would love to get more deals...The only barrier is indeed the transfer problems. I also have noticed that, it's kind of a everything or nothing situation when it comes to deals from Estuary. As in, there is either very few deals coming through (like now), or suddenly it gets out of control, and I get 5/10 deals per minutes which is overwhelming for the market node. I've implemented CID gravity which helps limiting this, but I'm not sure how this affects my "score" (if there is any) within Estuary if I keep on refusing deals