# n8n-nodes-telegram-mtproto-client

[![NPM Version](https://img.shields.io/npm/v/n8n-nodes-telegram-mtproto-client.svg)](https://www.npmjs.com/package/n8n-nodes-telegram-mtproto-client)
[![License](https://img.shields.io/github/license/pvdyck/n8n-nodes-telegram-mtproto-client)](https://github.com/pvdyck/n8n-nodes-telegram-mtproto-client/blob/master/LICENSE)

This is a comprehensive Telegram Client node for [n8n](https://n8n.io) that allows you to interact with Telegram as a regular user (not a bot). It provides access to advanced Telegram features that are not available through the regular bot API.

<p align="center">
  <img src="./telegram.svg" alt="Telegram Client Node" width="120" />
</p>

## Features

- 🔐 **Full user authentication** support
- 📝 **Complete message operations** (send, edit, delete, pin)
- 📸 **Media handling** (photos, videos, documents)
- 👥 **Chat management** (join, leave, history)
- 📢 **Channel creation and management**:
  - Create broadcast channels or supergroups
  - Invite users in a single operation
  - Manage channel settings and permissions
- 💾 **Session management** for persistent authentication
- ⚡ **Real-time updates** and notifications
- 🔒 **Secure credential storage**

## Installation

### Community Node (Recommended)
For n8n v0.214.0 and above:

1. Go to **Settings > Community Nodes**
2. Click on **Install a node from NPM**
3. Enter `n8n-nodes-telegram-mtproto-client`
4. Click **Install**

### Manual Installation
For n8n versions below v0.214.0 or for manual installation:

1. Install n8n with `npm install n8n -g`
2. Navigate to your n8n user data directory: `~/.n8n/` or create it if it doesn't exist
3. Install the node:

```bash
cd ~/.n8n
npm install n8n-nodes-telegram-mtproto-client
```

### From Source
If you want to install from source or contribute to development:

```bash
# Clone the repository
git clone https://github.com/pvdyck/n8n-nodes-telegram-mtproto-client.git

# Install dependencies
cd n8n-nodes-telegram-mtproto-client
npm install

# Build the node
npm run build
```

## Configuration

### Prerequisites
Before you can use this node, you need to register your application with Telegram:

1. Go to [my.telegram.org](https://my.telegram.org) and log in with your Telegram account
2. Click on **API development tools**
3. Fill in the required fields (app title, short name, etc.)
4. Click on **Create application**
5. Note down your `api_id` and `api_hash` - you'll need these for the node configuration

### Setting up Credentials in n8n
1. Go to **Credentials** in your n8n instance
2. Click **Create New Credentials**
3. Select **Telegram Client API** from the list
4. Fill in the following details:
   - **API ID**: The `api_id` from my.telegram.org
   - **API Hash**: The `api_hash` from my.telegram.org
   - **Phone Number**: Your Telegram phone number in international format (e.g., +1234567890)
   - **2FA Password**: Your Two-Factor Authentication password (if enabled on your account)
5. Click **Create** to save your credentials

## First Use Authentication
When using the node for the first time, you'll need to complete the authentication process:

1. Add the Telegram Client node to your workflow
2. Select your credentials
3. Execute the workflow
4. You'll be prompted for a verification code that Telegram sends to your account
5. Enter the code in the n8n execution modal
6. The session will be saved for future use, and you won't need to authenticate again

## Usage

### Available Operations

The node provides a wide range of operations to interact with Telegram:

#### Message Operations
- `sendMessage`: Send text messages to users, groups, or channels
- `replyToMessage`: Reply to existing messages
- `editMessage`: Edit messages you've previously sent
- `deleteMessages`: Delete messages from chats
- `forwardMessages`: Forward messages between chats
- `pinMessage`: Pin important messages in chats

#### Media Operations
- `sendMedia`: Send photos, videos, documents, or audio files
- `sendAlbum`: Send multiple media files as an album (up to 10 files)
- `downloadMedia`: Download media from messages to use in your workflow

#### Chat Operations
- `getChatHistory`: Retrieve message history from a chat
- `joinChat`: Join public channels or groups
- `leaveChat`: Leave channels or groups
- `getChats`: Get a list of your chats
- `getDialogs`: Get recent conversations
- `createChannelAndInvite`: Create a new channel/supergroup and invite users to it

### Example Workflows

#### Basic Message Sending
```json
{
  "operation": "sendMessage",
  "chatId": "@username or chat_id",
  "messageText": "Hello from n8n!",
  "parseMode": "HTML",
  "disableWebPagePreview": false,
  "disableNotification": false
}
```

#### Sending Media with Caption
```json
{
  "operation": "sendMedia",
  "chatId": "@username or chat_id",
  "filePath": "/path/to/file.jpg",
  "mediaType": "photo",
  "caption": "Check out this image!",
  "parseMode": "HTML"
}
```

#### Forwarding Messages
```json
{
  "operation": "forwardMessages",
  "fromChatId": "@source_chat",
  "toChatId": "@destination_chat",
  "messageIds": [123, 124, 125]
}
```

#### Getting Chat History
```json
{
  "operation": "getChatHistory",
  "chatId": "@username or chat_id",
  "limit": 50,
  "offsetId": 0
}
```

#### Creating a Channel and Inviting Users

The `createChannelAndInvite` operation allows you to create a new Telegram channel or supergroup and invite users to it in a single operation. This is more efficient than creating a channel and then inviting users separately.

```json
{
  "operation": "createChannelAndInvite",
  "channelTitle": "My New Channel",
  "channelDescription": "This is a channel created via n8n",
  "channelType": "supergroup",
  "usersToInvite": "@user1, @user2, @user3"
}
```

**Important Notes:**
- For user invitations, only usernames with the @ prefix are supported (e.g., @username)
- User IDs are not supported for invitations
- Users must exist on Telegram and be accessible to your account
- Some users may have privacy settings that prevent being added to groups
- You can create a channel without inviting users by leaving the `usersToInvite` field empty
- There are limits to how many channels you can create and how many users you can invite

**Response Format:**
```json
{
  "success": true,
  "channelId": 1234567890,
  "accessHash": "123456789abcdef",
  "title": "My New Channel",
  "about": "This is a channel created via n8n",
  "type": "Supergroup",
  "link": "https://t.me/c/1234567890",
  "invitedUsers": [
    {
      "id": 123456789,
      "firstName": "John",
      "lastName": "Doe",
      "username": "user1"
    }
  ],
  "invitationStats": {
    "total": 3,
    "successful": 1,
    "failed": 2
  },
  "createdAt": "2023-06-15T12:34:56.789Z"
}
```

**Common Errors:**
- `CHANNELS_TOO_MUCH`: You've reached the maximum number of channels you can create or join
- `USER_RESTRICTED`: Your account is restricted from creating new channels
- `CHAT_TITLE_EMPTY`: The channel title cannot be empty
- `CHAT_ABOUT_TOO_LONG`: The channel description is too long
- `USER_PRIVACY_RESTRICTED`: Some users could not be invited due to their privacy settings

## Error Handling and Troubleshooting

The node includes comprehensive error handling for various scenarios:

### Common Errors and Solutions

| Error | Possible Cause | Solution |
|-------|----------------|----------|
| Authentication Failed | Incorrect API ID/Hash | Double-check credentials at my.telegram.org |
| Phone Code Invalid | Incorrect verification code | Request a new code and try again |
| Session Expired | Long period of inactivity | Re-authenticate with your phone number |
| Flood Wait | Too many requests | Wait for the specified time before retrying |
| Not Authorized | Session not established | Complete the authentication process |
| Peer ID Invalid | Incorrect chat ID format | Use correct format (@username or numeric ID) |

### Security Features

- ✅ Credentials are stored securely in n8n's encrypted storage
- ✅ Session strings are encrypted to prevent unauthorized access
- ✅ Full support for Two-Factor Authentication (2FA)
- ✅ No plaintext password storage
- ✅ Automatic session management

## Development

This section is for developers who want to contribute to the node or customize it for their needs.

### Build Process
```bash
# Install dependencies
npm install

# Build the node
npm run build

# Development with auto-rebuild
npm run dev
```

### Code Quality
```bash
# Format code
npm run format

# Lint code
npm run lint

# Fix linting issues
npm run lintfix
```

## Contributing

Contributions are welcome and appreciated! Here's how you can contribute:

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

Please make sure your code follows the existing style and includes appropriate tests.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Support

If you encounter any issues or have questions:

- Create an issue on [GitHub](https://github.com/pvdyck/n8n-nodes-telegram-mtproto-client/issues)
- Check existing issues for solutions
- Contact the maintainer

## Acknowledgments

- [Telegram Client API](https://core.telegram.org/api) for providing the underlying functionality
- [n8n](https://n8n.io) team and community for the amazing workflow automation platform
- All contributors who have helped improve this node

---

<p align="center">
  <a href="https://n8n.io">
    <img src="https://raw.githubusercontent.com/n8n-io/n8n/master/assets/n8n-logo.png" alt="n8n.io - Workflow Automation" width="200" />
  </a>
</p>
