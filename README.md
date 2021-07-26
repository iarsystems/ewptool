## Tutorial</br>Getting Started with the EWPtool

If you want to be notified in your GitHub inbox about updates to this tutorial, you can start __watching__ this repository. You can customize which types of notification you want to get. Read more about [notifications](https://docs.github.com/en/github/managing-subscriptions-and-notifications-on-github/setting-up-notifications/about-notifications) and how to [customize](https://docs.github.com/en/github/managing-subscriptions-and-notifications-on-github/setting-up-notifications/about-notifications#customizing-notifications-and-subscriptions) them.

If you end up with a question specifically related to [this tutorial](https://github.com/iarsystems/project-migration-tools), you might be interested in verifying if it was already answered from [earlier questions][repo-old-issue-url]. Or, [ask a new question][repo-new-issue-url] if you could not find any answer for your question.

## Frequently Asked Questions
### What is the EWPtool utility?
__EWPtool__ is a small utility that can help to speed-up when populating an __IAR Embedded Workbench__ project with an existing source code tree.

Currently the IDE allows adding source files from a single folder at a time, which is fine for a few folders. Although, this process can become time-consuming in cases where there are too many folders. The EWPtool is an external utility that integrates pretty well to the IDE, allowing it to perform the task of populating a project with an entire source tree at once. The bigger the source tree is, the bigger is the benefit this utility offers.

### What does EWPtool not do?
__EWPtool__ does __not__ directly convert existing project files created with 3rd-party IDEs.

Some flavors of the IAR Embedded Workbench offer built-in Project Converter tools that can convert projects created using 3rd-party IDEs. The __EWPtool__ utility is not a requirement for when using those tools. In those cases, you can safely disregard this tutorial. Instead, please visit the "Migration Guides" section in the official [Project Migration tools](https://www.iar.com/products/project-migration-tools/) page for more information specific to those tools.

### What is required for the EWPtool to function?
The __EWPtool__ uses the IPCF (_IAR Project Connection File_) feature. This feature is implemented on any reasonably recent versions of the __IAR Embedded Workbench IDE__.

Make sure this feature is enabled in `Tools` → `Options` → `Project` → `Enable Project Connections`.

![ide-options](images/ide-options.png)


## Installation
The installation of the __EWPtool__ utility is simple. Please follow the steps below:

1. Close all the instances of the __IAR Embedded Workbench IDE__.
2. Download the archive with the latest release of the [EWPtool utility](/common.zip). <!-- TODO: add released package URL -->
3. Extract the __ewptool.zip__ archive contents inside the `<path-to>/<iar-embedded-workbench-installation-folder>`, on top of its existing `common` folder.
4. Launch the __IAR Embedded Workbench IDE__.

>:warning: If multiple instances of the _IAR Embedded Workbench_ are installed on different locations, this __EWPtool__ installation process must be repeated for each instance in which the __EWPtool__ is going to be used with.


## General usage guidelines
The project layout in the IAR Embedded Workbench is logical. This means that, whenever desired, source files could be added and grouped in a completely different way than the way they are actually arranged in the filesystem.

The __EWPtool__ populates a project reflecting the source tree layout so the logical layout matches the layout from the filesystem.


## General usage guidelines
The project layout in the IAR Embedded Workbench is logical. This means that, whenever desired, source files could be added and grouped in a completely different way than the way they are actually arranged in the filesystem.

The __EWPtool__ populates a project reflecting the source tree layout so the logical layout matches the layout from the filesystem.

### Adding the project's sources
This section provides a general overview on how the __EWPtool__ can be used to quickly populate projects with existing source code.

>:warning: All the examples below are solely for illustrative purposes.

* Create a __New Empty Project__ by choosing `Project` → `Create New Project` → `Empty Project` → `OK`.

* A __Save As__ dialog will show up. Save the IAR **E**mbedded **W**orkbench **P**roject file (`<project-name>.ewp`) in the project's source tree top level folder.

* Select `Tools` → `Add new source folder…`.

![ewptool-menu-entry](images/7.png)

* A new window titled __Browse for Folder__ will show up pointing initially to the folder with the project file. From there, select the desired source tree folder that will be appended to the active project. Usually the project's top directory will be chosen. For example:

![ewptool-browser](images/8.png)

>:warning: Please notice that the __EWPtool__ is also able to add folders that are located on any upper or lower level relative to the project file, as far as the chosen folder belongs to **the same drive in which the project file is stored (i.e. C:, D:, etc)**. You can read more about it in the section [Appending extra source code](#appending-extra-source-code) below.

* The IDE will then tell you that the `<path-to>/<project>.ewp` project file has been modified on "disk" and will offer to reload the project. This happens because the __EWPtool__ automatically added a `<project>.ipcf` file to the project layout. Click on the `Yes` button to reload the project.

![dialog-project-reload](images/9.png)

* Reloading the project with the `<project>.ipcf` will recursively append the existing source files from the previously selected folder to the current project layout. The result can be verified by unfolding the group named `src`, located on the project's layout top level in the `Workspace window`. For example:
 
![workspace-initial-project-layout-tree](images/10.png)

>:warning: If desired, the current layout for these groups can be tweaked later on. Please refer to [Customizing the project tree layout](#customizing-the-project-tree-layout) for more information.

### Appending extra source code
Many embedded software projects use 3rd-party source code to support the actual application. If these components were located within the project's source tree, they would be already appended to the project layout from the time the project's directory was selected.

Still, there are more complex scenarios where pieces of the application might be located on a parent level, outside the project's source tree. Perhaps because these components are used on other projects and are maintained separately. If that's the case, the __EWPtool__ utility also can help populate the project with them.

For this example, let's consider adding a folder named `3rd-party-components`, located one level above (`..`) the project's directory (__$PROJ_DIR$__), using the __EWPtool__ utility a second time:

![add-3rd-party-code](images/11.png)

When adding directories located above __$PROJ_DIR$__, one or more groups named `..` are created as reference to how many levels above the __$PROJ_DIR$__ the extra source folders are located, reflecting the arrangement of these source files on the filesystem. On the logical layout this grouping helps to identify where the extra source files are located relatively to the __$PROJ_DIR$__.

![workspace-upper-folders](images/12.png)

>:warning: [$PROJ_DIR$](https://wwwfiles.iar.com/arm/webic/doc/EWARM_IDEGuide.ENU.pdf#page=85) is an IDE's internal [argument variable](https://wwwfiles.iar.com/arm/webic/doc/EWARM_IDEGuide.ENU.pdf#page=85) which translates to the absolute path for the location where the project file is stored.

### Preprocessor entries
By default, a project created with the IAR Embedded Workbench will search for C/C++ header files in the toolchain's headers default locations as well as in the $PROJ_DIR$. 

The IDE provides a simple way for other search locations to be added from the Project Options (`C/C++ Compiler` → `Preprocessor` → `Additional Include Directories: (one per line)`).

When one or more source folders are added to the project using the __EWPtool__, it will recursively detect any folders containing header files (any files with the __.h__ or the __.hpp__ extensions). Then it will fill the C/C++ Preprocessor entries accordingly. For example:

![options-compiler-preprocessor](images/13.png)

This automation will propagate through every existing build configuration within the project (i.e. "Debug", "Release", etc.).

:warning: The same automation will populate with the __Additional include directories__ for the __Assembler Preprocessor__ (`Assembler` → `Preprocessor` → `Additional Include Directories: (one per line)`).

### Source code detection
The __EWPtool__ utility detects source file types by their extension. The file extensions that are automatically detected are specified in the table below.

| Source file type | Extension           |
|------------------|---------------------|
|C sources         | __*.c__             |
|C++ sources       | __*.cc__, __*.cpp__ |
|Assembly sources  | __*.s__             |
|Static libraries  | __*.a__             |


## Customizing the project tree layout
Once we get all the desired files automatically appended to the project, we can think of ways of customizing the project layout in the __Workspace window__, according to any particular preferences.

Let's now consider that the `3rd-party-components` group should be on the same level of the `src` group. Also, `src` might not be as descriptive as `Application` for one's tastes, and so on.

### Removing the IAR Project Connection File (IPCF)
The nodes layout inside the `src` group is __persistent__. This happens while they are managed by the IAR Project Connection File (`*.ipcf`). This property is also true for the additional directories in the preprocessor settings. Severing this connection will cease this property and all the persistent nodes will become editable again. In order to get to that, the `<project>.ipcf` must first be removed from the project. If that's the case, please proceed as follows: 

* Right-click on the `<project>.ipcf` file and choose `Remove` from its context menu. (Alternatively, it is possible to highlight the file and press the <kbd>DEL</kbd> key.)

* A question dialog will pop-up for confirmation of its removal. Confirm the removal by clicking `Yes`.

![ide-pm-remove-ipcf-node](images/14.png)

>:warning: Removing the `<project>.ipcf` file from the project will not delete the file from its original location within the filesystem.

### Rearranging the project layout 
Once the `<project>.ipcf` is removed from the project, it becomes possible to rearrange groups (and/or files) nodes created with the __EWPtool__ within the __Workspace window__. This normally can be performed through simple drag'n drop operations. 

* Dragging the `3rd-party-components` group and dropping it on top of the project's name will move it, as shown below:

![workspace-move-group](images/15.png)

>:warning: After moving the `3rd-party-components` node to the project's layout top level, the group `..` became empty, hence it can be safely removed from the project layout using the <kbd>DEL</kbd> key.
 
* Renaming the `src` group can be performed by a right-click on top of this group followed by the `Rename...` command:

![workspace-rename-group](images/16.png)

* A modal window titled `Rename group <current-group-name>` will show up. Fill with the desired string (i.e. `Application`) and finish the operation by clicking on `OK`:

![rename-group-modal](images/17.png)

* Once you have the project layout the way you see fit, save the project by choosing `File` → `Save All` or else click the `Save All` icon in the __main toolbar__.

![ide-save-all](images/18.png)

### Excluding sources from the build configuration
There are cases where not every single source file present in a folder should be used on a build configuration at the same time. Perhaps multiple implementations of the same functions but with different trade-offs. In those cases, a choice should be made.

In our fictional project example, this happens with the __buffer__ component where a type choice must be made among the `type A`, `type B` and `type C`. 

Let's say we decide that our project needs the `type A` buffer implementation. We need to exclude `type B` and `type C` from the current build configuration. 

There are at least a couple of ways of accomplish this in the __Workspace window__:

- __Remove the undesired files from the project layout__:
   - Highlight each of the undesired file nodes (`buffer_typeB.c` and `buffer_typeC.c`).
   - Remove them from the project layout by pressing the <kbd>DEL</kbd> key.
or...

- __Exclude the undesired files from the build__:
    -  For each of the undesired file nodes: right-click and select: `Options` → `Exclude from build`.
or...

- __Create an "excluded" group__:
   - Create a new group (i.e. `excluded`) inside the desired group.
   - Right-click on the `excluded` group and select: `Options` → `Exclude from build`.
   - While holding <kbd>SHIFT</kbd>, select multiple files (i.e. click on `buffer_typeB.c` followed by `buffer_typeC.c`).
   - Finally drag the selection and drop these files inside the `excluded` group. By consequence, they will be excluded from the "Debug" build configuration.

![project-exclude-group](images/exclude.png)

:warning: Notice that when a group or a file is __excluded from build__, its icon becomes grayed.

## What are the next steps?
The main purpose of the __EWPtool__ is to help you to quickly populate your new (or existing) project with all the necessary source files, headers and static libraries. This tool can help save a huge amount of time, especially when it comes to bigger projects. Although, we are not done yet. There are some extra adjustments to look for when migrating projects like this, so it gets properly configured. It needs to be  in accordance with the used target device, library configuration, the compiler’s optimization levels, linker configurations, debug probe driver and so on. Even then, the __IAR Embedded Workbench__, for all of its supported architectures, makes very simple and quick to change those settings.

### Target Device Selection
Open the project’s options with `Project` → `Options (ALT+F7)` and navigate to the `General Options` → `Target`. This tab will allow you to select the target device and it may present itself in a slightly differently yet familiar manner, depending on the __IAR Embedded Workbench__ architecture in use. Below you can see how the `Target` tab looks like for *Arm* and for *RISC-V*:  

![optTargetArm](images/19.png)   ![optTargetRISCV](images/20.png)

### Library Configuration
Once you chose the device, you could move to the __Library Configuration__ tab to check some options. For *Arm* users, this tab is especially interesting if working with CMSIS-based projects; case in which, the `Use CMSIS` option should be selected.

![optLibConfig](images/21.png)

### Compiler's optimization levels
On the `C/C++ Compiler` category, under the `Optimizations` tab you can easily choose the optimization level from __None__ up to __High__. When __High__ is selected, it is possible to select among 3 different optimization objectives:

- `Size`, which means the smallest code size. Typically used to save flash memory resources when flash memory cost is more important than speed.

- `Speed`, which means a much faster code in which typically incurs some extra flash memory consumption as a trade-off.

- `Balanced`, will use heuristics with the objective of making smart decisions on every piece of code in order to make it run as fast as possible if it doesn’t come with a great toll in the code size.

![optLibOptimizations](images/22.png)

The `Enabled Transformations` can significantly affect the code generation. In order to get a better correlation between the written sources and the produced code during the debugging phase, it is recommended to leave the compiler optimization level on __None__ or __Low__ for *Debug*. Then, later on, as the application becomes ready for *Release* and the code has been already debugged, the level can be raised to __High__.

### Compiler's preprocessor
Another tab which is commonly used when migrating projects to the __IAR Embedded Workbench__ is the `Preprocessor` tab in the `C/C++ Compiler` category. This is where symbols could be specified. One possible scenario, for example, would be checking an originating *Makefile* in order to find any required symbols. Once they are found, it is just about bringing these symbols to the `Defined Symbols` box. For example: 

![optICCpreprocessor](images/23.png)

### Linker configuration
Once you have your [target device selected](#target-device-selection), the linker configuration file (*.icf*) will automatically change to one common configuration, as it can be seen on the `Config` tab, under the `Linker` category. The default linker configuration should allow the programs with no specific flash partitioning requirements (*as it would be, for example, in a bootloader application*) to run with no issues. Even then, if a specific configuration is needed for the project, it is just about enabling `Override default` for the configuration:

![optICClinkerConfig](images/24.png)

### Debugger Configuration
The last essential category to keep in mind when migrating projects to the __IAR Embedded Workbench__ is the `Debugger` category. Here you will find the `Setup` tab, where you can easily choose among several different supported debugging probes. For the optimal experience, we recommend the [__IAR I-jet probes__](https://www.iar.com/iar-embedded-workbench/add-ons-and-integrations/in-circuit-debugging-probes/). Worth mentioning that all the different __IAR Embedded Workbench__ architecture flavors comes with an integrated `Simulator` which allows you, for example, to play with your project in case the target board isn’t ready yet. To learn more about the `Simulator`, interrupt simulation and the __C-SPY macros__, visit `Help` → `Information Center` → `Product explorer` and try those tutorials.

![optDebuggerSetup](images/25.png)

### Summary
This short *Getting Started tutorial* is far from being a complete walkthrough guide which would present every possible option or particular occasion which could pop-up when migrating projects. Even then, the techniques presented here are good hints and powerful shortcuts for moving faster across the essential steps that pave the way to unleash the full potential on the *IAR Embedded Workbench* IDE. The [User guides](https://www.iar.com/support/user-guides/user-guide-iar-embedded-workbench-for-arm) included in the `Help` → `Information Center` → `User guides` can also be of great help in this process.  

![ewarmInfoCenter](images/26.png)

---
If you have questions regarding this repository contents, you can start by checking its [issue tracker][repo-old-issue-url] for the previously asked questions.
If it is a new question, feel free to post it [here][repo-new-issue-url].

<!-- Links -->
[repo-new-issue-url]: https://github.com/IARSystems/project-migration-tools/issues/new
[repo-old-issue-url]: https://github.com/IARSystems/project-migration-tools/issues?q=is%3Aissue+is%3Aopen%7Cclosed
