# runStagingBucketWorker

```html
func (cm *ContentManager) runStagingBucketWorker(ctx context.Context) {
	timer := time.NewTicker(cm.cfg.StagingBucket.AggregateInterval)
	for {
		select {
		case <-timer.C:
			log.Debugw("content check queue", "length", len(cm.queueMgr.queue.elems), "nextEvent", cm.queueMgr.nextEvent)

			buckets := cm.popReadyStagingZone()
			for _, b := range buckets {
				if err := cm.aggregateContent(ctx, b); err != nil {
					log.Errorf("content aggregation failed (bucket %d): %s", b.ContID, err)
					continue
				}
			}
		}
	}
}
```