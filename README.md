# Mounting Volumes for Local Out-of-Tree Dependencies

A simple example of using `Cross.toml` with volumes to ensure the project directory is mounted.

We assume a project directory analogous to the following, where we have a directory containing numerous independent crates (`lib1`, `lib2`, `lib3`) used as local dependencies for another crate (`bin`).

```text
project/
    lib1/
        Cargo.toml
    lib2/
        Cargo.toml
    lib3/
        Cargo.toml
    bin/
        Cargo.toml
```

First, we create a library (`local_lib`) and a binary (`binary`) that relies on that library:

**Cargo.toml**

```toml
[dependencies]
local_lib = { path = "../local_lib" }
```

Next, we ensure that the directory provided by `PROJECT_DIR` is mounted:

**Cross.toml**

```toml
[build.env]
volumes = [
    "PROJECT_DIR",
]
```

And finally, we write a simple wrapper around cross to ensure we define `PROJECT_DIR`:

**cross.sh**

```bash
#!/bin/bash
PROJECT_DIR=.. exec cross $@
````

Building the project works with:

```bash
git clone https://github.com/Alexhuszagh/cross_issue405
cd cross_issue405/binary
 ./cross.sh run --target powerpc64le-unknown-linux-gnu
```
