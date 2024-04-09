# Fully hermetic bazel build with nix, without /nix/store

This is an experiment in fully hermetic, but also self-installing nix based
hermetic bazel build.

The build rules at https://github.com/tweag/rules_nixpkgs allow bazel to bring
in dependencies from nixpkgs. But, it requires having a `/nix/store` on your
machine, which in turn means you need to have a pre-existing system-wide nix
installation.

This example repo adds the dir `//tools` to an existing `rules_nixpkgs`
example, which makes a stand-alone and ephemeral nix installation in your bazel
cache, and prepares all dependencies for compilation.  It then builds a hello
world app.

1. install bazelisk, name it `bazel`
2. try: `bazel run :hello`

# Bugs

* The bazel daemon stops after a few seconds after a run. This means that every
  time you rebuild, bazel re-reads all deps. This is not OK. The daemon should
  sit around for a long time instead.

* Sometimes bazel gets confused and crashes if invoked in short succession.

* The built binaries depend on a non-existent /nix/store dir. But I think there
  is a solution to that.

---

Original README.md below.
From https://github.com/tweag/rules_nixpkgs/tree/master/examples/flakes

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
