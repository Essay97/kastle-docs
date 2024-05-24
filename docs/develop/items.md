# Items

In Kastle, items can be used in two main ways:

- to convey information
- to open/close doors

The basic items that we already defined are perfect to convey information: they have a name and a description and we can use
these two properties to let the player discover details about the game. As an example, think about an incision on a wall. In the description we can put what the incision says and maybe that's a hint about how to go to the next room.

In the following paragraphs, we'll learn how to make more complex items.

## Grabbing items

In order to use an item to open a door, we must make that item storable in the player inventory. We do that exactly with the `storable` property:

```kotlin hl_lines="4"
item("i-key") {
    name = "Key"
    description = "This item opens a door"
    storable = true
}
```

An item with this property set can be grabbed, carried around and used.

## Making items easier to find

Items are found by the player in the environment by using their name.
We can make the life of the player easier by allowing multiple names to reference a specific item.
In particular, an item will respond to its name and all the strings that are defined in the `matchers` property

```kotiln hl_lines="3"
item("i-sword") {
  name = "Sword"
  matchers("blade", "weapon")
}
```
