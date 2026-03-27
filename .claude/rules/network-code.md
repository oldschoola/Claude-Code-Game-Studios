---
paths:
  - "Code/Networking/**"
---

# Network Code Rules

- Server is AUTHORITATIVE for all gameplay-critical state — check `IsProxy` before mutations
- Use `[Sync]` attribute to replicate properties from server to clients
- Use `[Broadcast]` attribute for server-to-all-clients RPC calls (multicast)
- Handle disconnection, reconnection, and host migration gracefully
- Rate-limit all network logging to prevent log flooding
- All networked values must specify replication strategy — use `[Sync]` with `SyncFlags` for frequency/priority control
- Bandwidth budget: define and track per-message-type bandwidth usage
- Security: validate all incoming data ranges on the server
- Use Network Storage v3 (via sbox.cool) for persistent data — never rely on local saves for multiplayer
- Mark properties as `[Sync]` only when needed — every synced property costs bandwidth

## Examples

**Correct** (server-authoritative with sync):

```csharp
public class PlayerScore : Component
{
    [Sync] public int Score { get; set; }
    [Sync] public string PlayerName { get; set; }

    [Broadcast]
    public void OnScoreChanged( int newScore, string reason )
    {
        // UI can listen to this to show score popup
    }
}
```

**Incorrect** (no authority check):

```csharp
public void AddScore( int amount )
{
    Score += amount; // VIOLATION: no IsProxy check — client could cheat
}
```
