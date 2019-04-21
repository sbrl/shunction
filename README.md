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
