# Testing Didact on Che 7.15.1

Open this file and click Ctrl+Alt+V to open it in the Didact webview (or right click and select `Didact: Start Didact Tutorial from file`). Copy this file and the two scaffold*.json files into a local directory to test.

Then follow these steps.

First, create a new file. [(Execute^)](didact://?commandId=vscode.didact.scaffoldProject&projectFilePath=scaffold-readme.json "Create a new readme file")

Then, open it and come back to the Didact window. [(Execute^)](didact://?commandId=vscode.open&projectFilePath=readme.txt "Open the readme file"){.didact}

Second, create a file nested in a folder. [(Execute^)](didact://?commandId=vscode.didact.scaffoldProject&projectFilePath=scaffold-deeper.json "Create a new text file nested in a folder called deeper")

Then, open the deeper file and come back to the Didact window. [(Execute^)](didact://?commandId=vscode.open&projectFilePath=deeper/lost.txt "Open the deeper text file"){.didact}

Lastly, when you come back to Didact, you should be able to then click this final link to open a terminal. [(Execute^)](didact://?commandId=vscode.didact.sendNamedTerminalAString&text=newTerm$$echo%20Hello%20Didact! "Open a new terminal and send an echo statement")

## Prerequisites

Use this section to test some common utilities.

<a href='didact://?commandId=vscode.didact.validateAllRequirements' title='Validate all requirements!'><button>Validate all Requirements at Once!</button></a>

| Requirement (Click to Verify)  | Status | Additional Information/Solution |
| :--- | :--- | :--- |
| [This should pass](didact://?commandId=vscode.didact.cliCommandSuccessful&text=node-status$$node%20--version "Ensure that Node is available at the command line"){.didact} | *Status: unknown*{#node-status} | Go to [https://nodejs.org](https://nodejs.org) and find the version of Node for your operating system, install it, then come back and try again.|
| [This should fail](didact://?commandId=vscode.didact.cliCommandSuccessful&text=mvn-status$$mvn%20--version "Ensure that Maven is available at the command line"){.didact} | *Status: unknown*{#mvn-status} | This is just to show what a failing requirements check looks like.|

## Commands that were previously not working

There were some commands that were previously not working in 7.14 che/theia. They were fixed in https://github.com/eclipse/che/issues/16940

### explorer.NewFolder

This seems to behave the same on Che as it does in the desktop version of VS Code: an editable field or pop up appears prompting for the new folder name. We should investigate if there's a way to pass a variable or use a different command to create a new folder without user intervention.

* [Create new folder and prompt for name](didact://?commandId=explorer.newFolder) (Works)
* [Create new folder and give it the name](didact://?commandId=explorer.newFolder&projectFilePath=newfolder) (not working) - need to investigate on the Didact side

### workbench.view.explorer

This puts focus on the Explorer view

* [Set focus to explorer](didact://?commandId=workbench.view.explorer) (Works)

### vscode.didact.sendNamedTerminalCtrlC (via workbench.action.terminal.sendSequence)

* [Open a new terminal and start ping](didact://?commandId=vscode.didact.sendNamedTerminalAString&text=newTerm2$$ping%20localhost)
* [Send Ctrl C to terminal to stop ping](didact://?commandId=vscode.didact.sendNamedTerminalCtrlC&text=newTerm2) (Works)

### vscode.didact.closeNamedTerminal (via workbench.action.terminal.kill)

* [Open terminal to kill](didact://?commandId=vscode.didact.startTerminalWithName&text=KillThisTerminal)
* [Kill the terminal](didact://?commandId=vscode.didact.closeNamedTerminal&text=KillThisTerminal)

### copyFilePath

This one is used in scaffolding as well as a few other places, taking the currently selected file or folder in the explorer in the utils.getCurretnFileSelectionPath() method. It works, but a warning pops up when we use it.

Apparently there are some issues upstream with the system clipboard. A warning pops up - "Clipboard API is not available. It can be enabled by 'dom.events.testing.asyncClipboard' preference on 'about:config' page. Then reload Theia. Note, it will allow FireFox getting full access to the system clipboard."

According to Artem (9-JUL-2020):

* By default, Async Clipboard API is disabled in FireFox. All Theia can do is detect the API calls and ask the user to grant access to the system clipboard with dom.events.testing.asyncClipboard browser's preference.
* More details are here https://github.com/eclipse-theia/theia/pull/5994
* BTW, Chrome implemented it much better, through the permissions pop-up.There was an idea to rely on browser's document.execCommand API as a fallback but it also has some restrictions in FireFox.
* See the details in https://github.com/eclipse-theia/theia/pull/5994#issuecomment-528883872There's the issue https://github.com/eclipse-theia/theia/issues/6209 in upstream Theia to collect the ideas for further improvements of Clipboard API.
