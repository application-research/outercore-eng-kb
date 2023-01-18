# websocket.JSON (duplicate code)

```html
go func() {
			for {
				select {
				case cmd := <-cmds:
					// Write
					err := websocket.JSON.Send(ws, cmd)
					if err != nil {
						log.Errorf("failed to write command to shuttle: %s", err)
						return
					}
				case <-done:
					return
				}
			}
		}()
```