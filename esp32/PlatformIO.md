# PlatformIO

## monitor_speed

in `platformio.ini`:

```
monitor_speed = 115200
```

## ExtraScripts:

in `platformio.ini`:

```
extra_scripts = pre:apply_patches.py
```

in `apply_patches.py`:

```python
from os.path import join, isfile

Import("env")

FRAMEWORK_DIR = env.PioPlatform().get_package_dir("framework-arduinoespressif32")
patchflag_path = join(FRAMEWORK_DIR, ".patching-done")

# patch file only if we didn't do it before
if not isfile(join(FRAMEWORK_DIR, ".patching-done")):
    original_file = join(FRAMEWORK_DIR, "libraries",
                         "HTTPClient", "src", "HTTPClient.cpp")
    patched_file = join("src", "patches", "HTTPClient.cpp")

    print(original_file)
    print(patched_file)

    assert isfile(original_file) and isfile(patched_file)

    env.Execute("cp %s %s" % (original_file, patched_file))
    # env.Execute("touch " + patchflag_path)


    def _touch(path):
        with open(path, "w") as fp:
            fp.write("")

    env.Execute(lambda *args, **kwargs: _touch(patchflag_path))

```
