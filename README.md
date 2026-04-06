# Roblox CLI research

**roblox-cli** is an internal command-line interface tool for interacting with the Roblox Engine, primarily used for automation, testing, and CI workflows.

It's mentioned and used in repo's such as [jest-roblox](https://github.com/Roblox/jest-roblox), [react-luau](https://github.com/Roblox/jest-roblox) and [chalk-lua](https://github.com/Roblox/chalk-lua).

There seems to be two versions of the it, robloxdev-cli and roblox-cli. The difference between the two is not currently known. I'm guessing they have slightly differing features.

## **Roblox CLI Documentation**

All this information is from publicly accessible sources.

## ***run*** **Subcommand**

The run subcommand is the primary way to launch a Roblox environment (headless DataModel to execute scripts, run tests, or perform benchmarks on projects)

### **Usage**

`roblox-cli run \[OPTIONS\]`

### **Core Arguments**

These flags define the asset to be loaded into the environment:

| Flag | Description | Examples |
| :---- | :---- | :---- |
| \--load.place \<path\> | Loads a Roblox Place file (.rbxlx, .rbxl) into the environment. | \--load.place TestPlace.rbxlx |
| \--load.model \<path\> | Loads a Roblox Model file (.rbxm, .rbxmx) or a rojo project file (.project.json) into the environment and parents it to ReplicatedStorage. | \--load.model ci.rbxm, \--load.model tests.project.json |

### **Execution Arguments**

These flags control what script is executed and how

| Flag | Description | Examples |
| :---- | :---- | :---- |
| \--run \<path\> | Specifies the path to the Luau script file to be executed upon environment startup. This is typically the entry point for testing or automation. Script is most likely parented to TestService. | \--run scripts/run-first-render-benchmark.lua |
| \--lua.globals=\<var\>=\<value\> | Defines global Luau variables (\_G). Useful for setting environment flags or mock values. | \--lua.globals=\_\_DEV\_\_=true, \--lua.globals=\_\_ROACT\_17\_MOCK\_SCHEDULER\_\_=true |
| \--fs.read=\<path\>, \--fs.readwrite=\<path\> | Enables FileSystemService and the provided operations (read or read and write). FileSystemService is sandboxed to only files and directories under the path specified. | \--fs.readwrite=$PWD, \--fs.read=$PWD, \--fs.read=/root/code/ |
| \--load.asRobloxScript | Loads the main execution script as an elevated Roblox Script (ElevatedGameScript) rather than a Script or other context. | \--load.asRobloxScript |
| \--coverage.on | Turns code coverage reporting on. |  |
| \--coverage.includes \<files\> | Specifies which files should be included in the coverage report | \--coverage.includes src/\* |

### **Environment & Configuration Flags**

These flags configure the behavior of the launched Roblox environment:

| Flag | Description | Examples |
| :---- | :---- | :---- |
| \--headlessRenderer \<value\> | Controls whether the engine runs with a headless graphical interface. **1**, **true** or **on** enables it.  | \--headlessRenderer 1, \--headlessRenderer on, \--headlessRenderer=true |
| \--virtualInput \<value\> | Enables or disables virtual input (e.g., simulating mouse and keyboard events) in a headless environment. **1**, **true** or **on** enables it. | \--virtualInput on \--virtualInput=true |
| \--fastFlags.allOnLuau | Enables all relevant Luau-related Fast Flags (FFlag). | \--fastFlags.allOnLuau |
| \--fastFlags.overrides "\<flag\>=\<value\>" | Overrides specific Fast Flags with custom values.| \--fastFlags.overrides "EnableLoadModule=true", \--fastFlags.overrides "UseDateTimeType3=true" "EnableLoadModule=true" |
| \--testService.errorExitCode=\<value\> | Sets the roblox-cli's exit code upon test failure (as reported by TestService). Used to signal CI pipelines when tests fail. | \--testService.errorExitCode=1 |

---

## ***analyze*** **Subcommand**

The analyze subcommand is used to perform static analysis, linting, or checking on project files without launching the full Roblox environment.

### **Usage**

`roblox-cli analyze \<path\_to\_project\_file\>`

### **Arguments**

| Argument | Description | Example |
| :---- | :---- | :---- |
| \<path\_to\_project\_file\> | The path to the project file (e.g., .project.json) to be analyzed. The CLI will inspect the project structure and associated scripts. | test-bundle.project.json |

---

## ***pack*** **Subcommand**

Builds a .rbxp (internal) project into a Roblox Binary Model (.rbxm/.rbxmx). Similar to `rojo build`.

### **Usage**

`roblox-cli pack --input <path_to_project_file> --output <path_to_output_file>`

## ***roblox-cli*** **Services**

### **FileSystemService**

**FileSystemService:WriteFile(path: string, content: string)**

- Writes a file to disk

**FileSystemService:ReadFile(path: string): string**

- Reads a file from disk

**FileSystemService:CreateDirectories(path: string)**

- Creates directories from the specified path

**FileSystemService:Exists(path: string): boolean**

- Checks if a directory or file exists at specified path

**FileSystemService:IsRegularFile(path: string): boolean**

- Checks if the file at the specified path is a regular file

**FileSystemService:Remove(path: string)**

- Removes the file at the specified path

**FileSystemService:RemoveAll(path: string)**

- Recursively deletes all files at the specified path

### **ProcessService**

**ProcessService:ExitAsync(code: number)**

- Exits roblox-cli’s process with the specified code

**ProcessService:GetCommandLineArgs(): {string}**

- Returns the command line args passed to roblox-cli

---

