# Slot Configuration

As an example, here is a configuration that can sell wheats and deduct the corresponding amount of money:

```hocon
Slot0 {
  Item {
    Count = 1
    ItemType = "minecraft:wheat"
    UnsafeDamage = 0
    DisplayName = "&lWheat seller"
    ItemLore = [
      "&ePrice for %player_name%: $10 => 9 wheats"
      "&eYou can left or right click to buy some wheats"
    ]
  }
  PrimaryAction {
    Command = "cost: 10; console: give %player_name% minecraft:wheat 9"
    KeepInventoryOpen = true
  }
  SecondaryAction {
    Command = "cost: 10; console: give %player_name% minecraft:wheat 9"
    KeepInventoryOpen = true
  }
  Requirements = "%economy_balance% >= 10"
}
```

The configuration for slots contains 5 options: `Item`, `Requirements`, `PrimaryAction`, `SecondaryAction`, and `IgnoredPermissions` (not shown above).

## `Item`

The value of this option should describe the item in the slot.

PlaceholderAPI, if available, will be applied to the configuration, and different items with different names, lores, etc. will be shown to different players.

For example: `Hello %player_name%` will be translated to `Hello Notch` if the name of the player is Notch.

An item has several attributes, and several frequently used attributes are listed below.

### The `Count` attribute

The count of items in the slot, needed to be explicitly declared in the configuration. In most cases it is 1.

### The `ItemType` attribute

The type of the item in the slot, needed to be explicitly declared in the configuration.

### The `UnsafeDamage` attribute

The item meta of the item in the slot, needed to be explicitly declared in the configuration. Usually it is 0.

### The `UnsafeData` attribute

The NBT of the item. For example, the section shown below represents a skull head which represents the player who opens the inventory:

```hocon
Item {
  Count = 1
  ItemType = "minecraft:skull"
  UnsafeDamage = 3
  UnsafeData = {
    SkullOwner = "%player_name%"
  }
}
```

### The `DisplayName` attribute

The custom name of the item. Formatting codes are available.

### The `ItemLore` attribute

The list of custom tips of the item (usually called item lore). Formatting codes are available.

### The `ItemEnchantments` attribute

The enchantment list of the item. For example:

```hocon
ItemEnchantments = [
  "minecraft:protection:4"
  "minecraft:fire_protection:4"
  "minecraft:feather_falling:4"
  "minecraft:blast_protection:4"
  "minecraft:projectile_protection:4"
]
```

It has all the protection enchantments, and all the levels are 4 (full level).

### The `HideEnchantments` attribute

Whether the item shows its enchantments or not, could be either `true` or `false`.

### The `HideAttributes` attribute

Whether the item (usually tools) shows its attributes or not, could be either `true` or `false`.

## `Requirements`

The value of this option should describe the requirement of the player. If the player does not match the requirements, the corresponding item will not be shown. If a list is provided to describe a slot, it will try the next item. If none of them matches, the slot will be empty.

For example, if it is hoped that a wheat item should be shown to the player if the player has enough money (not less than 10), otherwise a barrier should be shown, the configuration could be like this:

```hocon
Slot0 = [{
  Item {
    Count = 1
    ItemType = "minecraft:wheat"
    UnsafeDamage = 0
  }
  Requirements = "%economy_balance% >= 10"
}, {
  Item {
    Count = 1
    ItemType = "minecraft:barrier"
    UnsafeDamage = 0
  }
}]
```

If the player has enough money, the first item will be shown, otherwise the second one does.

### JavaScript

The requirement string is JavaScript, which means that it will be compiled or interpreted to decide if the player matches the requirements. Any of the following return values of the script will be considered that the player matches the requirements:

* The boolean `true`
* The case insensitive string `true`, `True`, `TRUE`, etc.

In other cases, it will be regarded as `false`, which means that the player does not match the requirement.

If PlaceholderAPI String (`%xxxx_xxxx_xxxx%`) is appeared in the script string, it will be translated to a function call (`papi('xxxx_xxxx_xxxx')`), then calculated to get the result value.

If you has ever used some other menu plugins which uses JavaScript such as DeluxeMenus, the difference between them and VirtualChest needs to be noticed. For example, if you want to check if the player is `Notch` when configuring DeluxeMenus, you may write `'%player_name%' == 'Notch'`, and it will be translated to `'Notch' == 'Notch'`, `'Dinnerbone' == 'Notch'`, or something else. However, if you are using VirtualChest, it will be translated to `'papi('player_name')' == 'Notch'`, which will produce a syntax error.

In VirtualChest, you should write `%player_name% == 'Notch'`, and the string with placeholders will be translated to `papi('player_name') == 'Notch'`, then calculated to `'Notch' == 'Notch'`, `'Dinnerbone' == 'Notch'`, etc. which have the correct JavaScript syntax.

Let's go back and talk about the requirement given at the beginning, `%economy_balance% >= 10`. If the player has 20, if will be equivalent to `'20' > 10`, and then JavaScript provides an implicit conversion from `Number` to `String`, so `'20' > 10` is equivalent to `20 > 10`, which is `true`.

There are some preset variables for convenience:

* `server`, the Sponge Server object
* `player`, the player object
* `papi`, the function translating placeholders to its actual values
* `tick`, ticks from opening the inventory, which can be used to create animations

### Frequently used requirement string patterns

* `%player_name% == 'Notch'`

  checks if the player is Notch

* `%economy_balance% >= 10`

  checks if the player has got 10 (default currency)

* `player.hasPermission('virtualchest.open.self')` or `%player_perm_virtualchest.open.self%`

  checks if the player has `virtualchest.open.self` permission, the first one is recommended

* `%server_online% > 20` or `server.getOnlinePlayers().size() > 20`

  checks if the count of online players is greater than 20, the first one is recommended

Of course, you can use `&&` (and) or `||` (or) to combine scripts together. For example, `%player_name% == 'Notch' && %economy_balance% >= 10` checks if the player is Notch and the player has got 10 (default currency).

## `PrimaryAction` and `SecondaryAction`

The value of the two options determines what will happen when a player clicks an item. `PrimaryAction` means left clicking, and `SecondaryAction` means right clicking. For example:

```hocon
PrimaryAction {
  Command = "cost: 10; console: give %player_name% minecraft:wheat 9"
  KeepInventoryOpen = true
}
```

The `KeepInventoryOpen` option determines if the inventory will be closed immediately after the target player clicks the item. The default value is `false`.

The `Command` option defines several actions which will be done orderly. The actions are separated by semicolons (`;`) and in the example shown above, there are two actions: `cost: 10`, and `console: give %player_name% minecraft:wheat 9`. The `Command` option supports PlaceholderAPI, and the `%player_name%` will be replaced by the name of the player who clicks the item.

An action in the `Command` option can have a prefix, and different prefixes represents different action types. The prefix and the main part of the action are separated by a colon (`:`). Please refer to [Available Action Prefixes](#available-action-prefixes).

There is another option which is not in the example shown above: `HandheldItem`. `HandheldItem` specifies the type and quantity of items which should be held in the mouse cursor. For example:

```hocon
PrimaryAction {
  Command = "cost-item: 9; cost: -8"
  HandheldItem {
    ItemType = "minecraft:wheat"
    Count = 9
    UnsafeDamage = 0
  }
  KeepInventoryOpen = true
}
```

It rules that the player must hold at least 9 wheats to click the item in order to trigger actions. The `UnsafeDamage` option is not required, and if it is absent, it means that any meta matches. This feature is often used to make chest shop menus, in which a player can hold something for selling them.

## `IgnoredPermissions`

VirtualChest does not provide the `op` action which is available in ChestCommands and BossShop. The reason is that Sponge ignored the concept of OP deliberately because of safety and some other reasons (please refer to this [pull request](https://github.com/SpongePowered/SpongeAPI/pull/285)). Instead, VirtualChest provides a feature called `IgnoredPermissions`, which is safer, and more flexible.

You can define a list of ignored permissions like this:

```hocon
Slot0 {
  Item {
    // blablabla
  }
  PrimaryAction {
    // blablabla
  }
  SecondaryAction {
    // blablabla
  }
  IgnoredPermissions = [
    "plugin1.permission1"
    "plugin2.permission2"
  ]
}
```

All the provided permissions will be set to `true` when actions begins to be executed, and they will be set to their original value as soon as all the actions are executed. Thanks to the Sponge's Permission Context system I am able to implement the temporary permission system which is safe enough.

# Available Action Prefixes

### Empty prefix

An empty prefix means that the action is a command which will be executed by the target player directly.

### The `console` prefix

`console ` means that the main part of the action will be executed by the server console.

### The `tell` prefix

`tell` means that the main part will be sent to the target player. Formatting codes are available.

### The `tellraw` prefix

`tellraw` means that the main part will be treated as a [raw json message](https://minecraft.gamepedia.com/Commands/tellraw) and be sent to the player.

### The `broadcast` prefix

`broadcast` means that the main part will be broadcasted to all the online players. Formatting codes are available.

### The `title` prefix

`title` means that the main part will be sent to the target player's action bar. It does not represents the title shown in the center of the game interface. The existence of the name, `title`, is just a historical legacy. If you want to use the one shown in the center of the game interface, please use `bigtitle` and `subtitle` instead.

### The `bigtitie` prefix

`bigtitle` means that the main part will be shown in the center of the target player's client GUI. Besides, the subtitle can be set by the `subtitle` prefix. Formatting codes are available.

### The `subtitle` prefix

`subtitle` means that the main part will be shown as a subtitle slightly below the center of the target player's client GUI. Formatting codes are available.

### The `delay` prefix

`delay` means that all the actions after this one will be delayed for what is specified in the main part, the unit of which is tick (usually 0.05s). For example, `delay: 20` means that all the following actions will be delayed for one second. Please do not specify an oversized value, because the subsequent actions may not be executed if the target player is teleported to another world out or the server closed.

### The `connect` prefix

`connect` means that the target player will be teleported to another server whose name is specified in the main part. This prefix does not work normally unless [BungeeCord](https://www.spigotmc.org/wiki/bungeecord/) is available.

### The `cost` prefix

`cost` means that the target player will be tried to deduct a certain amount of money. The amount is specified by the main part of the action. Subsequent actions will be still executed while the player does not have enough money. Therefore, please check if the player is wealthy enough by using `Requirements`.

### The `cost-item` prefix

`cost-item` means that the target will be tried to remove a certain number of items held by the mouse cursor. The amount is specified by the main part of the action. Subsequent actions will be still executed while the player does not have enough items held by the mouse cursor. Please check if the player does hold something you want by using `HandheldItem`.

### The `sound` prefix

`sound` means that a sound event will be fired. The sound name (such as `minecraft:block.chest.open `, `minecraft:block.chest.close`, etc.) and sound volume (between 0.0 and 2.0 in most cases) can be speficied by the main part of the action which are seperated by a colon(`:`). For example, `sound: minecraft:block.chest.open:0.5`, `sound: minecraft:block.chest.close:2.0`, etc. The sound volume is optional and the default value is 1 (`sound: minecraft:block.chest.open` is equivalent to `sound: minecraft:block.chest.open:1.0`).

### The `sound-with-pitch` prefix

`sound-with-pitch` means that a sound event with particular sound pitch will be fired. It can even play a song with note block sounds and `delay`s. The sound name, sound volume, and the sound pitch can be provided by the main part of the action orderly which are seperated by a colon(`:`). For example, `sound-with-pitch: minecraft:block.note.harp:0.5:1.5`, `sound-with-pitch: minecraft:block.note.harp:2.0:3.0`, etc. The sound volume is optional and the default value is 1 (`sound-with-pitch: minecraft:block.note.harp:1.5` is equivalent to `sound-with-pitch: minecraft:block.note.harp:1.0:1.5`).

