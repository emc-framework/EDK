The EMC (Easy Minecraft Client) Development Kit
===================

Development kit for [EMC](https://gitlab.com/EMC-Framework/EMC)

You can find API documentation at the [EMC website](https://emc-framework.gitlab.io/)

Setting up your IDE to develop with EMC
-------------------

Make a new gradle project and match your build.gradle file with [the example one](https://gitlab.com/EMC-Framework/EDK/blob/master/build.gradle)

Making your first mod with EMC
-------------------

1. Create a file called `client.json` in the resource folder of your project, this file is used so that EMC can find your main class and info about your mod.
2. In that file add the following:

```
{

    "name": "Example mod",
    "website": "https://gitlab.com/EMC-Framework/EDK",
    "author": "Deftware",
    "minversion": 13.8,
    "version": 1,
    "main": "me.deftware.mod.Main.Main",
    "updateLinkOverride": false

}
```

3. Create the main class you specified in the `client.json` file.
4. Extend your class by `EMCMod`, add the required methods.
5. Here's an example of a main class:

```
package me.deftware.mod;

import me.deftware.client.framework.event.*;
import me.deftware.client.framework.command.*;
import me.deftware.client.framework.wrappers.IChat;
import me.deftware.client.framework.main.EMCMod;

public class Main extends EMCMod {
	
	@Override
	public void initialize() {
		// Here you initialize anything needed for the mod
		// We need to register this class to handle events 
		EventBus.registerClass(this.getClass(), this);
		// We can also register a custom client command here
		CommandRegister.registerCommand(new ExampleCommand());
	}

	@EventListener(eventType = EventUpdate.class) 
	public void onUpdate(EventUpdate event) {
		// Example of an update event, called 20 times a second
	}

	public class ExampleCommand extends EMCModCommand {

		@Override
		public CommandBuilder getCommandBuilder() {
			return new CommandBuilder().set((LiteralArgumentBuilder) LiteralArgumentBuilder.literal("test")
					.executes(c -> {
						IChat.sendClientMessage("It works!);
						return 1;
					})
			);
		}

	}

}
```

If you want more help you can check out this [example Discord rich presence mod](https://gitlab.com/EMC-Framework/EMC-Discord-RPC) made with EMC.

Installing your mod
-------------------

Once you're done with your mod, run the gradle `build` task. Copy your mod jar file from `build/libs/` to `.minecraft/libraries/EMC/<Minecraft version>/` and start Minecraft with EMC.

License
-------------------

EMC is licensed under GPL-3.0
