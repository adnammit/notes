# VSCODE WAT DO

## SHORTCUTS
```
C-p                 fuzzy finder
C-shift-p           command palette
C-,                 open settings
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

## DEBUGGING
* [more instructions](https://medium.com/idomongodb/visual-studio-code-debugging-vue-js-d3bf5bcc6656)
* vscode tasks (build, debug, etc) are configured in `.vscode/tasks.json`
* launch chrome etc by configuring `launch.json`

### C#
* [Omnisharp vs Language Server Protocol](https://github.com/dotnet/vscode-csharp/issues/5708)
* use the C# Dev ToolKit extension -- best for C# 6+ and can be configured to work with legacy versions as well

### Web Apps
* perform your usual build/serve operations: should be something like `npm i && npm run serve` and then begin debugging using the launch profile
* once you've done that, you should just be able to F5/green play button with your launch configuration selected in the debug screen
* [instructions on how to set up vue debugging in VSCode](https://vuejs.org/v2/cookbook/debugging-in-vscode.html#Launching-the-Application-from-VS-Code)
	* for vue, update `vue.config.json` so that webpack will build sourcemaps

