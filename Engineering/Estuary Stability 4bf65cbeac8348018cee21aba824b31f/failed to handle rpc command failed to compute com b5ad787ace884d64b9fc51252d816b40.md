# failed to handle rpc command: failed to compute commP

```html
{"level":"error","ts":"2022-08-27T01:03:42.747Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacedgsu55prhvgme34k6jrjiaux7we7lhxtav6it7mcowx36nx6jxxg: blockstore: block not found","app_version":"v0.1.7"}
```

GH project item: [https://github.com/orgs/application-research/projects/7/views/5](https://github.com/orgs/application-research/projects/7/views/5)

## Discussions

Neel’s first investigation:

- “failed to compute commP” is thrown here: [https://github.com/application-research/estuary/blob/21d596246f13354751f449f37ad7da56e69caabc/cmd/estuary-shuttle/rpc.go#L208](https://github.com/application-research/estuary/blob/21d596246f13354751f449f37ad7da56e69caabc/cmd/estuary-shuttle/rpc.go#L208)
    - “block not found” seems to eventually be coming from dagstore and/or go-ipfs-blockstore
        - dagstore raising ErrBlockNotFound: [https://github.com/filecoin-project/dagstore/blob/f9e7b7b4594221c8a4840a1e9f3f6e003c1b4c52/indexbs/indexbacked_bs.go#L128-L165](https://github.com/filecoin-project/dagstore/blob/f9e7b7b4594221c8a4840a1e9f3f6e003c1b4c52/indexbs/indexbacked_bs.go#L128-L165)
        - go-ipfs-blockstore raising ErrNotFound: [https://github.com/ipfs/go-datastore/search?q=ErrNotFound](https://github.com/ipfs/go-datastore/search?q=ErrNotFound)
            - relevant for 1st throwing case of dagstore ([L129](https://github.com/filecoin-project/dagstore/blob/f9e7b7b4594221c8a4840a1e9f3f6e003c1b4c52/indexbs/indexbacked_bs.go#L129))
- commP meaning: [https://spec.filecoin.io/](https://spec.filecoin.io/) (search for commP)
    - similar to a cid but for a piece. when negotiating a deal with a SP, is verification that the file being stored is equivalent to the file proposed
- initial theory (with limited understanding):
    - the error may be correctly representing the current state, but we’re preventing any retries from succeeding by memoizing the error
        - memo lib memoizing the error: [https://github.com/whyrusleeping/memo/blob/master/memo.go#L70-L79](https://github.com/whyrusleeping/memo/blob/master/memo.go#L70-L79)
    - solution A:
        - identify a potential race condition leading to commP computation being attempted before bs is updated
    - solution B:
        - prevent this error case from being memoized (i.e. only memoize deterministic errors) so that it may be re-attempted and potentially succeed

## Related PRs

## Notes

There is a function that computes for the commitment piece but this may very well be just a communication / transmission error.

What I'm actually concerned here is the the process seem to retry without any interval or "guard" so it just keeps on retrying. We do intend to re-try each but it just keeps re-trying even if the CID doesn't exist anymore on the blockstore.

There are several instance of that error on the logs:

```html
{"level":"error","ts":"2022-08-27T01:03:42.747Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacedgsu55prhvgme34k6jrjiaux7we7lhxtav6it7mcowx36nx6jxxg: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:42.754Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacedgsu55prhvgme34k6jrjiaux7we7lhxtav6it7mcowx36nx6jxxg: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:42.762Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacedece6333jyfvzbmi2qmizdojrpbhl6qrni5ra3omhvnplflnirui: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:42.770Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacedece6333jyfvzbmi2qmizdojrpbhl6qrni5ra3omhvnplflnirui: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:42.808Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacebquoa2vk2636ljimyiqaynyzyp4ttn6ktvmxgr3o22rr6fzbkdeg: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:42.810Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzaced3gnd5cfx4l3pe5i2czzszrh6333o5cyncmfmuaex4esl57pqo5i: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:42.819Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzaceaoumnn3l2qdsnyi566z6qs52gpy7hfwxajhnyr5nlcd376emuapm: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:42.834Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzaceb5jl3ewmpfn44ed5v4jmwzprwoxfcdmvp4l5syhby5xysx2r4c7c: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.020Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacebjjn23evne6spktqykwx55pbjyybol6agaeyabygrhkzpiwef4fy: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.117Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacebb6utpb6ojv23nm3nsktxv2hkybrcz6jmajdjv7bwl2ke6tbiio6: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.119Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacecjbanpvqlc77crdbbjuwzlo5x5n6ijifev3h6qqwhtbat6ovzfvw: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.134Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacecvrhobwzkfl3c3e5qy5uokewcxeqtyhdwtc2fyzp6ig2mk76itfw: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.135Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacedyetikuu7r5xj6ij26bihvvy6ayhe3emrlgqn3vrewa4a4adqyik: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.143Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacebq5jd3hc7ucxshfraz5t7wiqjgpiwzdj2cmaf32vba33wc7d5ycm: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.151Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacedickzx2fw7rvfjktsyarzdch5kg7h4gbzcpg2ahge2hmbv7nlxri: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.324Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacebtwjpzvtgbdhdpn7oysl6rapocyq4niapjr7557xpvzaxvwi6ah2: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.332Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacedbrrretu5ujuo2mi6v4j6vppnolrtbbb2sy5hpkd6vfewfvtywca: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.340Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzaceb7riwod2ktitluesvae4vriirncqawtcojm2d4anj6nk6ewxmxmu: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.348Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacecbk6wpdpsy25psfqw6lruvcuqf5sj3tktga5lb6p6fmx3cqkoas6: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.364Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacebphdnqg6erh3h4qb6rxrigc4egpoanhize3w2ocftoqwhrnrz4he: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.372Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacecnukejb2pb5hhxuz6tg22v7wm2otcy5ucc4c5bx5fegl24fwwg5o: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.373Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzaceaoqqjqheojqfew7trf3zshqa3cruhent5bm5eeuzz53efxnug7wc: error traversing node at \"Roots/0/Links/2/Hash\": could not load link \"bafk2bzacecuxs4tsnebcelgjqqjt4si7mdketu2n72l7ficx5qmb672xiewmc\": blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.388Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacebd3w6udqh3zowldai4mk5lhkwrpc4ycvab7vrpqhnrcabmuqqjii: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.394Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzaceasl2y7wybhcie2gruj74ctbahxas6ziggykac6frhq3iwhixvpyc: error traversing node at \"Roots/0\": could not load link \"bafk2bzacebrdqozn6ql5mn62jc6nlipfuemqunxvazsy4oh4gzdxogb45a72e\": blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.401Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzaceanmnj4akjocltsze3owdnwruxh2w4uxa6awiajoprkja7grzod3q: error traversing node at \"Roots/0\": could not load link \"bafk2bzacea33ix4ros66vj5ar4v5drgpmjxe4rkrx7sf5rg3brjy53ruopme2\": blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.406Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacecxcpvcv62arv3z4moe7t7rp7xup5hacanxedskop5piuw6npi5vi: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.420Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacedinuhr3ml6xunkjfsenl4olddy7if7kpqz6n5anwvbvv5utyplwu: blockstore: block not found","app_version":"v0.1.7"}
{"level":"error","ts":"2022-08-27T01:03:43.422Z","logger":"shuttle","caller":"estuary-shuttle/main.go:834","msg":"failed to handle rpc command: failed to compute commP for bafy2bzacecrzgens7tuzopkam4mbpyu7qejkyuprcbhr42fg56wwtffgiu2am: blockstore: block not found","app_version":"v0.1.7"}
```