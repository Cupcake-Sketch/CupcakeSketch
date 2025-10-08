# Reproduction Guide

This guide will help you reproduce our work. Please follow the steps below carefully.

## 1. Environment Setup

### Prerequisites
- **Operating System**: Linux-based system (tested on Ubuntu 20.04+)
- **Compiler**: GCC with C++17 support
- **Build Tools**: Make
- **Debugger**: GDB

### Required Dependencies
Based on the VSCode configuration, this project uses standard C++ libraries including:
- STL containers (`vector`, `deque`, `map`, `unordered_map`, etc.)
- File handling (`fstream`)
- Timing and chrono utilities
- Mathematical functions
- Random number generation

Install essential build tools on Ubuntu:
```bash
sudo apt update
sudo apt install build-essential gdb make
```

### VSCode Configuration (Optional)
If you're using VSCode for development/debugging, you can set up the following configurations:

Create `.vscode/launch.json`:
```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "makefile run",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/exec/exp",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "clean-build-run",
            "miDebuggerPath": "/usr/bin/gdb"
        },
        {
            "name": "makefile debug",
            "type": "cppdbg",
            "request": "launch",
            "program": "${workspaceFolder}/exec/debug.out",
            "args": [],
            "stopAtEntry": false,
            "cwd": "${workspaceFolder}",
            "environment": [],
            "externalConsole": false,
            "MIMode": "gdb",
            "setupCommands": [
                {
                    "description": "Enable pretty-printing for gdb",
                    "text": "-enable-pretty-printing",
                    "ignoreFailures": true
                }
            ],
            "preLaunchTask": "clean-build-debug",
            "miDebuggerPath": "/usr/bin/gdb"
        }
    ]
}
```

Create `.vscode/tasks.json`:
```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "clean",
            "command": "make",
            "args": ["clean"],
            "type": "shell"
        },
        {
            "label": "build-debug",
            "command": "make",
            "args": ["debug"],
            "type": "shell"
        },
        {
            "label": "clean-build-debug",
            "dependsOn": ["clean", "build-debug"]
        },
        {
            "label": "build",
            "command": "make",
            "args": ["all"],
            "type": "shell"
        },
        {
            "label": "clean-build-run",
            "dependsOn": ["clean", "build"]
        }
    ]
}
```

Create `.vscode/tasks.json`:
```json
{
    "files.associations": {
        "*.tcc": "cpp",
        "fstream": "cpp",
        "chrono": "cpp",
        "array": "cpp",
        "atomic": "cpp",
        "bit": "cpp",
        "cctype": "cpp",
        "clocale": "cpp",
        "cmath": "cpp",
        "compare": "cpp",
        "concepts": "cpp",
        "cstdarg": "cpp",
        "cstddef": "cpp",
        "cstdint": "cpp",
        "cstdio": "cpp",
        "cstdlib": "cpp",
        "cstring": "cpp",
        "ctime": "cpp",
        "cwchar": "cpp",
        "cwctype": "cpp",
        "deque": "cpp",
        "map": "cpp",
        "string": "cpp",
        "unordered_map": "cpp",
        "vector": "cpp",
        "exception": "cpp",
        "algorithm": "cpp",
        "functional": "cpp",
        "iterator": "cpp",
        "memory": "cpp",
        "memory_resource": "cpp",
        "numeric": "cpp",
        "random": "cpp",
        "ratio": "cpp",
        "string_view": "cpp",
        "system_error": "cpp",
        "tuple": "cpp",
        "type_traits": "cpp",
        "utility": "cpp",
        "initializer_list": "cpp",
        "iosfwd": "cpp",
        "iostream": "cpp",
        "istream": "cpp",
        "limits": "cpp",
        "new": "cpp",
        "numbers": "cpp",
        "ostream": "cpp",
        "stdexcept": "cpp",
        "streambuf": "cpp",
        "cinttypes": "cpp",
        "typeinfo": "cpp"
    }
}
```

## 2. Dataset Preparation

### Download Pre-processed Datasets
All datasets used in this project are available at:  
**https://github.com/Cupcake-Sketch/CupcakeSketch/releases/tag/Dataset**

Download and extract the datasets to the appropriate directory in the project.

### Generate Zipf Dataset
The Zipf distribution dataset can be generated using the provided script:
```bash
python scripts/zipf_generate.py
```

### Process Wikipedia Dataset
For the Wikipedia dataset:
1. First download the raw data from Wikipedia's official website
2. Then preprocess it using our script:
```bash
python scripts/wiki_preprocess.py [path_to_raw_wikipedia_data]
```

## 3. Program Usage

### Automated Compilation and Execution
We provide a convenient script to automatically compile and run the program with various configurations:

```bash
./scripts/make_run.sh
```

### Customizing Parameters
You can modify the macro definitions in `scripts/make_run.sh` to customize the experiment parameters:

Key parameters include:
- `USE_TOWER`: Tower sketch usage flag
- `USE_CS`: Count sketch usage flag  
- `METRICS`: Metrics collection flag
- `MEMORY_1_24`: Memory allocation size
- `K`: Top-K parameter
- `DATASET`: Dataset name ("caida", "webdocs_form01", etc.)
- `ITEM_SIZE`: Size of each item
- `REPEAT`: Number of repetitions
- `S_FACTOR`: Sampling factor
- `HEAVY_BIAS`: Heavy hitter bias parameter
- `SWAP_FACTOR`: Swap factor parameter
- `TIMES_RESAMPLE`: Resampling times
- `DHASH`: Hash function flag

### Manual Compilation
If you prefer to compile manually:

```bash
# Clean previous builds
make clean

# Build with custom flags
make all MYFLAGS="-DCOSTUM -DUSE_TOWER=1 -DUSE_CS=1 ..."

# Run the executable
./exec/exp
```

### Debug Build
For debugging purposes:
```bash
make debug
./exec/debug.out
```

The script automatically tests different parameter combinations and runs the experiments sequentially. Check the output files for results.
