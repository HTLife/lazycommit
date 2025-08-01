# lazycommit (fixed)

Fixed version that resolves the `<no value>: <nil>` issue in lazygit when no changes are staged.

<p align="center">
  <img src="https://github.com/chhoumann/bunnai/assets/29108628/1ec69e68-7d5e-4a4d-b4d6-56e202e1c54c">
</p>

have ai write commit messages for you in [lazygit](https://github.com/jesseduffield/lazygit).

uses Openai or google AI to generate commit message suggestions based on the diff between the current branch and master.
then you can select a commit message from the list and use it to commit your changes.

## installation and configuration

```sh
bunx @jackyliu132/lazycommit@latest config
```

set up with your openai api key & preferred model:

## usage

You can specify custom templates. use `bunx @jackyliu132/lazycommit@latest config` to edit the templates.

### as a menu

this creates a menu of commit messages based on the diff between the current branch and master.

insert the following custom command into your [lazygit](https://github.com/jesseduffield/lazygit) config file:

~/.config/lazygit/config.yml
```yaml
customCommands:
      - key: "<c-a>" # ctrl + a
        description: "pick AI commit"
        command: "cat << 'EOF' | git commit -F -\n{{.Form.Msg}}\nEOF"
        context: "files"
        prompts:
          - type: "menuFromCommand"
            title: "ai Commits"
            key: "Msg"
            command: "npx @jackyliu132/lazycommit@latest"
            filter: '^(?P<number>\d+)\.\s(?P<message>.+)$'
            valueFormat: "{{ .message }}"
            labelFormat: "{{ .number }}: {{ .message | green }}"
```

### with neovim

this allows you to edit the commit message in vim after you've selected it from the menu.

abort committing by deleting the commit message in vim.

```yaml
customCommands:
  - key: "<c-a>" # ctrl + a
    description: "Pick AI commit"
    command: 'echo "{{.Form.Msg}}" > .git/COMMIT_EDITMSG && nvim .git/COMMIT_EDITMSG && [ -s .git/COMMIT_EDITMSG ] && git commit -F .git/COMMIT_EDITMSG || echo "Commit message is empty, commit aborted."'
    context: "files"
    subprocess: true
    prompts:
      - type: "menuFromCommand"
        title: "AI Commits"
        key: "Msg"
        command: "npx @jackyliu132/lazycommit@latest"
        filter: '^(?P<number>\d+)\.\s(?P<message>.+)$'
        valueFormat: "{{ .message }}"
        labelFormat: "{{ .number }}: {{ .message | green }}"
```

## Fixes in this version

- Fixed `<no value>: <nil>` issue in lazygit when no changes are staged
- Tool now exits silently instead of outputting error messages that don't match lazygit's regex pattern

## acknowledgements

- check out original project [bunnai](https://github.com/chhoumann/bunnai).
- Fork of [@m7medvision/lazycommit](https://github.com/m7medvision/lazycommit) with bug fixes
- check out these other projects that inspired this one:

- https://github.com/BuilderIO/ai-shell

## Custom Endpoint

You can now specify a custom endpoint for using OpenAI-like APIs. To set a custom endpoint, use the `bunx @jackyliu132/lazycommit@latest config` command and select the "Custom Endpoint" option. This allows you to use different API endpoints that are compatible with OpenAI.
