# websocket.JSON.Send

```html
for {
		select {
		case <-readDone:
			return fmt.Errorf("read routine exited, assuming socket is closed")
		case msg := <-d.outgoing:
			if err := conn.SetWriteDeadline(time.Now().Add(time.Second * 30)); err != nil {
				log.Errorf("failed to set the connection's network write deadline: %s", err)

			}
			if err := websocket.JSON.Send(conn, msg); err != nil {
				log.Errorf("failed to send message: %s", err)
			}
			if err := conn.SetWriteDeadline(time.Time{}); err != nil {
				log.Errorf("failed to set the connection's network write deadline: %s", err)
			}
		}
	}
```