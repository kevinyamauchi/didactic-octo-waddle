---
date: 2025-01-16
authors:
    - kevinyamauchi
---
# Setting up Python with uv and VS Code

I have been a long time conda (now mamba) + VS Code  user and I have been wanting to try out `uv`. I decided to finally give it a go and I am documenting the process here.

<!-- more -->

## Why try something new?
I am generally a proponent of "if it ain't broke, don't fix it" and my conda + VS Code set up was working. There were a couple of pain points though:

- my conda installation was ~40 GB on my laptop. I generally make an environment for each project and I often use the same core libraries (`numpy`, `skimage`, `napari`, etc.), so a lot of that space is probably duplicated libraries. `uv` has a global cache which should help deduplicate some libraries.
- Pycharm can really slow down my computer when it's indexing my libraries. I wanted to try a new IDE that might be a bit lighter.

Additionally, there were a couple of features of `uv` that I wanted to experiment with:

- I like that you can write the requirements run a script in the header and then just run it with `uv run`. For scientific research, this is really handy because I write a lot of scripts I think are a one-off and then I want to run them again later.
- I like that you can specify sources for dependencies that can depend on the platform you are installing the package on. For hardware accelerated libraries (e.g., `torch`) this seems like it could be a way to simplify installation for the end user. Let's see how it goes...

## Installing everything

### Setting up VS Code
I installed VS Code using the binary from the website. You could also consider using [VS Codium](https://vscodium.com/), which is a version of VS Code [released with an MIT license and built with telemetry disabled](https://vscodium.com/#why).

Next, I added some extensions. You can open the extension marketplace from the View -> Extensions menu. I installed the following extensions:

- Python
- Python Debugger
- Github Actions: view your Github Actions workflows in VS Code
- Even Better TOML (nice for editting the toml files that are common in Python projects)
- Ruff (linting the code)

I then made some small updates to my user settings. You can access the user settings by first opening the command pallet (CMD + SHIFT + P) and then typing "user settings json". If you select this, it will open the JSON file containing your user settings in the editor. I updated my user settings as follows:

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
    "python.envFile": "${workspaceFolder}/.env",
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

I installed `uv` using the instructions on [on their readme](https://github.com/astral-sh/uv).

