# caddy-json-schema

JSON schema generator for Caddy v2. (@fedorg fix)
If you just want to use it in vscode, install the [YAML extension](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml) and paste this into your `.vscode/settings.json`:
```
  "yaml.schemas": {
    "https://gist.githubusercontent.com/fedorg/616a8d367c481024766fc1d4712f8715/raw/1763d76692fa22f6fbdf47333fe361ca894fbf9b/caddy_schema.json": [
      "*caddy*.yaml",
      "*caddy*.yml"
    ]
  }
```
Alternatively, paste this at the top of your YAML file (extension still needed):
`# yaml-language-server: $schema=https://gist.githubusercontent.com/fedorg/616a8d367c481024766fc1d4712f8715/raw/1763d76692fa22f6fbdf47333fe361ca894fbf9b/caddy_schema.json`

The generated schema can be integrated with editors for intellisense and better experience with configuration and plugin development.

![Demonstration](https://github.com/abiosoft/caddy-json-schema/blob/master/gif/schema.gif)

## Installation

To build a caddy binary in current dir and generate a .vscode schema from scratch, use these commands:
```
docker rm cjs || true
docker run --name cjs caddy:2-builder-alpine xcaddy build v2.4.6 --with github.com/fedorg/caddy-json-schema
docker cp cjs:/usr/bin/caddy .
./caddy json-schema --vscode
docker rm cjs
```

The generated schema is for the caddy binary. i.e. all modules in the binary will
be include in the schema.

```sh
xcaddy build v2.4.1 \
    --with github.com/abiosoft/caddy-json-schema \
    # any other module you want to include in the generated schema
```

## Usage

Run `caddy help json-schema` to view help.

```
usage:
  caddy json-schema [--output <file>] [--indent <int>] [--vscode] [--no-cache]

flags:
  -indent int
        Number of spaces to indent the generated JSON with (default 2)
  -no-cache
        Discard local cache and fetch latest API docs
  -output string
        The file to write the generated schema (default "./caddy_schema.json")
  -vscode
        Generate VSCode configuration
```

## Editors

### Visual Studio Code

`caddy json-schema --vscode` generates Visual Studio Code configuration in the current directory.

Open the directory in Visual Studio Code and it should just work.
Ensure the config filename is of the format `*caddy*.[json|yaml]`.

**Note** that you need [vscode-yaml](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml) plugin to get similar experience for YAML files.

### Vim/NeoVim

There are multiple Vim/NeoVim plugins with language server and JSON schema support.

Below is a config for [coc-json](https://github.com/neoclide/coc-json) and [coc-yaml](https://github.com/neoclide/coc-yaml). The path to schema file is relative to the config file being edited.

```json
{
  "json.schemas": [
    {
      "fileMatch": ["*caddy*.json"],
      "url": "./caddy_schema.json"
    }
  ],
  "yaml.schemas": {
    ".vscode/caddy_schema.json": ["*caddy*.yaml", "*caddy*.yml"]
  }
}
```

## Features

| Modules     | Intellisense | Documentation                                          |
| ----------- | ------------ | ------------------------------------------------------ |
| Standard    | Supported    | Supported                                              |
| Third Party | Supported    | Supported (if plugin is registered on caddyserver.com) |

## License

Apache 2
