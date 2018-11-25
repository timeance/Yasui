# yasui
Control entity resource consumption by drop AI computing. Mobは安い。

### Use Cases

Yasui is the plugin for servers that would like to keep minimum impact on vanilla gameplay.

By dropping AI computing and reduce server random tick speed on demand automatically, yasui enables much higher entity load on your server.

### Get Started

1. Download and install [NyaaCore](https://github.com/NyaaCat/NyaaCore/releases) corresponding to your server version.
2. Download and install [yasui](https://github.com/NyaaCat/Yasui/releases).
3. [Optional] Install [NyaaUtils](https://github.com/NyaaCat/NyaaUtils/releases) (for realtime TPS logging).
4. Remove all your lag removal plugins and restart server.
5. Enjoy benefit.

### Configuration

Configuration file `yasui/config.yml`

```
language: en_US
enable: true  # enable yasui auto nerfing
check_interval_tick: 60
tps_disable_ai: 17.0  # remove AI if server tps lower than this value for 60 ticks
tps_enable_ai: 19.0  # restore AI if server tps higher than this value for 60 ticks
world_entity: 2400  # allow disable AI when entity amount greater than this value
chunk_entity: 3  # remove AI for chunks containing more entities than this value
ignored_world:
- ignored_world  # this world will be ignored
__class__: cat.nyaa.yasui.Configuration  # DON'T TOUCH THIS
ignored_entity_type:    # those entities will not be removed AI                                                                                                                       
- SNOWMAN
- ARMOR_STAND
use_essentials_tps: true  # use Essentials for realtime TPS logging (will be replaced by NyaaUtils)
rules:
  1:    # Restore AI when 1 / 5 / 15 minutes tps value higher than 19.0
    __class__: cat.nyaa.yasui.Rule
    enable: false
    condition: tps_1m >= 19.0 && tps_5m >= 19.0 && tps_15m >= 19.0
    enable_ai: '1'
  2:    # Remove AI when 1 minute tps lower than 18.0
    __class__: cat.nyaa.yasui.Rule
    enable: false
    condition: tps_1m < 18.0
    disable_ai: '1'
  tickspeed1:    # lift worlds' random tick speed (maximum 3) when tps higher than 19.0 and notify players in actionbar
    __class__: cat.nyaa.yasui.Rule
    enable: false
    condition: tps_1m > 19.0 && tps_5m > 19.0 && world_random_tick_speed < 3
    worlds:
      - world
      - world_nether
      - world_the_end
    world_random_tick_speed: MIN(world_random_tick_speed + 1,3)
    messageType: ACTION_BAR
    message: '&arandom tick speed&r: &9{world_random_tick_speed}'
  tickspeed2:    # reduce worlds' random tick speed (minimum 0) when tps lower than 18.0 and notify players in actionbar
    __class__: cat.nyaa.yasui.Rule
    enable: false
    condition: tps_1m < 18.0 && tps_5m < 18.0 && world_random_tick_speed > 0
    worlds:
      - world
      - world_nether
      - world_the_end
    world_random_tick_speed: MAX(world_random_tick_speed - 1,0)
    messageType: ACTION_BAR
    message: '&arandom tick speed&r: &9{world_random_tick_speed}'
listen_mob_spawn: false    # do not catch new mob spawn
ignored_spawn_reason:
- CUSTOM    # spawned by plugins (all nature spawned mobs will be removed AI when in high load)
```


