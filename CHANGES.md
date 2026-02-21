# Fork Changes

Changes made on top of upstream [noctalia-shell](https://github.com/end-4/dots-hyprland).

---

## Clock widget

### Calendar week token `WW` in format strings

All clock format strings (bar horizontal/vertical/tooltip, desktop minimal style)
now support a custom `WW` token that expands to the ISO 8601 week number,
zero-padded to two digits (e.g. `08`).

Example format: `HH:mm\nCW WW` → `14:30\nCW 08`

The token is also listed as a clickable entry in the format token helper panel
inside clock widget settings, with a live example showing the current week number.

Files changed: `Commons/Time.qml`, `Modules/Bar/Widgets/Clock.qml`,
`Modules/DesktopWidgets/Widgets/DesktopClock.qml`,
`Modules/Panels/Settings/Bar/WidgetSettings/ClockSettings.qml`,
`Modules/Panels/Settings/DesktopWidgets/WidgetSettings/ClockSettings.qml`,
`Widgets/NDateTimeTokens.qml`, `Assets/Translations/*.json` (17 languages)

---

## UX

### Selectable description text in settings (`Widgets/NLabel.qml`)

Description/hint text below setting labels was rendered with Qt's `Text` element,
which does not support text selection. Replaced with a read-only `TextEdit` so
command examples and other hints in settings panels can be copied.

### Notification icon urgency color (`Modules/Bar/Widgets/NotificationHistory.qml`)

The notification bell icon now turns red (`mError`) when there are unread
notifications, instead of only showing a small dot badge. Reverts to the
configured icon color when the unread count reaches zero.

---

## Launcher

### `launcher translate` IPC command (`Services/Control/IPCService.qml`)

Toggles the launcher into translate mode (`>translate ` prefix) on the current screen.

- If the launcher is closed, opens it with `>translate ` pre-filled
- If it is already in translate mode, closes it
- If it is open in a different mode, switches to translate mode

```sh
qs -c noctalia-shell ipc call launcher translate
```

---

## IPC

### `notifications readAll`

Marks all notifications as read by advancing the last-seen timestamp to now.
Useful after `dismissAll` to clear the urgency color without opening the panel.

```sh
qs -c noctalia-shell ipc call notifications readAll
```

### `notifications readLast`

Advances the last-seen timestamp to the oldest unread notification, reducing
the unread count by one per call. Call repeatedly to step through unread
notifications one at a time.

```sh
qs -c noctalia-shell ipc call notifications readLast
```
