# Fully hermetic bazel build with nix, without /nix/store

This is an experiment in fully hermetic, but also self-installing bazel builds.

The build rules at https://github.com/tweag/rules_nixpkgs allow bazel to bring
in dependencies from nixpkgs. But, it requires having a `/nix/store` on your machine,
which in turn means you need to have a pre-existing system-wide nix installation.

This example repo adds the dir `//tools` to an existing `rules_nixpkgs` example,
which makes a stand-alone and ephemeral nix installation in your bazel cache,
and prepares all dependencies for compilation.  It then builds a hello world
app.

1. install bazelisk, name it `bazel`
2. try: `bazel run :hello`

# Bugs

* I displaced bazel's cache to a nix directory somehow. That's not a good idea.

---

Original README.md below.

---

# bazel-nix-flakes-example

The example is generating a local nixpkgs repository using the `flakes.lock` file already present on
[flakes](https://nixos.wiki/wiki/Flakes) projects.

## Requirements

The nix package manager should be installed with flakes support enabled.

## Running the example

The local nixpkgs repository can be used by explicitly specifying the generated toolchain.

``bash
nix-shell --run "bazel run --crosstool_top=@nixpkgs_config_cc//:toolchain :hello"
```
