# PyBuild
A simple build system made completely in Python. It acts as a Python library allowing for scripting in Python, which makes it very versatile.
# Usage
Using the Python interpreter ( >= Python 3 ) call the build Python script supplied with any project and not the `pybuild.py` file. The correct syntax for any script is `python <script> <target name>`. A target name allows for multiple mini-scripts in one file. For example each project can have a "build" target and a "clean" target. Note that if a script contains only one target, `<target name>` can be omitted and the sole target will run.
# Example Build Script
```py
"""
In this example the file structure is as follows:
example/
|-- src/
|   |-- hellofunc.cpp
|   |-- hellomake.h
|   |-- main.cpp
|-- bin/ -> will be filled at build
|-- system/
|   |-- pybuild.py
| -- run.py -> actual script

"""

from system.pybuild import * # pybuild.py is in a directory called system
from sys import argv

maker = PyBuildArgMaker()
src_files = []
obj_files = []


def target_all():
    src_files = batchAction(listFiles("src"),
                            lambda f: os.path.join("src",f) if getFileExtension(f) == ".cpp" else None)
    obj_files = []
    for src in src_files:
        obj = src.replace("src\\","bin\\").replace(".cpp",".o")
        obj_files.append(obj)
        v = runCommand(["g++","-c","-Isrc/",src,"-o",obj],shell=True) # shell = True for windows
    runCommand_os(f"g++ -o bin\\hello.exe {' '.join(obj_files)}")

def target_clean():
    clearDir("bin")

maker.addTarget("all",target_all)
maker.addTarget("clean",target_clean)


if __name__ == "__main__":
    maker.run(argv)


```

# Documentation
There are no documentation files. All functions are self-explanatory and on top of every function, a comment describes its action, all types in each function argument and its return type are described using the Python 3.5 feature called `typing`.
