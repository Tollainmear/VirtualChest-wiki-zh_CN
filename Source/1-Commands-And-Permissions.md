# Commands

The following three command names are equivalent:

* `/virtualchest`
* `/vchest`
* `/vc`


### `/vc v` or `/vc version`

Shows the version and other information of this plugin.

Sample output:

```
================================================================
VirtualChest v0.4.0
A sponge plugin providing virtual chest GUIs for menus.
================================================================
Released at 21 Oct 2017
Git commit hash: a155dbf
Website: https://ore.spongepowered.org/zzzz/VirtualChest
GitHub repository: https://github.com/ustc-zzzz/VirtualChest
================================================================
```

### `/vc r` or `/vc reload`

Reloads configurations, which contains the main configuration (`config/virtualchest/virtualchest.conf`) and the menu configurations (in `config/virtualchest/menu/`).

Sample output:

```
Start reloading for VirtualChest ...
Reloading complete for VirtualChest.
```

The executor should has `virtualchest.reload` permission.

### `/vc o <inventory>` or `/vc open <inventory>`

Opens a specific `<inventory>` to the player which has executed the command.

If the executor is not a player (the server console or a command block), an error message will be created.

If the player does not have the `virtualchest.open.self.<inventory>` permission, the player will not be able to open the menu.

If the corresponding inventory does not actually exist, or the player has neither `virtualchest.open.self.<inventory>` permission nor `virtualchest.open.others.<inventory>`, an error message saying that the inventory does not exist will be shown to the player.

The name of an `<inventory>` is determined by the name of configuration files.

### `/vc o <inventory> <player>` or `/vc open <inventory> <player>`

Opens a specific `<inventory>` to specified players.

This command could be executed by the server console, command blocks, etc.

If the executor is a player, the player should have the `virtualchest.open.others.<inventory>` permission for opening the `<inventory>` to others.

Selectors are able to be used in this command. For example, command `/vc o <inventory> @a` will show an `<inventory>` to all the online players.

If the corresponding inventory does not actually exist, or the player has neither `virtualchest.open.self.<inventory>` permission nor `virtualchest.open.others.<inventory>`, an error message saying that the inventory does not exist will be shown to the player.

The name of an `<inventory>` is determined by the name of configuration files.

### `/vc l` or `/vc list`

Lists all the available menus.

If the executor is a player, and the player does not has any permission to open a menu (self or others), the menu will not be shown in the list.

The executor should has `virtualchest.list` permission.

# Permissions

| Permission String                  | Description                              |
| :--------------------------------- | :--------------------------------------- |
| `virtualchest`                     | All the permissions related to VirtualChest |
| `virtualchest.list`                | Permission for listing all the menus     |
| `virtualchest.reload`              | Permission for reloading plugin          |
| `virtualchest.bypass`              | Permission for ignoring click frequency restriction |
| `virtualchest.open`                | Permission for opening any inventory to anyone |
| `virtualchest.open.others`         | Permission for opening any inventory to other players |
| `virtualchest.open.self`           | Permission for opening any inventory to the executor itself |
| `virtualchest.open.others.example` | Permission for opening an inventory named `example` to other players |
| `virtualchest.open.self.example`   | Permission for opening an inventory named `example` to the executor itself |

