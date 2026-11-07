# openmosh-relay

Optional, self-hosted push relay for [OpenMosh](https://github.com/lesquel/OpenMosh) on **iOS**.
Apple only accepts pushes signed with the app publisher's APNs key, so iOS needs a relay that
forwards the hook's **already encrypted** payload to APNs. Android does not need this component.

> Status: **Phase 2**. Not started.

## Guarantees

- Receives ciphertext only. It cannot read notification content.
- Stores only `topic → APNs device token`.
- Does not log payloads.

## Architecture

Clean Architecture by feature. See [`ARCHITECTURE.md`](./ARCHITECTURE.md).

## License

Proposed Apache-2.0 (pending confirmation).
