# FriendAPI

Free Fire friend management API built with Flask. Supports sending friend requests and removing friends.

## Installation

```bash
pip install -r requirements.txt
```

## Run

```bash
python app.py
```

Server runs at `http://0.0.0.0:5050`

## API

### Add friend

```
GET /add?uid=<uid>&password=<password>&friend_uid=<friend_uid>
```

### Remove friend

```
GET /remove?uid=<uid>&password=<password>&friend_uid=<friend_uid>
```

### Response

```json
{
  "status": "success" | "failed",
  "code": 200 | 400 | 500
}
```

## Config

Configuration parameters in `app.py`:

- `TOKEN_API` - JWT token endpoint
- `RELEASE_VERSION` - Current release version
- `AES_KEY` / `AES_IV` - Encryption keys
