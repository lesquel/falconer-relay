# openmosh-relay — Architecture (Phase 2)

Clean Architecture organized by feature. Global rules: dependencies point inward (`domain` imports no framework or I/O), features never import
another feature's adapters, and wiring happens only in the composition root.

## Layout

```text
src/
├── main.rs                  # composition root: config, adapters, axum router
├── http/                    # axum handlers → application use cases
├── shared/domain/           # ids, errors
└── features/
    ├── registrations/       # register/unregister topic → APNs token
    │   ├── domain/          # Registration entity, RegistrationStore port
    │   ├── application/
    │   └── infrastructure/  # SQLite store
    └── delivery/            # forward encrypted payload to APNs
        ├── domain/          # Envelope (opaque ciphertext), PushGateway port
        ├── application/
        └── infrastructure/  # APNs HTTP/2 client
```

## Rules

1. The domain treats the payload as opaque bytes. No decryption code exists in this repo.
2. No payload logging, enforced by a test that inspects log output.
3. Same layering rules as `openmosh-hook`.
