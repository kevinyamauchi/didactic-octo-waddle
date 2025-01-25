---
date: 2025-01-16
authors:
    - kevinyamauchi
---
# Setting up Python with uv and VS Code

In this post, I share how I set up my Python development environment using `uv` and Visual Studio Code.

<!-- more -->

## Why uv?

I use `uv` for managing my Python environments and Python project dependencies. My main reasons for using `uv` are:

- It is super fast. I was skeptical that the speed would really matter, but it really is so much faster that it changes they way that I use environments. When they only take a few seconds to build, I can create and throw them away at will.
- I like that you can write the requirements run a script in the header and then just run it with `uv run`. For scientific research, this is really handy because I write a lot of scripts I think are a one-off and then I want to run them again later. I no longer have to try and remember which environment I ran the script in.
- I like that you can specify sources for dependencies that can depend on the platform you are installing the package on. For hardware accelerated libraries (e.g., `torch`) this seems like it could be a way to simplify installation for the end user. Let's see how it goes...

## Why Visual Studio Code?

I don't really have a strong opinion on which IDE/text editor to use. I also used PyCharm for many years. However, I have recently been using VS Code for the following reasons:

- It is free and available on most platforms. I like that all of my students can use VS Code when I am teaching.
- It is reasonably light weight.
- It works for many languages. I like that I don't have to swich programs for different projects.
- It has a decent Python debugger.

## Installing everything

### Setting up VS Code
I install VS Code using the binary from the website. You could also consider using [VS Codium](https://vscodium.com/), which is a version of VS Code [released with an MIT license and built with telemetry disabled](https://vscodium.com/#why).

Next, I added some extensions. You can open the extension marketplace from the View -> Extensions menu. By default, I install the following extensions:

- Python
- Python Debugger
- Github Actions: view your Github Actions workflows in VS Code
- Even Better TOML (nice for editting the toml files that are common in Python projects)
- Ruff (linting the code)

I then make some small updates to my user settings. You can access the user settings by first opening the command pallet (CMD + SHIFT + P) and then typing "user settings json". If you select this, it will open the JSON file containing your user settings in the editor. I updated my user settings as follows:

```json
{
	// Imports
	// Add and organize missing imports on save
	"editor.codeActionsOnSave": {
		"source.organizeImports": true,
		"source.addMissingImports": true
	},

    // Python settings
    "python.analysis.autoSearchPaths": true,
    
    // It is common to put the package in the src directory
    // so we tell the static analysis tools to look there
    "python.analysis.extraPaths": [
        "${workspaceFolder}/src"
    ],
    "python.terminal.activateEnvironment": true,

    // Test settings
    // These set pytest as the default 
    "python.testing.pytestEnabled": true,
    "python.testing.unittestEnabled": false,
    
    // I store my tests in a /tests folder in the base directory
    // you might change this if you have a different preference
    "python.testing.cwd": "${workspaceFolder}/tests",
    
    // We will be using virtual environments via uv, so this is the path
    // to the project's pytest
    "python.testing.pytestPath": "${workspaceFolder}/.venv/bin/pytest",
    "python.testing.autoTestDiscoverOnSaveEnabled": true,
}
```

### Installing uv

I install `uv` using the instructions on [their readme](https://github.com/astral-sh/uv). There are a few tutorials on the `uv` docs that I found helpful for getting started:

- [Running scripts](https://docs.astral.sh/uv/guides/scripts/). I think one of the strengths of `uv` is the fact that it is so fast to create environments that you can make them just to run a script. This is great for being able to re-run scripts down the road. This tutorial shows how to run scripts using `uv`. 
- [Working on projects](https://docs.astral.sh/uv/guides/projects/). This tutorial goes into how to use `uv` to manage your Python projects.
