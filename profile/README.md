## Your editor runs code from strangers

Every language server, formatter and linter your editor starts is an ordinary
process with your home directory, your SSH keys, your cloud credentials and your
network. Nothing separates "index this workspace" from "read `~/.aws` and make a
request". Installing an extension installs a subprocess, and the editor asks you
nothing.

**dragon-sec** builds the parts that make that defensible.

### Ward

Vetted, reproducibly built, sandboxed language servers.

Ward sits between your editor and the language server, so it works in the editor
you already use — no new IDE, no extension rewrite. Each server runs under a
capability profile derived from what it actually needs: rust-analyzer gets the
workspace, `~/.cargo` and the toolchain, and nothing else.

The profiles are the product. A sandbox that breaks rust-analyzer is not a
security control, it is an uninstall.

**Status: early.** There is nothing to install yet. Watch
[`ward`](https://github.com/dragon-sec/ward) if you want to see it take shape.
