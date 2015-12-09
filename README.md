# vscode-haxe-debug

Installation
------------

Pre-requisite: download and install [Visual Studio Code](https://code.visualstudio.com/)

Place (or symlink) the `vscode-haxe-debug` folder in your VSCode extensions directory:
- Linux: `$HOME/.vscode/extensions`
- Mac: `$HOME/.vscode/extensions`
- Windows: `%USERPROFILE%\.vscode\extensions`

(If it's running, restart Visual Studio Code after installing the extension.)

Note: This extension doesn't provide language support / syntax highlight. For that, install https://github.com/nadako/vscode-haxe in your extensions directory in the same way.

Usage
-----

In your project, add a .vscode folder with a launch.json file in it. See example launch.json files in the [test CLI project](https://github.com/jcward/vscode-hxcpp-debug/tree/master/test%20cli) or the [test OpenFL project](https://github.com/jcward/vscode-hxcpp-debug/tree/master/test%20openfl). You may need to update some of the parameters -- these tell the extension how to compile and launch your project:

```
	"compilePath=${workspaceRoot}",
	"compileCommand=openfl build linux -debug -DHXCPP_DEBUGGER",
	"runPath=${workspaceRoot}/Export/linux64/cpp/bin/",
	"runCommand=DisplayingABitmap",
	"runInTerminal=false"
```

Install the debugger library from git: `haxelib git debugger https://github.com/HaxeFoundation/hxcpp-debugger`

Your app needs to include the library (`-lib debugger`). Near the entry point of your app, add the following code:

```
#if debug
    new debugger.HaxeRemote(true, "localhost");
#end
```

(In place of localhost, you can use the IP address of your computer if you're debugging a mobile app, for example.)

Open your project folder in Visual Studio Code. 

Select the debugger button on the left (bottom button) and you should see the debug launch configurations (Build and debug, Debug) in the dropdown list:

![image](https://cloud.githubusercontent.com/assets/2192439/11687462/104c31f8-9e44-11e5-8f2c-8fcb60a49022.png)

If you do not see these options, ensure your project folder contains a `.vscode/launch.json` file like the samples given above.

Potential error conditions
--------------------------

**Configured debug type 'hxcpp' is not supported** - the extension is not properly installed in your `.vscode/extensions` directory. Ensure the `vscode-hxcpp-debug` directory is there, and contains the package.json file. Try restarting VSCode.
**Build and debug fails with COMPILE FAILED error** - ensure the `compileCommand` and `compilePath` in your `launch.json` file has the correct command, arguments, and syntax required to build your project.
**The debugger hangs after compiling** - ensure your `runCommand` and `runPath` in your `launch.json` file has the correct command, arguments, and syntax required to build your project. In addition, ensure you've compiled your project in debug mode, included the debugger library, and inserted the `new HaxeRemote()` hunk of code listed above. 

Development notes
-----------------

VSC documentation: 
- https://code.visualstudio.com/docs/extensions/example-debuggers
- https://code.visualstudio.com/docs/extensionAPI/api-debugging

HXCPP debugger:
- Existing CLI debugger: https://github.com/HaxeFoundation/hxcpp-debugger

Dan's haxe extension: https://github.com/nadako/vscode-haxe
