# s&box Networking Module

Last verified: 2026-03-27

## Key Attributes
- `[Sync]` — Replicate property from server to clients
- `[Broadcast]` — Call method on all clients (server -> all)
- `IsProxy` — Check if running on client (true) or server/authority (false)

## Patterns

**Synced property**:
```csharp
[Sync] public float Health { get; set; }
```

**Server-only logic**:
```csharp
public void TakeDamage( float amount )
{
    if ( IsProxy ) return;
    Health -= amount;
}
```

**Server-to-client RPC**:
```csharp
[Broadcast]
public void OnScoreChanged( int score )
{
    // Runs on all clients
}
```

**Network Storage v3** (persistent data):
```csharp
// Save data
await CallEndpoint( "save_data", new { key, value } );

// Load data
var doc = await GetDocument( "collection", "id" );
var values = await GetGameValues( "collection_name" );
```

## Notes
- Only sync what clients need — every `[Sync]` costs bandwidth
- Use Network Storage v3 for persistent multiplayer data, never local saves
- `IsProxy == true` means this is a client-side mirror of server state
