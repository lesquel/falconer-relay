# falconer-relay

Planned, optional, self-hosted push relay for [Falconer](https://github.com/lesquel/falconer) on **iOS**.

> **Status: planned, not started.** There is no code in this repository yet, only the design. Falconer works
> today on Android without this component.

## Why it exists

Android phones receive Falconer notifications through ntfy (UnifiedPush), with no relay. iOS only accepts pushes
signed with the app publisher's APNs key, so an iOS app would need a small server that forwards the hook's
**already encrypted** payload to Apple Push Notification service (APNs).

```text
falconer-hook ──encrypted payload──► falconer-relay ──APNs HTTP/2──► iPhone (decrypts locally)
```

## What it will do

- **Register / unregister:** map a topic to an APNs device token.
- **Deliver:** accept an opaque, end-to-end encrypted envelope for a topic and forward it to APNs.
- Nothing else: no accounts, no dashboards, no analytics.

## Guarantees

- Receives ciphertext only. It contains no decryption code and cannot read notification content.
- Stores only `topic → APNs device token`.
- Never logs payloads.

## Security requirements before implementation

These must be designed and tested before the first release:

1. **Authenticated register/unregister.** Without it, anyone could register their own device token for someone
   else's topic and receive (encrypted) pushes, or unregister another user's device. Registration must prove
   control of the topic (for example, a per-topic secret or a signature from the paired hook).
2. **Authenticated delivery.** Only the hook that owns a topic may send to it.
3. **Rate limiting** per topic and per client address on every endpoint, to protect the APNs key and the
   devices from floods.
4. **Minimal storage.** Only `topic → token`; no payloads, no IPs, no timestamps beyond what expiry needs.
5. **No payload logging**, enforced by a test that inspects log output.
6. **APNs key handling.** The `.p8` key is read from a file or secret store, never committed or logged.

## Architecture

Clean Architecture by feature. See [`ARCHITECTURE.md`](./ARCHITECTURE.md).

## Contributing and security

See [CONTRIBUTING.md](./CONTRIBUTING.md) and [SECURITY.md](./SECURITY.md). The project's
[Code of Conduct](https://github.com/lesquel/falconer/blob/main/CODE_OF_CONDUCT.md) applies here.

## License

Proposed Apache-2.0 (pending confirmation).
