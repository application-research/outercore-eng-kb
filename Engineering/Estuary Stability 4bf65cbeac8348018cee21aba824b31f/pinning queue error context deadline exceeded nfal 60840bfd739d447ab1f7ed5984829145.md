# pinning queue error: context deadline exceeded\nfallback provide failed\nmain

```html
{"level":"error","ts":"2022-08-27T01:49:03.981Z","logger":"pinner","caller":"pinner/pinmgr.go:308","msg":"pinning queue error: context deadline exceeded\nfallback provide failed\nmain.(*Shuttle).Provide\n\t/home/ubuntu/estuary-rb/cmd/estuary-shuttle/main.go:1248\nmain.(*Shuttle).doPinning\n\t/home/ubuntu/estuary-rb/cmd/estuary-shuttle/main.go:1530\ngithub.com/application-research/estuary/pinner.(*PinManager).doPinning\n\t/home/ubuntu/estuary-rb/pinner/pinmgr.go:192\ngithub.com/application-research/estuary/pinner.(*PinManager).pinWorker\n\t/home/ubuntu/estuary-rb/pinner/pinmgr.go:307\nruntime.goexit\n\t/snap/go/9854/src/runtime/asm_arm64.s:1263\nfailed to provide - contID(35379216), cid(bafkreif334oyn36xd373nlff33grup6wz3rdmrarlsxtkjle5ixixnt54y)\nmain.(*Shuttle).doPinning\n\t/home/ubuntu/estuary-rb/cmd/estuary-shuttle/main.go:1531\ngithub.com/application-research/estuary/pinner.(*PinManager).doPinning\n\t/home/ubuntu/estuary-rb/pinner/pinmgr.go:192\ngithub.com/application-research/estuary/pinner.(*PinManager).pinWorker\n\t/home/ubuntu/estuary-rb/pinner/pinmgr.go:307\nruntime.goexit\n\t/snap/go/9854/src/runtime/asm_arm64.s:1263\nshuttle RunPinFunc failed\ngithub.com/application-research/estuary/pinner.(*PinManager).doPinning\n\t/home/ubuntu/estuary-rb/pinner/pinmgr.go:202\ngithub.com/application-research/estuary/pinner.(*PinManager).pinWorker\n\t/home/ubuntu/estuary-rb/pinner/pinmgr.go:307\nruntime.goexit\n\t/snap/go/9854/src/runtime/asm_arm64.s:1263"}
```

## Discussions

## Related PRs