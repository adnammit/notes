# VSCODE WAT DO

## SHORTCUTS
```
C-p                 fuzzy finder
C-shift-p           command palette
alt-t               show file explorer
alt-x               view extensions
alt-shift-t         toggle terminal
C-k z               zen mode (esc esc to exit)
alt-z               toggle word wrap
C-\                 split vertical
C-shift-\           split horizontal
alt-right/left      switch to right/left pane

F12                 go to definition

C-d                 highlight current word

$ReturnValue        enter in the Immediate window to see the return value

```


## KEYBINDING
* customization files like `keybindings.json` are found in
    ```
    C:\Users\amanda.ryman\AppData\Roaming\Code\User
    ```
* keybindings are overwritten in `keybindings.json`
* custom bindings are appended to the default keybindings at runtime
    - the rules are read from bottom to top
    - the first rule that matches the pattern is used
    - thus, *defaults do not need to be unset*, however they can be removed
    ```
    // In Default Keyboard Shortcuts
    ...
    { "key": "tab", "command": "tab", "when": ... },
    { "key": "tab", "command": "jumpToNextSnippetPlaceholder", "when": ... },
    { "key": "tab", "command": "acceptSelectedSuggestion", "when": ... },
    ...

    // To remove the second rule, for example, add in keybindings.json:
    { "key": "tab", "command": "-jumpToNextSnippetPlaceholder" }
    ```
