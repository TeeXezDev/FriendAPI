# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Run

```bash
python app.py
```

Server runs at `http://0.0.0.0:5050`.

## Dependencies

Install with:
```bash
pip install -r requirements.txt
```

## Architecture

Single-file Flask app (`app.py`) acting as a proxy API for Free Fire's friend management endpoints.

### Flow

1. **Client sends** `uid`, `password`, `friend_uid` → this API
2. **This API calls** a local token service (`localhost:5000/token`) to get a JWT
3. **This API crafts** an encrypted payload and proxies it to Free Fire's backend (`clientbp.ggpolarbear.com`)

### Two endpoints

- **`GET /add`** — Send friend request. Uses a custom `Encrypt_ID()` encoding on the friend UID + hardcoded hex payload structure, then AES-CBC encrypts the whole thing.
- **`GET /remove`** — Remove friend. Serializes a Protobuf message (`RemoveFriend` with `AuthorUid`/`TargetUid`), then AES-CBC encrypts it.

Both endpoints share the same auth flow (`get_token`) and request headers.

### Key details

- AES key/IV are hardcoded byte arrays in `app.py`
- JWT is decoded **without signature verification** (`options={"verify_signature": false}`)
- `ReleaseVersion` header is `OB54` (Free Fire OB build version)
- Protobuf descriptor for `RemoveFriend` is embedded as a serialized binary string in the source (line 21)
- `Encrypt_ID()` is a custom encoding that repeatedly divides by 128, not a standard algorithm
- No tests, no lint config, no CI

### Dependencies

`flask`, `requests`, `pyjwt`, `pycryptodome`, `protobuf`
