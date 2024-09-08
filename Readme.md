# WorldGuard Events

## For developers
### Events
This API provides you with the following events :

   - **RegionsChangedEvent**
   - **RegionsEnteredEvent**
   - **RegionsLeftEvent**
   - **RegionEnteredEvent**
   - **RegionLeftEvent**

### API
On top of events, and since version 1.15.2 of this plugin, we now provide you with a small API to get the most basic informations out of WorldGuard :

   - **getRegions**
   - **getRegionNames**
   - **isPlayerInRegion**
   - **isPlayerInAnyRegion**
   - **isPlayerInAllRegions**

Example:
```java
import net.raidstone.wgevents.WorldGuardEvents;
import net.raidstone.wgevents.events.RegionEnteredEvent;
import net.raidstone.wgevents.events.RegionsLeftEvent;
import org.bukkit.Bukkit;
import org.bukkit.entity.Player;
import org.bukkit.event.EventHandler;
import org.bukkit.event.Listener;
import org.bukkit.plugin.java.JavaPlugin;
import java.util.Set;

public class TestPlugin extends JavaPlugin implements Listener {
    @Override
    public void onEnable() {
        Bukkit.getPluginManager().registerEvents(this, this);
     
        // On plugin activation, send a message to players that are located in a jail.
        for(Player p : Bukkit.getOnlinePlayers())
        {
            if(WorldGuardEvents.isPlayerInAnyRegion(p.getUniqueId(), "jail", "cell"))
            {
                p.sendMessage("You are in jail ! Did you do something bad ?");
            }
        }
    }
 
   @EventHandler
   public void onRegionEntered(RegionEnteredEvent event)
   {
       Player player = Bukkit.getPlayer(event.getUUID());
       if (player == null) return;
     
       String regionName = event.getRegionName();
       if(regionName.equalsIgnoreCase("jail"))
       {
           player.sendMessage("You are now in jail !");
       }
   }

   @EventHandler
   public void onRegionsLeft(RegionsLeftEvent event)
   {
       Player player = Bukkit.getPlayer(event.getUUID());
       if(player == null) return;
     
       Set<String> regionsNames = event.getRegionsNames();
     
       if(regionsNames.contains("jail") || regionsNames.contains("cell"))
       {
           player.sendMessage("You are in jail, you can't escape !");
           event.setCancelled(true);
       }
   }
}
 
```

## Maven dependency
This plugin is located in the Maven Central repository, so you simply need to add it as dependency.

```xml
<dependency>
    <groupId>net.raidstone</groupId>
    <artifactId>WorldGuardEvents</artifactId>
    <version>1.18.1</version>
    <scope>provided</scope>
</dependency>
```
    
## [Spigot page](https://www.spigotmc.org/resources/worldguard-events.65176/)

### If you like this plugin, don't forget to rate it, to help other people discover it !
