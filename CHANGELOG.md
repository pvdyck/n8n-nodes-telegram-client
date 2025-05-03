# Changelog

All notable changes to the n8n-nodes-telegram-mtproto-client package will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.7] - 2023-06-15

### Added
- New operation: `createChannelAndInvite` - Create a new channel/supergroup and invite users to it in a single operation
  - Support for creating broadcast channels or supergroups
  - Support for inviting users by username (with @ prefix)
  - Detailed response with invitation statistics
  - Comprehensive error handling and user-friendly error messages

### Changed
- Package renamed from `n8n-nodes-telegram-client` to `n8n-nodes-telegram-mtproto-client` to better reflect the MTProto protocol implementation
- Enhanced documentation with detailed examples and usage instructions
- Improved error handling for all operations

## [0.1.6] - 2023-05-20

### Added
- Support for joining and leaving channels
- Support for getting chat members

### Fixed
- Fixed issue with message sending to channels
- Improved error handling for authentication

## [0.1.5] - 2023-04-15

### Added
- Support for sending files
- Support for forwarding messages

### Fixed
- Fixed session management issues
- Improved error handling

## [0.1.4] - 2023-03-10

### Added
- Support for getting message history
- Support for deleting messages

### Fixed
- Fixed authentication issues
- Improved error handling

## [0.1.3] - 2023-02-05

### Added
- Support for sending messages
- Support for getting user information

### Fixed
- Fixed session storage issues

## [0.1.2] - 2023-01-15

### Added
- Initial implementation of Telegram Client API
- Support for authentication with phone number

## [0.1.1] - 2023-01-01

### Added
- Initial project setup
- Basic functionality

## [0.1.0] - 2022-12-15

### Added
- Initial release
