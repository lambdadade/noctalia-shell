# Fork Changes

Changes made on top of upstream [noctalia-shell](https://github.com/end-4/dots-hyprland).

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
