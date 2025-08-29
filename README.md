# Bots.Business API Documentation

This documentation is extracted from the provided `extension.js` file, which appears to be a VS Code extension for interacting with the Bots.Business API. The API is used for managing bots, commands, libraries, folders, and store items. All endpoints are inferred from the API calls in the code (using `apiGet`, `apiPost`, `apiPut`, `apiDelete`).

## Base URL
`https://appapi.bots.business/v1/`

## Authentication
All requests require an `api_key` query parameter:  
`?api_key=YOUR_API_KEY`  
(Obtained from user input in the extension; stored in VS Code config.)

## General Notes
- Responses are typically JSON objects (e.g., bot details, lists of items).
- Success responses return the created/updated entity (e.g., `{id: 123, name: "MyBot"}`).
- Errors may return non-200 status codes; the extension handles them by showing error messages.
- No full schema is available in the code, but examples are derived from usage.
- Endpoints are grouped by resource (e.g., Bots, Commands).
- Methods: GET (read), POST (create), PUT (update), DELETE (delete).
- Request bodies are JSON for POST/PUT.

## Endpoints

### User
#### GET /user/unsecure
- **Description**: Check user details (e.g., email) after login. Used to verify API key validity.
- **Parameters**: None (besides api_key).
- **Example Request**:  
  `GET https://appapi.bots.business/v1/user/unsecure?api_key=YOUR_API_KEY`
- **Example Response** (inferred success):  
  ```json
  {
    "email": "user@example.com"
  }
  ```
- **Error Handling**: If invalid, shows "Failed. Please check your BB Api key".

### Bots
#### GET /bots
- **Description**: List all bots for the authenticated user.
- **Parameters**: None.
- **Example Request**:  
  `GET https://appapi.bots.business/v1/bots?api_key=YOUR_API_KEY`
- **Example Response** (inferred; array of bots):  
  ```json
  [
    {
      "id": 123,
      "name": "MyBot",
      "token": "391686724:AAG6XGW0Z9NtfZwqEuWkkno_Eri932cX0Hg",
      "status": "works",
      "libs": []  // Array of installed libs
    }
  ]
  ```

#### POST /bots/
- **Description**: Create a new bot.
- **Parameters**: JSON body with `name` and `token`.
- **Example Request**:  
  `POST https://appapi.bots.business/v1/bots/?api_key=YOUR_API_KEY`  
  ```json
  {
    "name": "BBAdminBot",
    "token": "391686724:AAG6XGW0Z9NtfZwqEuWkkno_Eri932cX0Hg"
  }
  ```
- **Example Response**: The created bot object (e.g., with `id`).

#### GET /bots/:id
- **Description**: Get details of a specific bot (includes libs).
- **Parameters**: `:id` - Bot ID.
- **Example Request**:  
  `GET https://appapi.bots.business/v1/bots/123?api_key=YOUR_API_KEY`
- **Example Response** (inferred):  
  ```json
  {
    "id": 123,
    "name": "MyBot",
    "token": "391686724:AAG6XGW0Z9NtfZwqEuWkkno_Eri932cX0Hg",
    "status": "works",
    "libs": [
      {
        "id": 456,
        "name": "MyLib"
      }
    ]
  }
  ```

#### PUT /bots/:id/status
- **Description**: Update bot status (start or stop).
- **Parameters**: `:id` - Bot ID. JSON body with `status` ("start_launch" to start, "start_stopping" to stop).
- **Example Request** (Start Bot):  
  `PUT https://appapi.bots.business/v1/bots/123/status?api_key=YOUR_API_KEY`  
  ```json
  {
    "status": "start_launch"
  }
  ```
- **Example Request** (Stop Bot):  
  `PUT https://appapi.bots.business/v1/bots/123/status?api_key=YOUR_API_KEY`  
  ```json
  {
    "status": "start_stopping"
  }
  ```
- **Example Response**: Updated bot object.

#### DELETE /bots/:id
- **Description**: Delete a bot.
- **Parameters**: `:id` - Bot ID.
- **Example Request**:  
  `DELETE https://appapi.bots.business/v1/bots/123?api_key=YOUR_API_KEY`
- **Example Response**: Success confirmation (e.g., deleted bot ID or object).

#### POST /bots/installed
- **Description**: Install a bot from the store.
- **Parameters**: JSON body with `id` (store bot ID).
- **Example Request**:  
  `POST https://appapi.bots.business/v1/bots/installed?api_key=YOUR_API_KEY`  
  ```json
  {
    "id": 789
  }
  ```
- **Example Response**: Installed bot object.

### Store
#### GET /store/collections/
- **Description**: List bot collections from the store.
- **Parameters**: None.
- **Example Request**:  
  `GET https://appapi.bots.business/v1/store/collections/?api_key=YOUR_API_KEY`
- **Example Response** (inferred):  
  ```json
  [
    {
      "id": 101,
      "title": "Collection Title",
      "description": "Collection Description"
    }
  ]
  ```

#### GET /store/collections/:id/bots
- **Description**: List bots in a specific store collection.
- **Parameters**: `:id` - Collection ID.
- **Example Request**:  
  `GET https://appapi.bots.business/v1/store/collections/101/bots?api_key=YOUR_API_KEY`
- **Example Response** (inferred):  
  ```json
  [
    {
      "id": 789,
      "name": "StoreBot",
      "description": "Bot Description"
    }
  ]
  ```

#### GET /store/libs/
- **Description**: List libraries from the store.
- **Parameters**: None.
- **Example Request**:  
  `GET https://appapi.bots.business/v1/store/libs/?api_key=YOUR_API_KEY`
- **Example Response** (inferred):  
  ```json
  [
    {
      "id": 456,
      "name": "MyLib",
      "description": "Lib Description"
    }
  ]
  ```

### Libraries (Libs)
#### POST /bots/:id/libs/
- **Description**: Install a library to a bot.
- **Parameters**: `:id` - Bot ID. JSON body with `lib_id`.
- **Example Request**:  
  `POST https://appapi.bots.business/v1/bots/123/libs/?api_key=YOUR_API_KEY`  
  ```json
  {
    "lib_id": "456"
  }
  ```
- **Example Response**: Installed lib object.

#### DELETE /bots/:id/libs/:lib_id
- **Description**: Uninstall a library from a bot.
- **Parameters**: `:id` - Bot ID, `:lib_id` - Lib ID.
- **Example Request**:  
  `DELETE https://appapi.bots.business/v1/bots/123/libs/456?api_key=YOUR_API_KEY`
- **Example Response**: Success confirmation.

### Command Folders
#### GET /bots/:id/commands_folders
- **Description**: List command folders for a bot.
- **Parameters**: `:id` - Bot ID.
- **Example Request**:  
  `GET https://appapi.bots.business/v1/bots/123/commands_folders?api_key=YOUR_API_KEY`
- **Example Response** (inferred):  
  ```json
  [
    {
      "id": 202,
      "title": "Admin"
    }
  ]
  ```

#### POST /bots/:id/commands_folders
- **Description**: Create a new command folder.
- **Parameters**: `:id` - Bot ID. JSON body with `title`.
- **Example Request**:  
  `POST https://appapi.bots.business/v1/bots/123/commands_folders?api_key=YOUR_API_KEY`  
  ```json
  {
    "title": "Admin"
  }
  ```
- **Example Response**: Created folder object.

#### DELETE /bots/:id/commands_folders/:folder_id
- **Description**: Delete a command folder.
- **Parameters**: `:id` - Bot ID, `:folder_id` - Folder ID.
- **Example Request**:  
  `DELETE https://appapi.bots.business/v1/bots/123/commands_folders/202?api_key=YOUR_API_KEY`
- **Example Response**: Success confirmation.

### Commands
#### GET /bots/:id/commands
- **Description**: List commands for a bot.
- **Parameters**: `:id` - Bot ID.
- **Example Request**:  
  `GET https://appapi.bots.business/v1/bots/123/commands?api_key=YOUR_API_KEY`
- **Example Response** (inferred):  
  ```json
  [
    {
      "id": 303,
      "command": "/start",
      "commands_folder_id": null,
      "code": "/* code */",
      "answer": "Hello",
      "keyboard": "Button1",
      "need_reply": false,
      "auto_retry_time": null
    }
  ]
  ```

#### POST /bots/:id/commands
- **Description**: Create a new command.
- **Parameters**: `:id` - Bot ID. JSON body with `command` (required) and optional `commands_folder_id`.
- **Example Request** (Root Command):  
  `POST https://appapi.bots.business/v1/bots/123/commands?api_key=YOUR_API_KEY`  
  ```json
  {
  "command": "/help2",
  "aliases": "help, ❤ Help",
  "answer": "Hellow world",
  "group": "-1001390263766, -1001390263756",
  "help": null,
  "keyboard_body": "Contact, Reach out \n Support",
  "commands_folder_id": 6805501,
  "auto_retry_time": "",
  "bjs_code": "",
  "need_reply": true
  }
  ```

  **Response:**
```json
{
    "id": 65752277,
    "command": "/help2",
    "bot_id": 2646009,
    "help": null,
    "created_at": "2025-08-29T10:01:53.106Z",
    "updated_at": "2025-08-29T10:01:53.106Z",
    "answer": "Hellow world",
    "last_csv_import_at": null,
    "created_via_csv_import": null,
    "keyboard_body": "Contact, Reach out \n Support",
    "bjs_mode": null,
    "need_reply": true,
    "auto_retry_time": null,
    "last_auto_retry_at": null,
    "commands_folder_id": 6805501,
    "scenarios": [],
    "aliases": [{
        "id": 20636228,
        "command": "help",
        "bot_command_id": 65752277,
        "created_at": "2025-08-29T10:01:53.675Z",
        "updated_at": "2025-08-29T10:01:53.675Z"
    }, {
        "id": 20636229,
        "command": "❤ help",
        "bot_command_id": 65752277,
        "created_at": "2025-08-29T10:01:53.745Z",
        "updated_at": "2025-08-29T10:01:53.745Z"
    }],
    "commands_folder": {
        "id": 6805501,
        "title": "My folder",
        "bot_id": 2646009,
        "created_at": "2025-08-29T09:43:35.680Z",
        "updated_at": "2025-08-29T09:43:35.680Z"
    },
    "code": ""
}
```

#### GET /bots/:id/commands/:command_id
- **Description**: Get details of a specific command (includes code, answer, etc.).
- **Parameters**: `:id` - Bot ID, `:command_id` - Command ID.
- **Example Request**:  
  `GET https://appapi.bots.business/v1/bots/123/commands/303?api_key=YOUR_API_KEY`
- **Example Response** (inferred):  
  ```json
  {
    "id": 303,
    "bot_id": 123,
    "command": "/start",
    "code": "/* code */",
    "answer": "Hello",
    "keyboard": "Button1",
    "need_reply": false,
    "auto_retry_time": null
  }
  ```

#### PUT /bots/:id/commands/:command_id
- **Description**: Update a command (e.g., move to a different folder by changing `commands_folder_id`).
- **Parameters**: `:id` - Bot ID, `:command_id` - Command ID. JSON body with updated fields (e.g., `commands_folder_id`).
- **Example Request** (Move to Folder):  
  `PUT https://appapi.bots.business/v1/bots/123/commands/303?api_key=YOUR_API_KEY`  
  ```json
  {
    "commands_folder_id": 202
  }
  ```
- **Example Request** (Move to Root):  
  `PUT https://appapi.bots.business/v1/bots/123/commands/303?api_key=YOUR_API_KEY`  
  ```json
  {
    "commands_folder_id": null
  }
  ```
- **Example Response**: Updated command object.

#### PUT /bots/:id/commands/:command_id/code
- **Description**: Update the code of a command (e.g., after saving in VS Code).
- **Parameters**: `:id` - Bot ID, `:command_id` - Command ID. JSON body with `code`.
- **Example Request**:  
  `PUT https://appapi.bots.business/v1/bots/123/commands/303/code?api_key=YOUR_API_KEY`  
  ```json
  {
    "code": "/* Your JavaScript code here */"
  }
  ```
- **Example Response**: Updated command object or success.

#### DELETE /bots/:id/commands/:command_id
- **Description**: Delete a command.
- **Parameters**: `:id` - Bot ID, `:command_id` - Command ID.
- **Example Request**:  
  `DELETE https://appapi.bots.business/v1/bots/123/commands/303?api_key=YOUR_API_KEY`
- **Example Response**: Success confirmation.
