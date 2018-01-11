# VirtualChest Wiki

Welcome to the VirtualChest wiki. This wiki contains some instructions and examples about how to use a plugin called VirtualChest.

VirtualChest is a [Sponge](https://www.spongepowered.org/) Plugin, which make it possible to create a menu with chest GUI. There are several Bukkit plugins having similar features, such as ChestCommands, BossShop, and DeluxeMenus.

The whole wiki is based on version 0.4.x of VirtualChest.

## Dependencies

VirtualChest v0.4.x is designed to work normally on the newest version of SpongeForge/SpongeVanilla 1.10.2, 1.11.2, and 1.12.x. In other words, it should work normally under SpongeAPI v5.1.0, v6.0.0, and v7.0.0.

It's recommended to install a [PlaceholderAPI](https://ore.spongepowered.org/rojo8399/PlaceholderAPI) plugin on your server in order to show different instructions to different player. VirtualChest v0.4.x supports both PlaceholderAPI v3.x and v4.x.

## Download and install the plugin

The release of VirtualChest plugin could be found both at [Github](https://github.com/ustc-zzzz/VirtualChest/releases) and [Ore](https://ore.spongepowered.org/zzzz/VirtualChest).

In order to install the plugin, you just need to put it into the `mods/` directory, as you did with other Sponge plugins.

## File Structure

A new directory, `config/virtualchest`, will be created after you installed the plugin and start the Sponge server.

The main configuration is located at `config/virtualchest/virtualchest.conf`. All the menu configuration files are located in `config/virtualchest/menu/` directory.

VirtualChest uses [HOCON](https://github.com/typesafehub/config/blob/master/HOCON.md) file format to store and read configurations, which is the most commonly used in Sponge plugins.

The name of the files in `config/virtualchest/menu/` directory determine the name of the corresponding menu. For example, two example confguration files will be created when the plugin is loaded on the server for the first time. They are `config/virtualchest/menu/example.conf`, which represents a menu named `example`, and `config/virtualchest/menu/example2.conf`, which represents a menu named `example2`.

## Reporting bugs or suggesting new features

There are two ways recommended for reporting bugs and suggesting new features:

* Replying on the [Sponge Forum](https://forums.spongepowered.org/t/virtualchest/17917)
* Submitting an [Issue](https://github.com/ustc-zzzz/VirtualChest/issues) or a [Pull Request](https://github.com/ustc-zzzz/VirtualChest/pulls) on GitHub

Please provide as much information as possible to help me find out what went wrong if you want to report bugs. It is highly recommended to provide the following information:

* The version of VirtualChest
* The version of SpongeForge/SpongeVanilla and Minecraft Forge/Vanilla Minecraft
* The version of PlaceholderAPI if it is available
* The `fml-server-latest.log` or `latest.log` (usually stored in pastebin)

## License

The source code of VirtualChest is under [LGPLv3](https://github.com/ustc-zzzz/VirtualChest/blob/master/LICENSE) license.

[![CC-BY-SA 4.0](https://i.creativecommons.org/l/by-sa/4.0/88x31.png)](http://creativecommons.org/licenses/by-sa/4.0/)
The whole VirtualChest wiki is under [CC-BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/) license.
