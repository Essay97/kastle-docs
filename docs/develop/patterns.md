# Game Patterns

The functionalities of Kastle are currently quite limited, but if used correctly, they can be very flexible. Below are some patterns that allow for interesting solutions.

## Chain of objects

Example: The player inspects a painting and discovers a stain. Then they inspect the stain and find that a bugging device is embedded in the clump of paint, etc.

The major downside of this is that if the player tries to inspect the bugging device (perhaps randomly or because a friend told them about it), Kastle will allow them to inspect it.

## “Fake” movements

Imagine having a chest in a room that can only be opened with a key. Kastle doesn't allow for opening and closing objects, but you can place the chest in a "room" that is accessed with a key. Narratively, the player will always be in the same place, only moving close to the chest, but this “place near the chest” is modeled as a room in Kastle.

## Story items

It's possible to use items as fictitious objects to advance the story. For example, in each room, we can define an item named "plot" that in its description contains the part of the story to be discovered in that room. You can use the room's description to instruct the player on how to use this feature, for instance, by inserting a phrase like "Type _inspect plot_ to proceed with the story."

## Multiple requirements

If you want to require the player to find more than one item (for example, all pieces of a puzzle) to proceed, you can set up a series of successive rooms, each requiring a specific item as the key for the next one. For instance, you are in the main room, and to enter the final room, you need to collect four puzzle pieces. The door to the next room will be locked: it opens using piece 1 as the key. Once you open and pass through the door, you are not in the final room yet, but in an intermediate room with another locked door, which opens with piece 2, and so on.
