# Bots.Business Unofficial API — Extracted from VSCode Extension

> Source: extension/dist/extension.js (de-minified). Base URL: `https://appapi.bots.business/v1/` with `?api_key=YOUR_KEY` appended to every request.

## Auth
- **Query param**: `?api_key=YOUR_KEY`
- No Authorization header used by the extension.

## Endpoints

### `bots`
- Methods: **GET**
- Notes: List your bots.

### `bots/`
- Methods: **POST**
  - POST body example: `{ name:e,token:a}`
- Notes: Create a new bot.

### `bots/installed`
- Methods: **POST**
  - POST body example: `{ id:a.id}`

### `bots/{id}`
- Methods: **GET**
- Notes: Get bot details.

### `bots/{id}/commands`
- Methods: **GET, POST**
- Notes: List or create commands.

### `bots/{id}/commands/{id}`
- Methods: **GET, PUT**
- Notes: Get command details or update command.

### `bots/{id}/commands/{id}/code`
- Methods: **PUT**
  - PUT body example: `{ code:s}`
- Notes: Update command code.

### `bots/{id}/commands_folders`
- Methods: **GET, POST**
  - POST body example: `{ title:i}`
- Notes: List or create command folders.

### `bots/{id}/libs/`
- Methods: **POST**
  - POST body example: `{ lib_id:String(o.id)}`
- Notes: Install a library to a bot.

### `bots/{id}/libs/{id}`
- Methods: **DELETE**
- Notes: Uninstall a library from a bot.

### `bots/{id}/status`
- Methods: **POST**
  - POST body example: `{ status:"start"===a?"start_launch":"start_stopping"}`
- Notes: Start/stop a bot.

### `store/collections/`
- Methods: **GET**
- Notes: List store collections.

### `store/collections/{id}/bots`
- Methods: **GET**
- Notes: List bots available in a store collection.

### `store/libs/`
- Methods: **GET**
- Notes: Install a library to a bot.

### `user/unsecure`
- Methods: **GET**
- Notes: Get user info by API key.

## Full URL Examples

- `GET https://appapi.bots.business/v1/bots?api_key=YOUR_KEY`
- `POST https://appapi.bots.business/v1/bots/?api_key=YOUR_KEY`
- `GET https://appapi.bots.business/v1/bots/{id}?api_key=YOUR_KEY`
- `POST https://appapi.bots.business/v1/bots/{id}/status?api_key=YOUR_KEY`
- `POST https://appapi.bots.business/v1/bots/{id}/commands?api_key=YOUR_KEY`
- `PUT https://appapi.bots.business/v1/bots/{id}/commands/{id}/code?api_key=YOUR_KEY`
- `GET https://appapi.bots.business/v1/store/collections/?api_key=YOUR_KEY`
- `GET https://appapi.bots.business/v1/store/collections/{id}/bots?api_key=YOUR_KEY`
- `GET https://appapi.bots.business/v1/store/libs/?api_key=YOUR_KEY`
- `GET https://appapi.bots.business/v1/user/unsecure?api_key=YOUR_KEY`