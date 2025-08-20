# Gradle and Git extras

## Gradle AGP upgrade
tldr; do not press this button to upgrade the AGP if it shows up.

![](./agp.png)

More information can be found at [this Cookbook article](https://cookbook.dairy.foundation/gradle/dont_upgrade/dont_upgrade.html).

## "Dubious ownership" when trying to use Git
You may have some problems if Git reports dubious ownership over the BunyipsLib/BunyipsFTC project.

The error may have text like:

![](./error.png)

This can happen if the BunyipsFTC project is on another drive.
If you're on a club laptop, open `cmd` (Command Prompt) in Windows, and run:
```
git config --global --add safe.directory "*"
```
*note: this command should only be run on club laptops

## Java JDK target not 17

Sometimes, building may fail since the Gradle JDK version is too recent.

If you get compilation issues from Java version, change your JDK version to 17.

To do this, go to Android Studio settings, then `Build, Execution, Deployment > Build Tools > Gradle`, then change
the Gradle JDK field to one that is version 17. I recommend clicking Download JDK and downloading `BellSoft Liberica 17`.

After this perform a Gradle sync and the code should build.

> [!TIP]
> Ctrl+Shift+O will Gradle sync. Double tapping Right Shift, then searching for "Compile All" and running `Compile All Sources` will compile everything locally.

## Gradle doesn't build while connected to the robot wirelessly (after update or first time)

You may encounter build problems when updating BunyipsLib, or building for the first time, as Gradle fails to grab dependencies if wirelessly connected to the robot.

> [!NOTE]
> It is advised to Git Pull every time you open the project again. If BunyipsLib has updated, run a Gradle sync and Compile All.

The problem can be resolved by building the source code locally **and with access to internet**. It will not work if trying
to build while connected to the robot's Wi-Fi if you don't have these dependencies locally.

To compile all sources, double tap Right Shift, then search for "Compile All" and run `Compile All Sources` while connected to a proper Wi-Fi network. You can then connect back to the robot to build to it as normal and for subsequent builds. 
