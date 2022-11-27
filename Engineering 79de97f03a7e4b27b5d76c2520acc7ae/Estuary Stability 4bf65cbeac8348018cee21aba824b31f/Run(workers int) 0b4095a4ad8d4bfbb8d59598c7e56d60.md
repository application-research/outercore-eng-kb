# Run(workers int)

```html
func (pm *PinManager) Run(workers int) {
	for i := 0; i < workers; i++ {
		go pm.pinWorker()
	}

	var next *PinningOperation

	var send chan *PinningOperation

	next = pm.popNextPinOp()
	if next != nil {
		send = pm.pinQueueOut
	}

	for {
		select {
		case op := <-pm.pinQueueIn:
			if next == nil {
				next = op
				send = pm.pinQueueOut
			} else {
				pm.pinQueueLk.Lock()
				pm.enqueuePinOp(op)
				pm.pinQueueLk.Unlock()
			}
		case send <- next:
			pm.pinQueueLk.Lock()
			pm.activePins[next.UserId]++

			next = pm.popNextPinOp()
			if next == nil {
				send = nil
			}
			pm.pinQueueLk.Unlock()
		case op := <-pm.pinComplete:
			pm.pinQueueLk.Lock()
			pm.activePins[op.UserId]--

			if next == nil {
				next = pm.popNextPinOp()
				if next != nil {
					send = pm.pinQueueOut
				}
			}
			pm.pinQueueLk.Unlock()

		}
	}
}
```