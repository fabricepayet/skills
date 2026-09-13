# Migration slices

For a wide mechanical refactor, use **expand–contract**:

1. expand by introducing the new form beside the old while keeping the system green;
2. migrate consumers in bounded, independently green batches;
3. contract by removing the old form after every migration batch completes.

When a migration batch cannot be green alone, declare the shared integration boundary and add a final integrate-and-verify ticket. Never disguise technical preparation as user-visible value.
