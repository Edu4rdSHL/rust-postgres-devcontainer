# Rust and PostgreSQL devcontainer

This is a [devcontainer](https://containers.dev/) for Rust and PostgreSQL development. It is based on the official Rust devcontainer and adds PostgreSQL.

# How to use

1. Install the [Remote - Containers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension for Visual Studio Code.
2. Add a `.devcontainer/devcontainer.json` file to your repository with the following content:

```json
{
    "image": "ghcr.io/edu4rdshl/rust-postgres-devcontainer:latest",
    "customizations": {
        "vscode": {
            "settings": {
                "terminal.integrated.shell.linux": "/bin/bash"
            },
            "extensions": [
                "rust-lang.rust-analyzer",
                "ms-vscode.cpptools",
                "vadimcn.vscode-lldb",
                "ms-vscode.cmake-tools",
                "twxs.cmake",
                "fill-labs.dependi",
                "tamasfe.even-better-toml",
                "GitLab.gitlab-workflow",
                "ms-ossdata.vscode-postgresql",
                "mtxr.sqltools",
                "mtxr.sqltools-driver-pg"
            ]
        }
    },
    // Mount the workspace folder into the container
    "workspaceMount": "source=${localWorkspaceFolder},target=/home/vscode/workspace,type=bind,consistency=cached",
    "mounts": [
        // Note: The volumes cargo-cache-rust_devcontainer and postgres-rust_devcontainer are automatically used
        // to cache the cargo and postgresql data directories respectively. You can override them by setting the
        // mounts here. Example:
        //  {
        //     "source": "cargo-cache-${localWorkspaceFolderBasename}",
        //     "target": "/usr/local/cargo",
        //     "type": "volume"
        // },
        // {
        //     "source": "postgres-${localWorkspaceFolderBasename}",
        //     "target": "/var/lib/postgresql/15/main",
        //     "type": "volume"
        // }        
    ],
    "runArgs": [
        "--restart=always",
        "--name=devcontainer-${localWorkspaceFolderBasename}"
    ]
}
```
3. Install [devcontainer-cli](https://github.com/devcontainers/cli) by running `npm install -g @devcontainers/cli`.
4. Run `devcontainer up --workspace-folder .` in the repository root.
5. Attach to the devcontainer by clicking the green button in the bottom left corner of Visual Studio Code.
6. You're ready to go!

# What's included

- Rust
- PostgreSQL
- Diesel CLI
- Several useful bash_aliases

# Issues

If you encounter any issues, please open an issue on the [GitHub repository](https://github.com/Edu4rdSHL/rust-postgres-devcontainer/issues).