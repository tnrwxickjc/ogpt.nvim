

----
# OGPT.nvim
![GitHub Workflow Status](http://img.shields.io/github/actions/workflow/status/huynle/ogpt.nvim/default.yml?branch=main&style=for-the-badge)
![Lua](https://img.shields.io/badge/Made%20with%20Lua-blueviolet.svg?style=for-the-badge&logo=lua)

## Features
- **Multiple Providers**: OGPT.nvim can take multiple providers. Ollama, OpenAI, textgenui, Gemini, Anthropic, more if there are pull requests
- **Mix-match Provider**: default provider is used, but you can mix and match different provider AND specific model to different actions, at any point in your run or configurations.
- **Interactive Q&A**: Engage in interactive question-and-answer sessions with LLMs using an intuitive interface.
- **Persona-based Conversations**: Explore various perspectives and have conversations with different personas by selecting prompts from Awesome ChatGPT Prompts.
- **Customizable Actions**: Execute a range of actions utilizing the gpt model, such as grammar
  correction, translation, keyword generation, docstring creation, test addition, code
optimization, summarization, bug fixing, code explanation, and code readability
analysis. Additionally, you can define your own custom actions using a JSON file or just through
plugin configurations.

## Demo
[Screencast from 2024-02-16 15-27-06.webm](https://github.com/huynle/ogpt.nvim/assets/2416122/2c810aee-d282-47c2-86bd-558e39658b0e)


## Installation

If you do not specify a provider, `ollama` will be the default provider. `http://localhost:11434`
is your endpoint. `mistral:7b` will be your default if the configuration is not updated.

```lua
-- Simple, minimal Lazy.nvim configuration
{
  "huynle/ogpt.nvim",
    event = "VeryLazy",
    opts = {
      default_provider = "ollama",
      providers = {
        ollama = {
          api_host = os.getenv("OLLAMA_API_HOST") or "http://localhost:11434",
          api_key = os.getenv("OLLAMA_API_KEY") or "",
        }
      }
    },
    dependencies = {
      "MunifTanjim/nui.nvim",
      "nvim-lua/plenary.nvim",
      "nvim-telescope/telescope.nvim"
    }
}
```

## Configuration

`OGPT.nvim` comes with the following defaults. You can override any of the fields by passing a config as setup parameters.

https://github.com/huynle/ogpt.nvim/blob/main/lua/ogpt/config.lua


### Ollama Setup

`OGPT` is a Neovim plugin that allows you to effortlessly utilize the Ollama OGPT API, empowering you to generate natural language responses from Ollama directly within the editor in response to your prompts.

- Make sure you have `curl` installed.
- Have a local instance of Ollama running.

Custom Ollama API host with the configuration option `api_host_cmd` or
environment variable called `$OLLAMA_API_HOST`. It's useful if you run Ollama remotely.

### Gemini, TextGenUI, OpenAI, Anthropic Setup
* not much here, you just have to get your API keys and provide that in your configuration. If your
  configuration files are public, you probably want to create environment variable for your API
keys

### Edgy.nvim setup
![edgy-example](assets/images/edgy-example.png.png)

`edgy.nvim` plugin offers a side window (positioned to the right by default) that provides a parallel working space where you can work on your project while interacting with OGPT. Here is an example of an 'edgy' configuration.

```lua
{
{
  "huynle/ogpt.nvim",
    event = "VeryLazy",
    opts = {
      default_provider = "ollama",
      edgy = true, -- enable this!
      single_window = false, -- set this to true if you want only one OGPT window to appear at a time
      providers = {
        ollama = {
          api_host = os.getenv("OLLAMA_API_HOST") or "http://localhost:11434",
          api_key = os.getenv("OLLAMA_API_KEY") or "",
        }
      }
    },
    dependencies = {
      "MunifTanjim/nui.nvim",
      "nvim-lua/plenary.nvim",
      "nvim-telescope/telescope.nvim"
    }
},
{
    "folke/edgy.nvim",
    event = "VeryLazy",
    init = function()
      vim.opt.laststatus = 3
      vim.opt.splitkeep = "screen" -- or "topline" or "screen"
    end,
    opts = {
      exit_when_last = false,
      animate = {
        enabled = false,
      },
      wo = {
        winbar = true,
        winfixwidth = true,
        winfixheight = false,
        winhighlight = "WinBar:EdgyWinBar,Normal:EdgyNormal",
        spell = false,
        signcolumn = "no",
      },
      keys = {
        -- -- close window
        ["q"] = function(win)
          win:close()
        end,
        -- close sidebar
        ["Q"] = function(win)
          win.view.edgebar:close()
        end,
        -- increase width
        ["<S-Right>"] = function(win)
          win:resize("width", 3)
        end,
        -- decrease width
        ["<S-Left>"] = function(win)
          win:resize("width", -3)
        end,
        -- increase height
        ["<S-Up>"] = function(win)
          win:resize("height", 3)
        end,
        -- decrease height
        ["<S-Down>"] = function(win)
          win:resize("height", -3)
        end,
      },
      right = {
        {
          title = "OGPT Popup",
          ft = "ogpt-popup",
          size = { width = 0.2 },
          wo = {
            wrap = true,
          },
        },
        {
          title = "OGPT Parameters",
          ft = "ogpt-parameters-window",
          size = { height = 6 },
          wo = {
            wrap = true,
          },
        },
        {
          title = "OGPT Template",
          ft = "ogpt-template",
          size = { height = 6 },
        },
        {
          title = "OGPT Sessions",
          ft = "ogpt-sessions",
          size = { height = 6 },
          wo = {
            wrap = true,
          },
        },
        {
          title = "OGPT System Input",
          ft = "ogpt-system-window",
          size = { height = 6 },
        },
        {
          title = "OGPT",
          ft = "ogpt-window",
          size = { height = 0.5 },
          wo = {
            wrap = true,
          },
        },
        {
          title = "OGPT {{{selection}}}",
          ft = "ogpt-selection",
          size = { width = 80, height = 4 },
          wo = {
            wrap = true,
          },
        },
        {
          title = "OGPt {{{instruction}}}",
          ft = "ogpt-instruction",
          size = { width = 80, height = 4 },
          wo = {
            wrap = true,
          },
        },
        {
          title = "OGPT Chat",
          ft = "ogpt-input",
          size = { width = 80, height = 4 },
          wo = {
            wrap = true,
          },
        },
      },
    },
  }
}
```


## Usage

Plugin exposes following commands:

### `OGPT`
`OGPT` command opens an interactive window to communicate with LLM backends. The interactive window consists of four panes:
* OGPT Chat: Input text area to write prompts.
* OGPT: Output text area printing responses from LLM.
* OGPT Parameters: (Toggled by \<Ctrl-o\>,) Parameter panel to display and modify LLM paramters.
* OGPT Sessions: (Toggled by \<Ctrl-o\>,) Sessions panel to display chat sessions.

#### Useful shortcuts
<table>
    <thead>
        <tr>
            <th>Area</th>
            <th>Default shortcuts</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan=6>Common</td>
            <td>Ctrl-o</td>
            <td>Toggle parameter panel (OGPT Parameters) and sessions panel (OGPT Sessions).</td>
        </tr>
        <tr>
            <td>Ctrl-n</td>
            <td>Create a new session.</td>
        </tr>
        <tr>
            <td>Ctrl-c</td>
            <td>Close OGPT.
        </tr>
        <tr>
            <td>Ctrl-i</td>
            <td>Copy code in LLM's latest response in OGPT output text area.
        </tr>
        <tr>
            <td>Ctrl-x</td>
            <td>Stop generating response.</td>
        </tr>
        <tr>
            <td>Tab</td>
            <td>Cycle through panes.</td>
        </tr>
        <tr>
            <td rowspan=4>OGPT</td>
            <td>K</td>
            <td>Previous response.</td>
        </tr>
        <tr>
            <td>J</td>
            <td>Next response.</td>
        </tr>
        <tr>
            <td>Ctrl-u</td>
            <td>Scroll up.</td>
        </tr>
        <tr>
            <td>Ctrl-d</td>
            <td>Scroll down.</td>
        </tr>
        <tr>
            <td rowspan=5>OGPT Chat</td>
            <td>Enter<br>(normal mode)</td>
            <td>Send prompt to LLM.</td>
        </tr>
        <tr>
          <td>Alt-Enter<br>(input mode)</td>
          <td>Send prompt to LLM.</td>
        </tr>
        <tr>
          <td>Ctrl-y</td>
          <td>Copy the latest response from LLM in OGPT output text area.</td>
        </tr>
        <tr>
          <td>Ctrl-r</td>
          <td>Toggle role (assistant or user).</td>
        </tr>
        <tr>
          <td>Ctrl-s</td>
          <td>Toggle system message.</td>
        </tr>
        <tr>
          <td rowspan=1>OGPT Parameters</td>
          <td>Enter</td>
          <td>Change parameter.</td>
        </tr>
        <tr>
            <td rowspan=3>OGPT Sessions</td>
            <td>Enter</td>
            <td>Switch session.</td>
        </tr>
        <tr>
            <td>d</td>
            <td>Delete session.</td>
        </tr>
        <tr>
            <td>r</td>
            <td>Rename session. Note that an active session cannot be removed.</td>
        </tr>
    </tbody>
</table>

<!-- Someone please fix the ugly HTML table to Markdown table -->
<!-- Area | Default shortcuts | Description -->
<!-- ---- | ---- | ---- -->
<!-- Common | Ctrl-o | Toggle parameter panel (OGPT Parameters) and sessions panel (OGPT Sessions). -->
<!--      ^ | Ctrl-n | Create a new session. -->
<!--      ^ |      q | Close OGPT. -->
<!--      ^ | Ctrl-c | Close OGPT. -->
<!--      ^ | Ctrl-i | Copy code in LLM's latest response in OGPT output text area. -->
<!--      ^ | Ctrl-c | Close OGPT. -->
<!--      ^ | Ctrl-x | Stop generating response. -->
<!--      ^ |    Tab | Cycle through panes. -->
<!-- OGPT |      K | Previous response. -->
<!--    ^ |      J | Next response. -->
<!--    ^ | Ctrl-u | Scroll up. -->
<!--    ^ | Ctrl-d | Scroll down. -->
<!-- OGPT Chat |  Enter | Send prompt to LLM. -->
<!--         ^ | Alt-Enter (input mode inside OGPT Chat) | Send prompt to LLM. -->
<!--         ^ | Ctrl-y | Copy the latest response from LLM in OGPT output text area. -->
<!--         ^ | Ctrl-r | Toggle role (assistant or user). -->
<!--         ^ | Ctrl-s | Toggle system message. -->
<!-- OGPT Parameters | Enter | Change parameter. -->
<!-- OGPT Sessions | Enter | Switch session. -->
<!--             ^ |     d | Delete session. -->
<!--             ^ |     r | Rename session. Note that an active session cannot be removed. -->

Shortcuts for OGPT interactive window can be modified at `opts.chat.keymaps`.

### `OGPTActAs`
`OGPTActAs` command which opens a prompt selection from [Awesome OGPT Prompts](https://github.com/f/awesome-chatgpt-prompts) to be used with the `mistral:7b` model.

### `OGPTRun [action_name]`

`OGPTRun [action_name]` runs LLM with predefined action named `[action_name]`. There are [default actions](https://github.com/huynle/ogpt.nvim/?tab=readme-ov-file#default-actions) provided by OGPT, and [custom actions](https://github.com/huynle/ogpt.nvim/?tab=readme-ov-file#custom-actions) defined by users.

An action requires parameters to configure its behavior. Default model parameters are defined in `actions.<action_name>` in [config.lua](./lua/ogpt/config.lua), and custom model parameters are defined either in a custom OGPT configuration file or in a separate actions configuration file such as [`actions.json`](./lua/ogpt/actions.json).

Some of the action parameters include:
* `type`: The type of OGPT interface. Currently, there are three types of OGPT interface:
  * `popup`: Light-weight popup window
  * `edit`: OGPT window
  * `completions`: Does not open a window, completes directly on the editing window
* `strategy`: Determines the way OGPT interface behaves. There are specific `strategy`s you can use for each `type`:
  * `type = "popup"`: display, replace, append, prepend, quick_fix
  * `type = "completions"`: display, replace, append, prepend
  * `type = "edits"`: edit, edit_code
* `system`: System prompt
* `params`: Model parameters. Default model parameters are defined in `opts.providers.<provider_name>.api_params`. You can customize model parameters in `opts.actions.<action_name>.params` and override default model parameters.
  * `model`: LLM model to use for the action
  * `stop`: Condition for LLM to stop generating response. For example, a useful stop condition for `codellama` is "\`\`\`". See "optimize_code" action in [Example `lazy.nvim` Configuration](https://github.com/huynle/ogpt.nvim/?tab=readme-ov-file#example-lazynvim-configuration).
  * `temperature`: Variability of LLM's response
  * `frequency_penalty`: Somebody explain please
  * `max_tokens`: Maximum number of tokens
  * `top_p`: Somebody explain please
* `template`: Prompt template. Template defines a general instruction that LLM must follow. Template can include template argument in form of `{{{<argument_name>}}}`, where `<argument_name>` is a template argument defined in `args` . Other than the arguments defined in `args`, you can include the following arguments:
  * `{{{input}}}`: The selected text in visual mode.
  * `{{{filetype}}}`: The type of file you are interacting with.
  * Is there any other useful template arguments I'm missing?
* `args`: Template argument. Template arguments define the arguments that replace the arguments with the same name in `template`. Some common template arguments include:
  * `instruction`: Custom instruction for LLM to follow. It tends to be more specific and structured than the general instruction that goes into `template`.
  * `lang`: Language. Often used for language related actions, such as translation or grammar check.


#### Default actions

Default actions are defined in `actions.<action_name>` in [config.lua](./lua/ogpt/config.lua).

##### Example: `OGPTRun edit_with_instructions`
`OGPTRun edit_with_instructions` opens interactive window to edit selected text or whole window using the model defined in `config.<provider_name>.api_params`.

By default, `OGPTRun edit_with_instructions`'s interface type is `edit`, which opens up an interactive window to the right. In the window, you can use `<c-o>` (default keymap, can be customized)  to open and close the parameter panels. Note this screenshot is using `edgy.nvim`

![edit_with_instruction_no_params](assets/images/edit_with_instruction_no_param_panel.png)

![edit_with_instructions_with_params](assets/images/edit_with_instruction_with_params_panel.png)

#### Custom actions

You can customize OGPT actions by defining them in a custom OGPT configuration files, or in a separate action file. The default model configurations in [config.lua](./lua/ogpt/config.lua) can be good references when making your own [custom actions](https://github.com/huynle/ogpt.nvim/?tab=readme-ov-file#custom-actions).

##### A custom OGPT configuration file (preferred)

You can configure actions in your own OGPT configuration file (typically in a Lua file like `ogpt.lua` if using Neovim). In a custom OGPT configuration file, you must define actions in `actions.<action_name>` like the way default actions are defined in [config.lua](./lua/ogpt/config.lua).

```lua
--- config options lua
opts = {
  ...
  actions = {
    grammar_correction = {
      -- type = "popup", -- could be a string or table to override
      type = {
        popup = { -- overrides the default popup options - https://github.com/huynle/ogpt.nvim/blob/main/lua/ogpt/config.lua#L147-L180
          edgy = true
        }
      },
      strategy = "replace",
      provider = "ollama", -- default to "default_provider" if not provided
      model = "mixtral:7b", -- default to "provider.<default_provider>.model" if not provided
      template = "Correct the given text to standard {{{lang}}}:\n\n```{{{input}}}```",
      system = "You are a helpful note writing assistant, given a text input, correct the text only for grammar and spelling error. You are to keep all formatting the same, e.g. markdown bullets, should stay as a markdown bullet in the result, and indents should stay the same. Return ONLY the corrected text.",
      params = {
        temperature = 0.3,
      },
      args = {
        lang = {
          type = "string",
          optional = "true",
          default = "english",
        },
      },
    },
  ...
  }
}
```

The `edit` type consists in showing the output side by side with the input and available for further editing prompts.

The `display` strategy shows the output in a float window. `append` and `replace` modify the text directly in the buffer with "a" or "r"

##### Using Actions.json

It is possible to define custom actions using a JSON file. Please see the example at [`actions.json`](./lua/ogpt/actions.json) for reference.

An example of custom action may look like the following: (`#` marks comments)
```json
{
  "action_name": {
    "type": "popup", # "popup" or "edit"
    "template": "A template using possible variable: {{{filetype}}} (neovim filetype), {{{input}}} (the selected text) an {{{argument}}} (provided on the command line)",
    "strategy": "replace", # or "display" or "append" or "edit"
    "params": { # parameters according to the official Ollama API
      "model": "mistral:7b", # or any other model supported by `"type"` in the Ollama API, use the playground for reference
      "stop": [
        "```" # a string used to stop the model
      ]
    }
    "args": {
      "argument": "some value" -- or function
    }
  }
}
```
If you want to use other action files, you must append the paths to those files to `defaults.actions_paths` in [config.lua](./lua/ogpt/config.lua).

#### Run With Options with Vim Commands
On the fly, you can execute a command line to call OGPT. An example to replace
the grammar_correction call, is provided below.
`:OGPTRun grammar_correction {provider="openai", model="gpt-4"}`

To make it even more dynamic, you can change it to have the provider/model or any parameters be
inputted by the user on the spot when the command is executed.
`:OGPTRun grammar_correction {provider=vim.fn.input("Provider: "), type={popup={edgy=false}}}}`

Additionally, in the above example, `edgy.nvim` can be turned off. So that the response popup
inline where the cursor would be. For additional options for the popup, please read through
https://github.com/huynle/ogpt.nvim/blob/main/lua/ogpt/config.lua#L147-L180

For example, you and have it popup and change `enter = false`, which leaves the cursor in the same
location, instead of moving it to the popup.

Additionally, for advanced users, this allows you to use Vim autocommands. For example, autocompletion
can happen when the cursor is paused. Look at the various Template Helpers for this advanced
options, because now 

### Template Helpers
Currently, the given inputs to the API gets scanned for `{{{<template_helper_name>}}}` for expansion.
This is helpful when you want to give a little more context to your API requests, or simply to hook
in additional function calls.

#### Available Template Helpers
Look at this file for the most up to date Template Helpers. If you have more template helpers,
please make an MR, your contribution is appreciated!

https://github.com/huynle/ogpt.nvim/blob/main/lua/ogpt/flows/actions/template_helpers.lua

#### How to Use
This is a custom action that I use all the time to use the visible windows
as context to have AI answer any inline questions.

```lua
....
  -- Other OGPT configurations here
  ....
  actions = {
    infill_visible_code = {
      type = "popup",
      template = [[
      Given the following code snippets, please complete the code by infilling the rest of the code in between the two
      code snippets for BEFORE and