[//]: #@corifeus-header

# 📡 OpenWrt Redis stable version

                        
[//]: #@corifeus-header:end
# solution 

actually i just removed the jemalloc and it works out of the box.

# git

`git config --global credential.helper cache`

# feeds
```text
src-git packages https://git.openwrt.org/feed/packages.git^35e0b737ab496f5b51e80079b0d8c9b442e223f5
src-git luci https://git.openwrt.org/project/luci.git^f64b1523447547032d5280fb0bcdde570f2ca913
src-git routing https://git.openwrt.org/feed/routing.git^1b9d1c419f0ecefda51922a7845ab2183d6acd76
src-git telephony https://git.openwrt.org/feed/telephony.git^b9d7b321d15a44c5abb9e5d43a4ec78abfd9031b
src-git redis https://git.patrikx3.com/openwrt-redis.git;redis5
```

# build
````bash
git config --global credential.helper cache
./scripts/feeds update redis
# see the section below to edit

make package/feeds/redis/redis/{clean,prepare,compile} package/index V=s
make package/feeds/redis/redis/{prepare,compile} package/index V=s
````

# edit here
````text
make package/feeds/redis/redis/{clean,prepare} V=s QUILT=1

cd build_dir/target-arm_cortex-a9+vfpv3_musl_eabi/redis-5.0.0/

quilt series
quilt refresh
quilt push 010-redis.patch
quilt edit ./deps/jemalloc/src/pages.c 
quilt edit src/Makefile 
quilt edit src/atomicvar.h
quilt edit deps/jemalloc/src/background_thread.c 
quilt diff
quilt refresh
````

# undef

I could not undef, but for now, I just uncommented the code.

```text
JEMALLOC_HAVE_SCHED_SETAFFINITY  - 85
JEMALLOC_HAVE_PTHREAD_SETNAME_NP -511
```

```text
Index: redis-5.0.0/deps/jemalloc/src/background_thread.c
===================================================================
--- redis-5.0.0.orig/deps/jemalloc/src/background_thread.c
+++ redis-5.0.0/deps/jemalloc/src/background_thread.c
@@ -82,16 +82,7 @@ background_thread_info_init(tsdn_t *tsdn

 static inline bool
 set_current_thread_affinity(UNUSED int cpu) {
-#if defined(JEMALLOC_HAVE_SCHED_SETAFFINITY)
-       cpu_set_t cpuset;
-       CPU_ZERO(&cpuset);
-       CPU_SET(cpu, &cpuset);
-       int ret = sched_setaffinity(0, sizeof(cpu_set_t), &cpuset);
-
-       return (ret != 0);
-#else
        return false;
-#endif
 }

 /* Threshold for determining when to wake up the background thread. */
@@ -508,9 +499,6 @@ static void *
 background_thread_entry(void *ind_arg) {
        unsigned thread_ind = (unsigned)(uintptr_t)ind_arg;
        assert(thread_ind < max_background_threads);
-#ifdef JEMALLOC_HAVE_PTHREAD_SETNAME_NP
-       pthread_setname_np(pthread_self(), "jemalloc_bg_thd");
-#endif
        if (opt_percpu_arena != percpu_arena_disabled) {
                set_current_thread_affinity((int)thread_ind);
        }
```
[//]: #@corifeus-footer

---

[**P3X-OPENWRT-REDIS**](https://pages.corifeus.com/openwrt-redis) Build v2019.4.7 

[![Donate for Corifeus / P3X](https://img.shields.io/badge/Donate-Corifeus-003087.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=QZVM4V6HVZJW6)  [![Contact Corifeus / P3X](https://img.shields.io/badge/Contact-P3X-ff9900.svg)](https://www.patrikx3.com/en/front/contact) [![Like Corifeus @ Facebook](https://img.shields.io/badge/LIKE-Corifeus-3b5998.svg)](https://www.facebook.com/corifeus.software) 


## P3X Sponsors

[IntelliJ - The most intelligent Java IDE](https://www.jetbrains.com/?from=patrikx3)
  
[![JetBrains](https://cdn.corifeus.com/assets/svg/jetbrains-logo.svg)](https://www.jetbrains.com/?from=patrikx3) [![NoSQLBooster](https://cdn.corifeus.com/assets/png/nosqlbooster-70x70.png)](https://www.nosqlbooster.com/)

[The Smartest IDE for MongoDB](https://www.nosqlbooster.com)
  
  
 

[//]: #@corifeus-footer:end