# Modocil
A compile-time **Jai Module Organizer**.

The goals are:
- A standardized way to document external dependency/versions. [WIP]
- A standardized way to store licenses.
- Checked consistency between specified modules/versions, the module code, and their licenses.
- Prevent long and wide dependency chains.

What Modocil does:
- Nothing if the dependencies are already there.
- Download and cache git/path modules.
- Place licenses in the licenses directory.

Modocil is a Jai module itself. It contains normal Jai procedures and is meant to be called from a build workspace prior to the main library or application that uses the external modules.

## Requirements
For GitModules `git` needs to be installed and available in the PATH.

## Usage
1. Place `Modocil.jai` inside the modules directory.
1. Add a fetch statement to the **build workspace** for each dependency module.
   >NOTE: If you do not already have a build workspace you'll need to create one, see [example: one](./examples/one/first.jai).
   ```jai
   #run {
       #import "Modocil";
       fetch(GitModule.{
            name = "Do_A_Thing",
            url = "git@github.com:sjorsdonkers/modocil.git",
            path = "./examples/assets/Do_A_Thing.jai"});
       ...
   }
   ```
1. [recommended] Update modules when the `- update` given. Like: `jai .\first.jai - update`
   ```jai
   args := get_build_options().compile_time_command_line;
   do_update := array_find(args, "update");
   fetch(PathModule.{...}, do_update);
   ```
1. [optional] Enable Modocil to update itself by adding it as a dependency.
   >NOTE: Does not work if Modocil is placed in the global modules directory.
   ```jai
   fetch(GitModule.{
            name = "Modocil",
            url = "git@github.com:sjorsdonkers/modocil.git",
            path = "Modocil.jai"}, 
        do_update);
   ```

### Limitations:
- Modocil cannot fetch dependency modules for the workspace it itself is in.
- Defining the dependencies in code prevents other tooling from processing the data.
