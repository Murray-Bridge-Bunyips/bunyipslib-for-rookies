# Double servo

The double servo is a simplified version of combining two `Switch` instances, used exclusively for claws that use two servos.

The `DualServos` subsystem controls a single servo.

## Adding a servo subsystem to your config
To your `Hardware` static class in your configuration file, add two new `Servo` instances:

```java
public static class Hardware {
    // ...
    public Servo leftServo; // Add these lines, and name the servo something more descriptive
    public Servo rightServo;
}
```

Then, add the declaration of a `DualServos` subsystem in your configuration public declaration area:

```java
public class Cubicle extends RobotConfig {
    public final Hardware hw = new Hardware();

    // ... SUBSYSTEMS AND OTHER PUBLIC DECLARATIONS HERE ...
    public DualServos claw;
    // ...
}
```

Finally, you need to initialise your hardware and subsystem. These are currently null.

In `onRuntime()`, add:
```java
@Override
protected void onRuntime() {
    // ...

    hw.leftServo = getHardware("leftServo", Servo.class, (d) -> {
        // TODO: Set the direction of the servo here
        d.setDirection(Servo.Direction.FORWARD);
        // TODO: Scale your servo accordingly or as additional arguments to the DualServos constructor
        d.scaleRange(0, 1);
    });
    hw.rightServo = getHardware("rightServo", Servo.class, (d) -> {
        // TODO: Set the direction of the servo here
        d.setDirection(Servo.Direction.FORWARD);
        // TODO: Scale your servo accordingly or as additional arguments to the DualServos constructor
        d.scaleRange(0, 1);
    });

    claw = new DualServos(hw.leftServo, hw.rightServo)
            .withName("Claw");
}
```
and **ensure to change the names "leftServo" and "rightServo" to your hardware mapping** set in Configure Robot.
Your servos may also close/open in the wrong state after you test it, and you will need to change the enum from `FORWARD` to `REVERSE`.

Your claw is now configured.
