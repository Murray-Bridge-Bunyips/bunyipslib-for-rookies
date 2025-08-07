# Continuous spinning intake

A continuous spinning intake is a continuous rotation servo (or DC motor) that simply rotates. This guide will focus specifically on a continuous rotation servo.

The `Actuator` subsystem is used to control a non-encoder actuator.

## Adding an intake to your config
To your `Hardware` static class in your configuration file, add a new `CRServo`:
```java
public static class Hardware {
    // ...
    public CRServo intake;
}
```

Then, add the declaration of a `Actuator` subsystem in your configuration public declaration area:
```java
public class Cubicle extends RobotConfig {
    public final Hardware hw = new Hardware();

    // ... SUBSYSTEMS AND OTHER PUBLIC DECLARATIONS HERE ...
    public Actuator intake;
    // ...
}
```

Finally, you need to initialise your hardware and subsystem. These are currently null.

In `onRuntime()`, add:
```java
@Override
protected void onRuntime() {
    // ...

    hw.intake = getHardware("intake", CRServo.class, (d) -> {
        // TODO: Set the direction of the intake motor here
        d.setDirection(DcMotorSimple.Direction.FORWARD);
    });

    intake = new Actuator(hw.intake)
            .withName("Intake");
}
```
and **ensure to change or confirm the name "intake" matches your hardware mapping** set in Configure Robot.
Your actuator may also be configured backwards after you test it, and you will need to change the enum from `FORWARD` to `REVERSE`.

The intake is now configured.

