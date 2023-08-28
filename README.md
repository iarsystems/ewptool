## Getting started with IAR EWPtool Utility

### What is EWPtool?
The _IAR Embedded Workbench IDE_ allows adding source files from a single folder at a time, which is fine for a few folders. Although, this process can become time-consuming in cases where there are many folders are involved.

__EWPtool__ can speed up the process of populating an IAR **E**mbedded **W**orkbench **P**roject file (`*.ewp`) by traversing an existing source code tree.

### What does EWPtool do?
From the IDE, the IAR EWPtool Utility acts as an external tool that extends its functionality. When invoked, it asks for where the project's source tree can be found. Then EWPtool automates the task of populating the project with the source code tree. The bigger the selected source tree is, the more evident its yielded benefit becomes. 

The __EWPtool__ modifies the following entries in the `.ewp` project:
* Populates the project's tree layout, reflecting the selected source code tree layout in the filesystem.
* Fills the preprocessor search paths entries, for the active _build configuration_.
* Updates moved/renamed entries from the project tree layout.
* Removes stale entries from the project tree layout.

### What does EWPtool not do?
Rest assured that:
* __EWPtool__ does __not__ modify your _source files_.
* __EWPtool__ does __not__ convert existing _project files_ created by 3rd-party IDEs.
>:bulb: About that, some target architectures of the _IAR Embedded Workbench_ might offer _built-in project converter tools_ able to deal with projects from 3rd-party IDEs. The _EWPtool utility_ is __not__ a requirement for when using those tools. In those cases, you can safely disregard this tutorial. Instead, please visit the _Migration Guides_ section in the official [Project Migration tools][url-iar-migration] page for more information specific to those tools.


## Installing
The __EWPtool__ can be used with any reasonably recent versions of the _IAR Embedded Workbench_. Installing the tool simply means deploying its files on top of an existing instance of the IDE. Please follow the steps below:

1. Close all the instances of the IDE.
2. Download the archive (EWPtool-\<version\>.zip) available from the latest [Release notes][url-repo-tool-release-rn].
3. Extract the zip archive contents inside the `<path-to>/<iar-embedded-workbench-installation-folder>/common` folder.
4. Launch the IDE.

>:warning: Please notice that, if multiple instances of the _IAR Embedded Workbench_ are installed on independent locations, __step 3.__ must be repeated for each instance in which the __EWPtool__ is going to be used with.


## Upgrading
For upgrading from previous __EWPtool__ versions, replace the old files in the `common` folder and re-launch the IDE.


## How to use it? 
The project layout in the _IAR Embedded Workbench_ is logical. This means that, whenever desired, source files could be added and grouped in a completely different way than the way they are actually arranged in the filesystem.

The __EWPtool__ populates a project by reflecting the selected source tree layout, so the logical layout will initially match the layout from the filesystem.

### Adding the project's sources
From this section onwards, we will use a fictional project containing many source folders as an example on how the __EWPtool__ can be used.

>:warning: __EWPtool__ does not create any sort of backup of the affected `<project-name>.ewp`. Before proceeding it is recommended to have a _version control system_ in place or __-at least-__ a backup copy of the project, in case you wish to rollback any performed changes.

* Create a __New Empty Project__ by choosing `Project` → `Create New Project` → `Empty Project` → `OK`.

* A __Save As__ dialog will show up. Save the `<project-name>.ewp` in the project's source tree top level folder.

>:bulb: The tool can only act upon the contents it finds on the "disk". To avoid the need of re-scanning the selected source tree twice, remember to __always save the project before__ invoking the __EWPtool__.

* Invoke the __EWPtool__: select `Tools` → `Select source folder…`.

>![ewptool-menu-entries](https://github.com/IARSystems/project-migration-tools/assets/54443595/ef59336b-b787-4a8e-9edd-a7f6856ee4a5)

* A new window titled __Browse for Folder__ will show up pointing initially to the folder where the `.ewp` file is. From there, select the desired source tree folder that will populate the active project. Usually the project's top directory will be chosen. For example:

>![ewptool-browser](https://github.com/IARSystems/project-migration-tools/assets/54443595/fc2fdba4-00d6-4bd6-8f37-8268d3df5c47)
>
>:warning: Please notice that the __EWPtool__ is also able to add folders that are located on any upper or lower level relative to the `.ewp` file, as far as the chosen folder belongs to **the same drive in which the project file is stored (i.e. C:, D:, etc.)**.
>
>:bulb: The selected folder is saved in `settings/<PROJ_FNAME>.cfg` so it can be used later with the command `Tools` → `Rescan selected source folder...`, which will skip the __Browse for folder__ dialog.

* The IDE will then tell you that the `<path-to>/<project-name>.ewp` project file has been modified on "disk" and will offer to __reload the project__. This happens because the __EWPtool__ scanned the selected source tree, found changes, and updated entries in the project tree layout. Click on the `Yes` button to allow the IDE to reload the project.

>![dialog-project-reload](https://github.com/IARSystems/project-migration-tools/assets/54443595/a938a672-5176-4ced-8c0f-8e40fe494811)

* Reloading the project will recursively add the source files from the selected folder to the current build configuration ("Debug" in this example). The result can be verified by unfolding the groups, located in the project's __Workspace window__:

>![workspace-initial-project-layout-tree](https://github.com/IARSystems/project-migration-tools/assets/54443595/9e72d036-621c-4266-9b7e-c7eb3d81068d)

And this is what you need to know to start using the IAR __EWPtool__ Utility.

Proceed to the [IAR EWPtool wiki][url-repo-wiki] for more information.

## Issues
Found an issue or have a suggestion related to the __EWPtool__? Feel free to use the public issue tracker.
- Do not forget to take a look on [earlier issues][url-repo-issue-old].
- If creating a [new][url-repo-issue-new] issue, please describe it in detail.

## Summary
This short tutorial offered general guidelines with tips and shortcuts for developers willing to get a speedup factor when migrating a project, from scratch, to the _IAR Embedded Workbench_.

<!-- Links -->
[url-repo-issue]:     https://github.com/IARSystems/project-migration-tools/issues
[url-repo-issue-new]: https://github.com/IARSystems/project-migration-tools/issues/new
[url-repo-issue-old]: https://github.com/IARSystems/project-migration-tools/issues?q=is%3Aissue+is%3Aopen%7Cclosed

[url-repo-tool-release-rn]: https://github.com/IARSystems/project-migration-tools/releases/latest

[url-repo-wiki]: https://github.com/IARSystems/project-migration-tools/wiki

[url-iar-migration]: https://iar.com/products/project-migration-tools
[url-iar-doc-proj-dir]: https://wwwfiles.iar.com/arm/webic/doc/EWARM_IDEGuide.ENU.pdf#page=89
