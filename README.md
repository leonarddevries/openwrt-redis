[//]: #@corifeus-header

 

[![Donate for Corifeus / P3X](https://img.shields.io/badge/Donate-Corifeus-003087.svg)](https://paypal.me/patrikx3) [![Contact Corifeus / P3X](https://img.shields.io/badge/Contact-P3X-ff9900.svg)](https://www.patrikx3.com/en/front/contact) [![Corifeus @ Facebook](https://img.shields.io/badge/Facebook-Corifeus-3b5998.svg)](https://www.facebook.com/corifeus.software)   [![Build Status](https://travis-ci.com/patrikx3/openwrt-redis.svg?branch=master)](https://travis-ci.com/patrikx3/openwrt-redis) [![Uptime Robot ratio (30 days)](https://img.shields.io/uptimerobot/ratio/m780749701-41bcade28c1ea8154eda7cca.svg)](https://uptimerobot.patrikx3.com/)

# 📡 OpenWrt Redis stable version


                    
 
                        
[//]: #@corifeus-header:end

## Deprecated  

It does work with Redis 5.0.3, bit given I have no use for this package, I deprecated it and archived.

## Info

Given, that ```/var/lib``` is usually in the ROM, so the ```REDIS``` data is in actually in ```/opt/var/lib/redis``` and ```/var/lib/redis``` is a symlink to ```/opt/var/lib/redis```. I think it is good if you use ```ext-root``` or an ```SD-CARD``` based backend storage or something like.

## Redis 5
In Redis 5, I have removed the jemalloc memory allocator to use the default built in libc memory allocator. Because of this, since Redis 5, the program is much smaller. 

## Redis 4
The last Redis 4 version is here:  
https://github.com/patrikx3/openwrt-redis/tree/v4.0.11 - tag  
https://github.com/patrikx3/openwrt-redis/tree/v4 - branch  
  
If someone needs it, it can be built as:
```bash
echo 'src-git redis https://github.com/patrikx3/openwrt-redis.git;v4' >> feeds.conf
```

## The feed

### Tested on Linksys WRT

http://cdn.corifeus.com/openwrt/18.06.2/packages/arm_cortex-a9_vfpv3/redis/

```text
src/gz reboot_redis http://cdn.corifeus.com/openwrt/18.06.2/packages/arm_cortex-a9_vfpv3/redis
```


## Built package:
  
* Like Linksys WRT ARM ```atomic instructions```
  * https://cdn.corifeus.com/openwrt/18.06.2/packages/arm_cortex-a9_vfpv3/redis/  


## The router service

Please, where you can find it in  [OPENWRT-INSOMNIA](https://pages.corifeus.com/openwrt-insomnia), of course it includes ```init.d``` service as well.

```bash
/etc/init.d/redis stop|start
```

## Your own build

```bash
cp feeds.conf.default feeds.conf
echo 'src-git redis https://github.com/patrikx3/openwrt-redis.git' >> feeds.conf
./scripts/feeds update -a
./scripts/feeds install -a
./scripts/feeds update redis
./scripts/feeds install -a -p  redis

# create a .config
make menuconfig

# either
make package/feeds/redis/redis/{clean,prepare,compile} package/index V=s

# or
make V=s
```


# PS

This is based on:
https://github.com/chrisber/openwrt-ipkg-redis and https://github.com/pdf/openwrt-14.07-x86_64-packages/tree/master/net/redis .

It will be in all of my [OPENWRT-INSOMNIA](https://pages.corifeus.com/openwrt-insomnia).

### CPU type
Right now, I only tested on ARM (Linksys WRT1900ACS, Linksys 3200ACM) since it is 5.0.4

# Built from

http://download.redis.io/releases/


[//]: #@corifeus-footer

---

[**P3X-OPENWRT-REDIS**](https://pages.corifeus.com/openwrt-redis) Build v2019.4.7 

[![Donate for Corifeus / P3X](https://img.shields.io/badge/Donate-Corifeus-003087.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=QZVM4V6HVZJW6)  [![Contact Corifeus / P3X](https://img.shields.io/badge/Contact-P3X-ff9900.svg)](https://www.patrikx3.com/en/front/contact) [![Like Corifeus @ Facebook](https://img.shields.io/badge/LIKE-Corifeus-3b5998.svg)](https://www.facebook.com/corifeus.software) 


## P3X Sponsors

[IntelliJ - The most intelligent Java IDE](https://www.jetbrains.com/?from=patrikx3)
  
[![JetBrains](https://cdn.corifeus.com/assets/svg/jetbrains-logo.svg)](https://www.jetbrains.com/?from=patrikx3) [![NoSQLBooster](https://cdn.corifeus.com/assets/png/nosqlbooster-70x70.png)](https://www.nosqlbooster.com/)

[The Smartest IDE for MongoDB](https://www.nosqlbooster.com)
  
  
 

[//]: #@corifeus-footer:end



