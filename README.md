# shunction (Work-in-Progress! :building_construction: :construction:)

> :fuelpump: Self-hosted [Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/) ftw!

shunction (SHell Function) is a parody of _Azure Functions_. When I first heard of Azure Functions, I knew I had to write this.

In short, _shunction_ given a folder of function scripts, provides various mechanisms by which they can be triggered.

## Getting Started
Simply clone this repository like this:

```bash
git clone git@github.com:sbrl/shunction.git
cd shunction;
```

Then you can call shunction like this to display some quick help on how to use it:

```bash
./shunction
```

Functions are, by default, looked for in `./functions`. A function file can be any executable with a filename in the form `function_name.func` - the shebang (the `#!...` bit) is respected.


## CLI Arguments
Extra CLI arguments are supported:

Argument		| Short form	| Meaning
----------------|---------------|--------------------
`--config`		| `-c`			| Specify the location of a configuration file to load


## Configuration
Although not required, shunction _can_ take a configuration file. Specify it's location with the `--config` CLI argument.

Such a configuration file should look something like this:

```bash
#!/usr/bin/env bash
functions_folder="path/to/directory";
```

Directive			| Default Value	| Meaning
--------------------|---------------|------------------------------------------
`functions_folder`	| `./functions`	| The location of the directory in which to find functions to execute

## Usage
Shunction supports 4 modes of operation: ad-hoc, cron, inotify, and http.

### Ad-hoc
For one-off runs, use _ad-hoc_ mode.

```bash
./shunction trigger adhoc function_name
```

### Cron
Regularly-repeating jobs can be invoked by editing your crontab with `crontab -e`. Paste in something like this:

```bash
5 4 * * *	/absolute/path/to/shunction trigger cron function_name
```

This website is really useful for generating crontab definitions: <https://crontab.guru/>.

<!-- TODO: Add CLI argument to disable colour output and add pipe-to-file example here -->

### Inotify
The inotify mode allows you to run jobs when something changes on disk. The `inotifywait` command is required. Shunction will automatically watch subdirectories for you.

Use it like this:

```bash
./shunction inotify "path/to/file/or/directory" "function_name"
```

It will stick around until it is killed. Note that it will run the function once for _every_ event detected in the background and start listening for additional jobs immediately, so if you need to ensure only 1 instance of your program is running, try looking at [this StackOverflow answer](https://stackoverflow.com/a/1985512/1460422).

### Http
Finally, shunction supports listening over HTTP, though it's not advised to listen on anything more than localhost. Unlike inotify, http mode supports execution of multiple different functions, though only 1 at a time.

```bash
./shunction http bind_address port_number
./shunction http 127.0.0.1 7777
```

Note that regular Linux rules still apply - if you want to listen on port numbers below 1024, shunction either needs to run as root (bad idea!), or you need to tell Linux that it's allowed to like this: `setcap 'cap_net_bind_service=+ep' path/to/shunction`.
