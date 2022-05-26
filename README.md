# Mounting Volumes for Local Out-of-Tree Dependencies

A simple example of using `Cross.toml` with volumes to ensure the project directory is mounted.

First, we create a library and a binary that relies on that library:

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
