# handleShuttleMessages

```html
func (cm *ContentManager) handleShuttleMessages(ctx context.Context, numHandlers int) {
	for i := 1; i <= numHandlers; i++ {
		go func() {
			for {
				select {
				case <-ctx.Done():
					return
				case msg := <-cm.IncomingRPCMessages:
					if err := cm.processShuttleMessage(msg.Handle, msg); err != nil {
						log.Errorf("failed to process message from shuttle: %s", err)
					}
				}
			}
		}()
	}
}
```