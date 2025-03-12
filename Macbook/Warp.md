## Open Warp with shortcut

- Open `Automator` 
- Create `Quick Action`
- Save as `New Terminal` for example :
  ```sh
on run {input, parameters}
    tell application "Warp"
        if it is running then reopen
        activate
    end tell
end run
```
- `Settings` > `Keyboard`> `Shortcuts`> `Services`>`General`>`New Terminal`
- Add shortcut <kbd>Control</kbd> + <kbd> Command</kbd> + <kbd> T</kbd>