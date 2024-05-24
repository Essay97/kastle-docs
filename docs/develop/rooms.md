# Rooms

Defining rooms is the core of worldbuilding for a Kastle game. Keep in mind that a room in Kastle does not have to model a physical
room: think about it more like a generic "place".

Every place that the player could interact with, in Kastle is a room: a forest, a village, even a portion of a room could be a room!

Given the importance of this DSL, let's dive into it and learn how to make better rooms.

!!! info "Room ID"

    The ID of any room must always start with `r-` and contain only lowecase letters, numbers and dashes.

## Connecting a room to the others

The Room DSL provides 4 functions (one for each direction) to link rooms together: `north`, `south`, `west` and `east`. They all work the same way and define a "door" between the room and the other one in the specified direction. The door can work in different ways based on the following configurations:

- **state**: indicates if the door is open or locked. The values are provided by an enum with `LinkState.OPEN` and `LinkState.LOCKED`
- **behavior**: indicates if the door state can be changed. The `LinkBehavior` enum provides the following values:
    - `OPENABLE`
    - `LOCKABLE`
    - `COMPLETE`
    - `CONSTANT`
- **triggers**: the list of items that can be used as a key to change the state of the door, if its behavior permits it. The items
are specified by their id.

If these values are not specified, the default connection is an `OPEN`, `CONSTANT` door.

## Adding characters and items
As we already saw, it's possible to put characters and items into rooms. This is the main way to characterize a room. Follow the dedicated guides to know how to create these objects. 