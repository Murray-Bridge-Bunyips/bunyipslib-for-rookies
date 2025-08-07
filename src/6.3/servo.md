# Single servo

The single servo subsystem is used commonly in designs with rotators, clasps, cannons, and other "open or close" applications.

The `Switch` subsystem controls a single servo.

## Adding a servo subsystem to your config
To your `Hardware` static class in your configuration file, add a new `Servo`:

```java
public static class Hardware {
    // ...
    public Servo servo; // Add this line, and name the servo something more descriptive
}
```

Then, add the declaration of a `Switch` subsystem in your configuration public declaration area:

```java
public class Cubicle extends RobotConfig {
    public final Hardware hw = new Hardware();

    // ... SUBSYSTEMS AND OTHER PUBLIC DECLARATIONS HERE ...
    public Switch servo;
    // ...
}
```

Finally, you need to initialise your hardware and subsystem. These are currently null.

In `onRuntime()`, add:
```java
@Override
protected void onRuntime() {
    // ...

    hw.servo = getHardware("servo", Servo.class, (d) -> {
        // TODO: Set the direction of the servo here
        d.setDirection(Servo.Direction.FORWARD);
        // TODO: Scale your servo accordingly, or use methods attached to the Switch subsystem directly
        d.scaleRange(0, 1);
    });

    servo = new Switch(hw.servo)
            .withName("Servo");
}
```
and **ensure to change the name "servo" to your hardware mapping** set in Configure Robot.
Your servo may also close/open in the wrong state after you test it, and you will need to change the enum from `FORWARD` to `REVERSE`.

Your servo is now configured.

## Scaling your servo range and limiting speed
The servo programmer is a nightmare to use. Hence, it is more beneficial if you have a servo-programmed range that is much wider than requirements for software to scale
the range appropriately.

In the above snippet, the `scaleRange(0, 1)` method is exposed. Changing values here will define the new minimum and maximum range the servo can go to from 0 to 1.

It is recommended to an FtcDashboard constant to allow rapid tuning of this value. This is not covered in the scope of this guide and can be found on the [IO section of the BunyipsLib Wiki](https://github.com/Murray-Bridge-Bunyips/BunyipsLib/wiki/IO).

Limiting the speed of a servo requires the use of the BunyipsLib-provided `ServoEx` hardware device. It can be dropped in place of the `Servo` class, exposing more
methods to control speed and acceleration. More information can be found [here](https://github.com/Murray-Bridge-Bunyips/BunyipsLib/wiki/IO#servos).
