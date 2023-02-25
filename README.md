# command_cond
A simple mpv script that help you run different commands under different conditions.

This script can be used to enhance the `input.conf`, and can also be used with [InputEvent](https://github.com/natural-harmonia-gropius/input-event).

Inspired by [InputEvent](https://github.com/natural-harmonia-gropius/input-event), and using a lot of its code.

## Installation
1. Put [command_cond.lua](command_cond.lua) into `~~/scripts`
2. **OPTIONAL**: Set the configuration files location in `~~/script-opts/command_cond.conf` file
    ```
    configs=~~/input.conf
    ```
3. Configure the commands to be run by the script, synax is just like `input.conf` but note the run condition in the comment.See [configuration](#Configuration) for more detail.

## Options
```
configs=~~/input.conf,~~/script-opts/xxxx.conf
```
The setting in `~~/script-opts/command_cond.conf` is used to specify where the script reads conditions and commands from.

Default value is `~~/input.conf`, but it is recommended using a separate file for the configuration.

## Configuration
### synax
```
<event_name> <command> #|[cond]
```
The `event_name` is the name of an event, it is a word without spaces and you can name it whatever you want.
> Note: `event_name` is case-sensitive

The `cond` is a conditional statement with the same syntax as the profile-cond in [Conditional auto profiles](https://mpv.io/manual/master/#conditional-auto-profiles) in mpv.

The `command` will be run only when the conditions are met.If `cond` is blank, it means the command will be executed unconditionally
### Priority
When an event is triggered, the script runs the first command in that event that satisfies the condition, checking whether the condition is satisfied in the order from top to bottom in the configuration file.

## Commands
This command will trigger the an event with the name `event_name`:

`script-message-to command_cond <event_name>`

## Example
### Jump to the next file at the end of the file
In the default setting of mpv, hitting the right arrow key `→` will seek forward 5 seconds.When at the end of the file, I want to hit `→` to play the next file.

1. in `command_cond` configuration file:
    ```
    next playlist-next #|pause and time_remaining < 10
    next seek 5        #|
    ```
1. in `input.conf`:
    ```
    RIGHT script-message-to command_cond next
    ```

When pressing the right arrow key, the script will first check if the conditions of the first command are met. The condition for the first command is that the player is paused and the remaining playback time of the file is less than 10 seconds.

When at the end of the file and paused, the condition of the first command is satisfied, so the first command `playlist-next` is run, and mpv jump to next file.

When not in the above state, the condition of the first command is not met and the script checks the condition of the next command. The condition of the second command is blank, then it is run unconditionally, so the second command `seek 5` is executed.

## Acknowledgements
[InputEvent](https://github.com/natural-harmonia-gropius/input-event) ([MIT License](https://github.com/natural-harmonia-gropius/input-event/blob/master/LICENSE))