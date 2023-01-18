# addDatabaseTrackingContent (duplicate code)

```html
func (cm *ContentManager) addDatabaseTrackingToContent(ctx context.Context, cont uint, dserv ipld.NodeGetter, root cid.Cid, cb func(int64)) error {
	ctx, span := cm.tracer.Start(ctx, "computeObjRefsUpdate")
	defer span.End()

	ctx, cancel := context.WithCancel(ctx)
	defer cancel()

	gotData := make(chan struct{}, 1)
	go func() {
		nodata := time.NewTimer(noDataTimeout)
		defer nodata.Stop()

		for {
			select {
			case <-nodata.C:
				cancel()
			case <-gotData:
				nodata.Reset(noDataTimeout)
			case <-ctx.Done():
				return
			}
		}
	}()
```