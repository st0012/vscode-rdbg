# VSCode Ruby rdbg Debugger

Ruby debugger to connect [debug](https://github.com/ruby/debug) library which utilize recent Ruby's debug support features.

## Requirement

You need to install `debug` gem and `rdbg` command should be in `$PATH`.

```shell
$ gem install debug --pre
```

## How to use

### Launch without configuration

Without any configuration, you can use this debugger by "Start Debugging" (F5 key) if you activate `.rb` file.

You will see the "Debug command line" input dialog.
Please specify your favorite command line you want to debug.

For example:
* `ruby foo.rb` (launch `foo.rb`)
* `ruby foo.rb 10 20 30` (launch `foo.rb` with options `10`, `20` and `30`)
* `rake taskA` (launch `rake` task `taskA`)
* `bundle exec rspec` (launch `rspec` command with `bundle exec`)
* `bin/rails s` (launch `bin/rails s`)

When you select a command line, the specified command will run on `rdbg` debugger, and VSCode will connect to the `rdbg` debugger with UNIX domain socket.

And new terminal is created (named `rdbg`).
You can see stdout/err outputs and you can input stdin on `rdbg` terminal.

You can stop the programs

* by setting breakpoints (F9) on source code.
* by exception (if you enable "rescue exception").
* by pushing the Pause button (F6).

When the program stops, you can see "Call stack", "Variables" and you can set "Watch" expressions.
On the debug console, you can input valid Ruby program and you will get an evaluation result on selected context ("Call stack").

See also: [Debugging in Visual Studio Code](https://code.visualstudio.com/docs/editor/debugging) 

For developers: `RUBY_DEBUG_DAP_SHOW_PROTOCOL=1` on `rdbg` terminal will show the all DAP protocol.

### Configuration

You can write your favorite setting in `.vscode/launch.json`.

To make a `.vscode/launch.json` with default settings, you only need to click "create a launch.json file" on the "Run and Debug" pane. And you will see the following default configurations.

```JSON
{
        // Use IntelliSense to learn about possible attributes.
        // Hover to view descriptions of existing attributes.
        // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
        "version": "0.2.0",
        "configurations": [
                {
                        "type": "rdbg",
                        "name": "Debug current file with rdbg",
                        "request": "launch",
                        "script": "${file}",
                        "args": [],
                        "askParameters": true
                },
                {
                        "type": "rdbg",
                        "name": "Attach with rdbg",
                        "request": "attach"
                }
        ]
}
```

It contains "Debug current file with rdbg" (launch) configuration and "Attach with rdbg" (attach) configuration.You can modify this configuration, and also you can add your favorite configuration like:

```JSON
                {
                        "type": "rdbg",
                        "name": "Run rake test",
                        "request": "launch",
                        "command": "rake",  # 
                        "script": "test",   # launch rake test with debugger
                        "args": [],
                        "askParameters": false  # Do not ask any more
                },
```

You can use the following "launch" configurations.

* `script`: Script file name (default: active ruby file on VSCode)
* `command`: Executable command (default: `ruby`)
* `cwd`: Directory to execute the program in (default: `${workspaceFolder}`)
* `args`: Command line arguments passed to the program (default: `[]`)
* `env`: Additional environment variables to pass to the debugging (and debugged) process.
* `useBundler`: Execute Ruby programs with `bundle exec` if `command` configuration is not given and `Gemfile` is available in the workspace.
* `askParameters`: Ask "Debug command line" before debugging (default: `true`)
* `rdbgPath`: Location of the rdbg executable.

### Attach configuration

You can attach to a Ruby process which run with opening debugger port.

The following command starts the `foo.rb` with opening debug port. There are more methods to open the port. See more for [ruby/debug: Debugging functionality for Ruby](https://github.com/ruby/debug).

```shell
$ rdbg --open foo.rb
```

After that, you can connect to the debug port. This extension searches opning debugger port and attach to that port by running "Attach with rdbg" (select it on the top of "RUN AND DEBUG" pane and push the green "Start Debugging" button).

## Acknowledgement

This extension is based on [Ethan Reesor / VSCode Byebug · GitLab](https://gitlab.com/firelizzard/vscode-byebug/-/tree/master/) by Ethan Reesor. With out his great work, the extension can not be released (Koichi learned TypeScript, VSCode extension and DAP by his extension).
