# Estuary Storage Provider Feedback

Starting around Feb 2022, we observed a trend of an increasing number of Estuary Storage Providers no longer taking deals from Estuary. We interviewed and got feedback from a group of storage providers around their experience with Estuary in March 2022; here are the top pain points in their experience: 

## Feedback Summary

- **As a storage provider, it is difficult to work with a storage client like Estuary because the *technical reliability* is lacking.**
    - Overall, low success rate makes it frustrating to keep working with Estuary
    - Recurring bugs/issues
        - **StorageDealWaitingForData**: https://github.com/application-research/estuary/issues/143
            - stuck in transferring mode, no data being transferred, generally will fail
            - **NOTE: this is the most commonly mentioned issue that SPs run into**
        - remote peer missing block: ‣
    - Unclear status update on what’s been fixed vs. not yet fixed, and when SPs can try Estuary with confidence that prior issues are resolved
    - Troubleshooting is difficult
        - Estuary specific errors are difficult to figure out (running out of cache, restarting shuttles, etc.)
        - For data transfer related issues, logs are not easy to read
        
- **As a storage provider, I care about *business profitability* of my operations, but the inconsistency of storage deals coming from Estuary makes it difficult to plan ahead for my business.**
    - Having consistent business from clients
        - difficult to estimate SP capacity relative to amount of deal requests coming in
        - in comparison, [bidbot](https://github.com/textileio/bidbot) deals are very reliable (many per day, that all succeed)
    - Either very few deals / no deals coming through, or a large influx that SP cannot handle
        - impossible to predict and prepare for
        - Prefer “offline” deals compared to online deals
            - Gives more flexibility
                - option for taking non-graphsync data transfer deals
                - option for taking deals at a later time
            - With online deals, with large influx of deals, sealing cluster can’t handle it time, and SP needs to reject deals
            - SP can’t get the data and start sealing it later, whereas this is possible with “offline” deals
    - Small sized deals are not as profitable
        - rising base fee cost makes it more appealing to work with other aggregators that mostly have 32GB size deals (Slingshot, bidbot)
        
- **As an Estuary storage provider, my perception of Estuary is that the main usage is for *testing purposes,* so I do not make deals with Estuary if I do not want to go through the trouble of debugging issues.**
    - Perception in community - used heavily for testing
    - Estuary doesn’t care that much if the deal is successful or not
    - minerX, SPX, were prepared for things to fail and clean it up
    - For production storage miners, are more unwilling to clean up things

## Interview Notes

[Stuberman](Estuary%20Storage%20Provider%20Feedback%20c443a6db9a3d4b3db8e46ad2a9d78e4a/Stuberman%20dd156fc6d46b4f879c70287d8b4e8cc5.md)

[Orjan](Estuary%20Storage%20Provider%20Feedback%20c443a6db9a3d4b3db8e46ad2a9d78e4a/Orjan%206f44aa50dcef42ec9317c78b2cc36525.md)

[Stephane](Estuary%20Storage%20Provider%20Feedback%20c443a6db9a3d4b3db8e46ad2a9d78e4a/Stephane%207cab5abb1eba48fabdb885521284a7b0.md)

[Patrick - Factor8 Solutions](Estuary%20Storage%20Provider%20Feedback%20c443a6db9a3d4b3db8e46ad2a9d78e4a/Patrick%20-%20Factor8%20Solutions%207a007157c42c4bd8b8a203fdef1ffe56.md)

 ‣ 

## Todos

- Log transfer errors (from Lotus)
    - structure logs well to make it more readable
- Pre validate deals (data size)
- Expose endpoint or tool to help give SPs insight into client side happenings
- Tagging releases + having a change log
- Queue for deals (manage sending deal loads to SPs) - release deals at a pace SPs can take
- System level aggregation instead of user level aggregation
- inconsistent APIs (json rpc vs. rest)