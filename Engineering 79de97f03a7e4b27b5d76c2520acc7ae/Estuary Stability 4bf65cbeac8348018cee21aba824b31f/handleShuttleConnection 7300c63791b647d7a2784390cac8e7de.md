# handleShuttleConnection

```html
for {
			var msg drpc.Message
			if err := websocket.JSON.Receive(ws, &msg); err != nil {
				log.Errorf("failed to read message from shuttle: %s, %s", shuttle.Handle, err)
				return
			}

			go func(msg *drpc.Message) {
				msg.Handle = shuttle.Handle
				s.CM.IncomingRPCMessages <- msg
			}(&msg)
		}
```