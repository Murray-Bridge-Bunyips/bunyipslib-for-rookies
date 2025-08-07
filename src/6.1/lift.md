# Vertical lift

The vertical lift is a very commonly used subsystem.

Since a vertical lift will rely on an encoder-driven motor, the `HoldableActuator` subsystem will be used.

> [!TIP]
> Since `HoldableActuator` is designed to be as generic as possible, `HoldableActuator` can be used for any type of actuator that has a motor! (e.g. rotators, winches, double-jointed arm with one `HoldableActuator` per arm) 
>
> A vertical lift will be used as the example here.

## Adding a vertical lift to your config

To your `Hardware` static class in your configuration file, add a new `DcMotor`:

```java
public static class Hardware {
    // ...
    public DcMotor lift; // Add this line
}
```

Then, add the declaration of a `HoldableActuator` subsystem in your configuration public declaration area:

```java
public class Cubicle extends RobotConfig {
    public final Hardware hw = new Hardware();

    // ... SUBSYSTEMS AND OTHER PUBLIC DECLARATIONS HERE ...
    public HoldableActuator lift;
    // ...
}
```

> [!IMPORTANT]
> By default, the vertical lift will be running the default PID controller.
>
> Configuring PID controllers and the BunyipsLib-provided drop-in `Motor` class are out of scope of this guide, and
> are better covered in the [IO section of the BunyipsLib Wiki](https://github.com/Murray-Bridge-Bunyips/BunyipsLib/wiki/IO).

Finally, you need to initialise your hardware and subsystem. These are currently null.

In `onRuntime()`, add:
```java
@Override
protected void onRuntime() {
    // ...

    hw.lift = getHardware("lift", DcMotor.class, (d) -> {
        // TODO: Set the direction of the lift motor here
        d.setDirection(DcMotorSimple.Direction.FORWARD);
    });

    lift = new HoldableActuator(hw.lift)
            // TODO: Configurations from the below section will go here!
            .withName("Lift");
}
```
and **ensure to change or confirm the name "lift" matches your hardware mapping** set in Configure Robot.
Your motor may also be configured backwards after you test it, and you will need to change the enum from `FORWARD` to `REVERSE`.

## Configuring safety
By default, `HoldableActuator` has limited safety features as it does not know the dynamics of your system. Several `with` methods are affixed to the `HoldableActuator`
to configure various features to improve the operation of your lift.

### Limit switches
Limit switches are hardware devices defined by the `TouchSensor` class. To configure one, add to your `Hardware` static class declarations:
```java
public static class Hardware {
    // ...
    public TouchSensor limitSwitch; // Add this line
}
```
and ensure to initialise it in `onRuntime()`:
```java
@Override
protected void onRuntime() {
    // ...

    // TODO: Ensure name matches hardware map!
    hw.limitSwitch = getHardware("limit_switch", TouchSensor.class);
}
```

To use this limit switch on your `HoldableActuator`, simply pass it as an argument to `withBottomSwitch()` or `withTopSwitch()`:
```java
// ...
lift = new HoldableActuator(hw.lift)

        // Configurations get added here on your already existing lift assignment
        .withBottomSwitch(hw.limitSwitch) // For a bottoming switch (most common)

        .withName("Lift");
```
### Stall detection
Motors are a fire hazard when they are running for too long against a hard stop. This is called a "stall". `HoldableActuator` attempts to stop stall events by
autonomously checking if the current draw of the motor exceeds a set threshold for an extended period.

You may need to configure the stall current of your motor. While the default may work, manual configuration is strongly recommended doing.

To configure the stall current, you will need to find your stall current. This can be done with the integrated `Hardware Tester` OpMode at the bottom of the TeleOp
list and observing the Current draw in Amps as the arm is stalled.

> [!NOTE]
> Too small of a threshold will cause overreactions, whereas too high of a threshold will cause underreactions.

Once you have figured out the time interval and stall current, you can set them:
```java
lift = new HoldableActuator(hw.lift)

        // Configurations get added here on your already existing lift assignment
        .withOvercurrent(Amps.of(8), Seconds.of(4)) // Use the values you desire

        .withName("Lift");
```

`HoldableActuator` also has a timeout for steady state, to preemptively prevent stall events if the motor does not reach a setpoint in time.
**This timeout is not set by default.** You can set a maximum steady state time using:
```java
lift = new HoldableActuator(hw.lift)

        // Configurations get added here on your already existing lift assignment
        .withMaxSteadyStateTime(Seconds.of(7)) // Good default

        .withName("Lift");
```

### Velocity profiling
`HoldableActuator` provides a basic motion profiler for users who do not use the more advanced `SystemController` features of a full `Motor` instance.

This motion profiler is able to determine a *maximum speed* at which the motor will attempt to reach, rather than using raw power controls. Using this mode
can also improve system dynamics against gravity.

To define a maximum speed in ticks per second, use the following:
```java
lift = new HoldableActuator(hw.lift)

        // Configurations get added here on your already existing lift assignment
        .withUserSetpointControl((dtSec) -> 200 * dtSec) // 200 ticks per second, change as desired

        .withName("Lift");
```
___

The options presented are not extensive. A fully configured `HoldableActuator` may look like the following:
```java
lift = new HoldableActuator(hw.lift)
        .withBottomSwitch(hw.limitSwitch) // homing switch
        .withTopSwitch(hw.limitSwitchTop) // bounds detection
        .withOvercurrent(Amps.of(8), Seconds.of(4)) // stall detection
        .withMaxSteadyStateTime(Seconds.of(7)) // preemptive stall detection
        .withUserSetpointControl((dtSec) -> 200 * dtSec) // motion profiling
        .withHomingPower(0.8) // homing process power
        .withHomingTimeout(Seconds.of(2)) // homing process max duration
        .withHomingZeroVelocityDuration(Milliseconds.of(500)) // homing check interval
        .withAutoPower(0.4) // max automatic movement power
        .withLowerLimit(-1000) // lower encoder limit
        .withUpperLimit(1000) // upper encoder limit
        .withLowerPowerClamp(-0.7) // lower motor speed limit
        .withUpperPowerClamp(0.9) // upper motor speed limit
        .withName("Lift");
```
Reading the API documentation for `HoldableActuator` can assist in determining what these methods control.