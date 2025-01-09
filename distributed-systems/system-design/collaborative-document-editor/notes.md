can be eventually consistent, just need to update quickly (<200ms)

For websockets, just use "WS"
WS /docs/{docID}
	SEND {
		type: "insert",
		...
	}
	SEND {
		type: "updateCursor",
		position: ...
	}
	SEND {
		type: "delete",
		...
	}
	RECEIVE {
		type: "update",
		...
	}

Consistency for concurrent users
- we obviously can't send entire snapshots per edit
- we'll send just the edits like INSERT(index, content) and DELETE(index) but this is also contextual
- we have two options
	1. Operational Transformation (OT)
		1. we just "transform" the request based on which is processed first by adjusting the offset
		2. problem is that it's tricky to maintain ordering and can only scale to a few collaborators
		3. google docs uses this
	2. Conflict-free Replicated Data Types (CRDTs)
		1. make every edit commutative (in any order) by making the text indices a real number (absolute) and use tombstones for deleted text
		2. don't need central server and more easily adaptable to offline use-case
		3. memory usage is higher because it remembers every edit. the document only ever grows in size.
		4. figma uses this

Operational transformation need to happen on both client and server.

Cursor location data is **ephemeral** (lasting for a very short time).