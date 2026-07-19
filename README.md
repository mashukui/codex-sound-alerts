# Codex Sound Alerts

English | [简体中文](README.zh-CN.md)

[![Version](https://img.shields.io/badge/version-v0.1.1-2563EB)](https://github.com/mashukui/codex-sound-alerts) [![Codex](https://img.shields.io/badge/Codex-0.144.3%2B-000000?logo=openai&logoColor=white)](https://github.com/openai/codex) ![Platforms](https://img.shields.io/badge/platform-macOS%20%7C%20Windows-555555) [![License](https://img.shields.io/badge/license-MIT-7C3AED)](https://github.com/mashukui/codex-sound-alerts?tab=MIT-1-ov-file)

Codex Sound Alerts is a lightweight plugin that lets you step away from the screen during long Codex tasks. It plays distinct sounds and shows desktop notifications when:

- Codex is waiting for you to approve a permission request.
- A root task finishes after running for at least 60 seconds.

The plugin supports macOS and Windows 10/11. It does not grant permissions, approve actions, inspect the screen, send network requests, or run a background service.

## Requirements

- Codex CLI or Codex app with plugin Hooks and the `PermissionRequest` event. Version 0.144.3 or newer is recommended.
- macOS, or Windows 10/11 with Windows PowerShell 5.1 or newer.

No Python, Node.js, or third-party package is required at runtime.

## Installation

Add the GitHub marketplace source, then install the plugin:

```sh
codex plugin marketplace add mashukui/codex-sound-alerts
codex plugin add codex-sound-alerts@codex-sound-alerts
```

Start a new Codex session after installation. Codex may ask you to review or trust the bundled Hooks once. Review the commands and accept them to enable alerts. Do not use `--dangerously-bypass-hook-trust`.

## Notifications

| Event | Sound | Notification |
| --- | --- | --- |
| Permission approval required | Ping / Exclamation | `Codex needs attention` |
| Task completed after 60 seconds | Glass / Asterisk | `Codex task finished` |

### Notification previews

**Permission approval required**

![Codex permission approval notification](notify_approval_required.png)

**Long-running task completed**

![Codex long-running task completion notification](notify_task_finish.png)

On macOS, sounds play at `2.0` gain (twice `afplay`'s default). Notifications are sent by Script Editor (`osascript`). If they do not appear, enable notifications for Script Editor in **System Settings > Notifications**.

On Windows, the plugin uses a native WinRT Toast. If Toast notifications are unavailable or disabled, the system sound still plays. Windows system sounds do not expose a per-playback gain setting, so they follow the configured system volume. Focus, Do Not Disturb, mute, and operating-system notification settings are always respected.

## Privacy and safety

- Approval notifications use generic text and never include commands, paths, prompts, or tool names.
- The approval Hook emits no `allow`, `deny`, or other decision. Codex continues to show its normal approval UI.
- Timing state contains only a hash of the Codex session/turn IDs and a Unix timestamp.
- Timing files live in Codex's plugin data directory, are removed when the turn ends, and stale entries older than seven days are cleaned up.
- Hook failures exit successfully so an audio or notification problem cannot block Codex.

## Uninstall

```sh
codex plugin remove codex-sound-alerts@codex-sound-alerts
codex plugin marketplace remove codex-sound-alerts
```

## Development

Run the dependency-free tests:

```sh
python3 tests/test_plugin.py
```

The test suite runs the native alert script in a test mode that records events instead of playing real sounds or displaying notifications.

## License

[MIT](https://github.com/mashukui/codex-sound-alerts?tab=MIT-1-ov-file)
