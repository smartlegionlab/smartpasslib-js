# SmartPassLib JS <sup>v1.0.2</sup>

**JavaScript implementation of deterministic smart password generator. Same secret + same length = same password across all platforms (Python, Go, Kotlin, JS).**

---

[![GitHub top language](https://img.shields.io/github/languages/top/smartlegionlab/smartpasslib-js)](https://github.com/smartlegionlab/smartpasslib-js)
[![GitHub license](https://img.shields.io/github/license/smartlegionlab/smartpasslib-js)](https://github.com/smartlegionlab/smartpasslib-js/blob/master/LICENSE)
[![GitHub release](https://img.shields.io/github/v/release/smartlegionlab/smartpasslib-js)](https://github.com/smartlegionlab/smartpasslib-js/)
[![GitHub stars](https://img.shields.io/github/stars/smartlegionlab/smartpasslib-js?style=social)](https://github.com/smartlegionlab/smartpasslib-js/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/smartlegionlab/smartpasslib-js?style=social)](https://github.com/smartlegionlab/smartpasslib-js/network/members)

---

## ⚠️ Disclaimer

**By using this software, you agree to the full disclaimer terms.**

**Summary:** Software provided "AS IS" without warranty. You assume all risks.

**Full legal disclaimer:** See [DISCLAIMER.md](https://github.com/smartlegionlab/smartpasslib-js/blob/master/DISCLAIMER.md)

---

## Core Principles

- **Deterministic Generation**: Same secret + same length = same password, every time
- **Zero Storage**: Passwords exist only when generated, never stored
- **Cross-Platform**: Compatible with Python, Go, Kotlin implementations
- **Client-Side Only**: All cryptographic operations happen in the browser
- **Web Crypto API**: Uses native browser cryptography

## Key Features

- **Smart Password Generation**: Deterministic from secret phrase
- **Public/Private Key System**: 30 iterations for private key, 60 for public key
- **Secret Verification**: Verify secret without exposing it
- **Random Password Generation**: Cryptographically secure random passwords
- **Authentication Codes**: Short codes for 2FA/MFA (4-20 chars)
- **No Dependencies**: Pure JavaScript, uses Web Crypto API

## Security Model

- **Proof of Knowledge**: Public keys verify secrets without exposing them
- **Deterministic Certainty**: Mathematical certainty in password regeneration
- **Ephemeral Passwords**: Passwords exist only in memory during generation
- **Local Computation**: No data leaves your browser
- **No Recovery Backdoors**: Lost secret = permanently lost passwords (by design)

---

## Research Paradigms & Publications

- **[Pointer-Based Security Paradigm](https://doi.org/10.5281/zenodo.17204738)** - Architectural Shift from Data Protection to Data Non-Existence
- **[Local Data Regeneration Paradigm](https://doi.org/10.5281/zenodo.17264327)** - Ontological Shift from Data Transmission to Synchronous State Discovery

---

## Technical Foundation

**Key derivation (same as Python/Go/Kotlin versions):**

| Key Type | Iterations | Purpose |
|----------|------------|---------|
| Private Key | 30 | Password generation (never stored) |
| Public Key | 60 | Verification (stored on server) |

**Character Set:** `abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789!@#$&*-_`

## Installation

Just copy `smartpasslib.js` to your project and include it:

```html
<script src="path/to/smartpasslib.js"></script>
```

## Quick Usage

### Generate Smart Password
```javascript
const secret = "MyCat🐱Hippo2026";
const length = 16;

const password = await SmartPassLib.generateSmartPassword(secret, length);
console.log(password); // "wcJjBKIhsgV%!6Iq"
```

### Generate Public/Private Keys
```javascript
const secret = "MyCat🐱Hippo2026";

const publicKey = await SmartPassLib.generatePublicKey(secret);
const privateKey = await SmartPassLib.generatePrivateKey(secret);

console.log('Public Key (store on server):', publicKey);
console.log('Private Key (never store):', privateKey);
```

### Verify Secret Against Public Key
```javascript
const secret = "MyCat🐱Hippo2026";
const storedPublicKey = "..."; // from server

const isValid = await SmartPassLib.verifySecret(secret, storedPublicKey);
if (isValid) {
    const password = await SmartPassLib.generateSmartPassword(secret, 16);
}
```

### Generate Random Passwords
```javascript
// Strong random (cryptographically secure)
const strong = await SmartPassLib.generateStrongPassword(20);

// Base random
const base = await SmartPassLib.generateBasePassword(16);

// Authentication code (4-20 chars)
const code = await SmartPassLib.generateCode(8);
```

## API Reference

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `VERSION` | string | Library version |
| `CHARS` | string | Character set used for generation |
| `PRIVATE_ITERATIONS` | number | 30 iterations for private key |
| `PUBLIC_ITERATIONS` | number | 60 iterations for public key |

### Methods

| Method | Parameters | Returns | Description |
|--------|------------|---------|-------------|
| `generatePrivateKey(secret)` | secret: string | Promise\<string\> | Private key (30 iterations) |
| `generatePublicKey(secret)` | secret: string | Promise\<string\> | Public key (60 iterations) |
| `verifySecret(secret, publicKey)` | secret, publicKey | Promise\<boolean\> | Verify secret matches public key |
| `generateSmartPassword(secret, length)` | secret, length | Promise\<string\> | Deterministic password |
| `generateStrongPassword(length)` | length | Promise\<string\> | Cryptographically random |
| `generateBasePassword(length)` | length | Promise\<string\> | Simple random password |
| `generateCode(length)` | length | Promise\<string\> | Short code (4-20 chars) |

### Input Validation

| Parameter | Minimum | Maximum |
|-----------|---------|---------|
| Secret phrase | 12 chars | unlimited |
| Password length | 12 chars | 1000 chars |
| Code length | 4 chars | 20 chars |

## Security Requirements

### Secret Phrase
- **Minimum 12 characters** (enforced)
- Case-sensitive
- Use mix of: uppercase, lowercase, numbers, symbols, emoji, or Cyrillic
- Never store digitally
- **NEVER use your password description as secret phrase**

### Strong Secret Examples
```
✅ "MyCat🐱Hippo2026"        — emoji + mixed case + numbers
✅ "P@ssw0rd!LongSecret"     — special chars + numbers + length
✅ "КотБегемот2026НаДиете"   — Cyrillic + numbers
```

### Weak Secret Examples (avoid)
```
❌ "password"                — dictionary word, too short
❌ "1234567890"              — only digits, too short
❌ "qwerty123"               — keyboard pattern
```

## Cross-Platform Compatibility

SmartPassLib JS produces **identical passwords** to:

| Platform   | Repository                                                                                                                |
|------------|---------------------------------------------------------------------------------------------------------------------------|
| Python     | [smartpasslib](https://github.com/smartlegionlab/smartpasslib)                                                            |
| JavaScript | [smartpasslib-js](https://github.com/smartlegionlab/smartpasslib-js)                                                      |
| Kotlin     | [smartpasslib-kotlin](https://github.com/smartlegionlab/smartpasslib-kotlin)                                              |
| Go         | [smartpasslib-go](https://github.com/smartlegionlab/smartpasslib-go)                                                      |
| Web        | [Web Manager](https://github.com/smartlegionlab/smart-password-manager-web)                                               |
| Android    | [Android Manager](https://github.com/smartlegionlab/smart-password-manager-android)                                       |
| Desktop    | [Desktop Manager](https://github.com/smartlegionlab/smart-password-manager-desktop)                                       |
| CLI        | [CLI PassMan](https://github.com/smartlegionlab/clipassman) / [CLI PassGen](https://github.com/smartlegionlab/clipassgen) |

## Testing

Open `test.html` in your browser to run the test suite.

## Ecosystem

**Core Libraries:**
- **[smartpasslib](https://github.com/smartlegionlab/smartpasslib)** - Python implementation
- **[smartpasslib-js](https://github.com/smartlegionlab/smartpasslib-js)** - JavaScript implementation
- **[smartpasslib-kotlin](https://github.com/smartlegionlab/smartpasslib-kotlin)** - Kotlin implementation
- **[smartpasslib-go](https://github.com/smartlegionlab/smartpasslib-go)** - Go implementation

**Applications:**
- **[Desktop Manager](https://github.com/smartlegionlab/smart-password-manager-desktop)** - Cross-platform desktop app
- **[CLI PassMan](https://github.com/smartlegionlab/clipassman)** - Console password manager
- **[CLI PassGen](https://github.com/smartlegionlab/clipassgen)** - Console password generator
- **[Web Manager](https://github.com/smartlegionlab/smart-password-manager-web)** - Web interface
- **[Mobile/Andorid manager](https://github.com/smartlegionlab/smart-password-manager-android)** - Mobile/Andorid app

## License

**[BSD 3-Clause License](LICENSE)**

Copyright (©) 2026, [Alexander Suvorov](https://github.com/smartlegionlab)

## Author

**Alexander Suvorov** - [GitHub](https://github.com/smartlegionlab)

---

## Support

- **Issues**: [GitHub Issues](https://github.com/smartlegionlab/smartpasslib-js/issues)
- **Documentation**: This [README](README.md)

---

# Test Suite

![Test Suite](https://github.com/smartlegionlab/smartpasslib-js/blob/master/data/images/logo.png)

---

