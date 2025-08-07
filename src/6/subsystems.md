# Adding more subsystems

With a simple robot configuration file set up and drivebase configured, adding more devices to your configuration file is essential.

To understand more about the subsystem/hardware interaction, read [this page from the BunyipsLib Wiki](https://github.com/Murray-Bridge-Bunyips/BunyipsLib/wiki/Paradigms#robot-subsystems).

Looking at other examples is also a good way of learning how to use subsystems. Review the robot code from other robots within your project.

The set of available built-in subsystems to use can be found on [this page of the BunyipsLib Wiki](https://github.com/Murray-Bridge-Bunyips/BunyipsLib/wiki/Subsystems).

A table is presented below describing the uses of some hardware devices and corresponding subsystem.

| Subsystem                     | Hardware                              | Use case                                                                                    |
| ----------------------------- | ------------------------------------- | ------------------------------------------------------------------------------------------- |
| Drivebases (previous section) | Varying # `DcMotor`                   | Robot locomotion.                                                                           |
| `HoldableActuator`            | 1x `DcMotor`, varying # `TouchSensor` | Any encoder-enabled motor used for precise control. Integrates limit switches autonomously. |
| `Actuator`                    | 1x `CRServo`                          | Any encoder-disabled motor/servo used to rotate.                                            |
| `Switch`                      | 1x `Servo`                            | Standard subsystem to control a Servo. Not related to limit switches.                       |
| `DualServos`                  | 2x `Servo`                            | Runs two Servo instances together. Simplified version of `Switch`.                          |

> [!NOTE]
> This list is not extensive and more information is available on the wiki.

Pages within this subsection will demonstrate some simple purpose hardware and subsystem configurations commonly used within robots.
