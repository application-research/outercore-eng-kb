# runDealWorker

```html
func (cm *ContentManager) runDealWorker(ctx context.Context) {
	// run the deal reconciliation and deal making worker
	for {
		select {
		case c := <-cm.ToCheck:
			log.Debugf("checking content: %d", c)

			var content util.Content
			if err := cm.DB.First(&content, "id = ?", c).Error; err != nil {
				log.Errorf("finding content %d in database: %s", c, err)
				continue
			}

			err := cm.ensureStorage(context.TODO(), content, func(dur time.Duration) {
				cm.queueMgr.add(content.ID, dur)
			})
			if err != nil {
				log.Errorf("failed to ensure replication of content %d: %s", content.ID, err)
				cm.queueMgr.add(content.ID, time.Minute*5)
			}
		}
	}
}
```