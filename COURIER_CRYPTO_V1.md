# Courier Crypto v1

## Purpose

Carry encrypted Pocket Actions through an untrusted public transport.

## Device identity

Pocket Relay generates a long-lived ECDH P-256 key pair.

- Public key may be published as JWK.
- Private key stays local and must never appear in this repository.

## Action encryption

For each action the sender:

1. Generates a fresh ephemeral P-256 key pair.
2. Derives ECDH shared secret with Pocket Relay public key.
3. Uses HKDF-SHA-256 with random salt and info `BriarCourier/v1/ACTION`.
4. Derives an AES-256-GCM key.
5. Generates a fresh 96-bit nonce.
6. Encrypts the complete private action JSON.
7. Publishes only the envelope.

## Receive order

1. Parse envelope.
2. Validate schema and recipient key id.
3. Derive shared secret.
4. Derive AES key.
5. AES-GCM authenticated decrypt.
6. Parse inner action.
7. Replay check.
8. Expiry/target check.
9. Human review during prototype.
10. Pocket Action prehash.
11. Rollback snapshot.
12. Exact mutation.
13. Posthash.
14. Record consumed message id.
15. Produce receipt.

Any crypto/authentication failure means zero workspace mutation.

## Important limitation

Automatic encrypted return receipts are deferred until there is durable,
explicit Bracken-side key custody. A chat thread is not a durable private-key
vault.
