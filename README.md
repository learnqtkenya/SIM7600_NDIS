# SIM7600_NDIS
Updated Linux Driver based on one at https://files.waveshare.com/upload/1/12/SIM7600X-Driver.7z

## Original version  build

```
~/bins/SIM7600_NDIS via C v13.3.0-gcc 
❯ make
rm -rf *.o *~ core .depend .*.cmd *.ko *.mod.c .tmp_versions Module.* modules.order
make -C /lib/modules/6.8.0-52-generic/build M=/home/erico/bins/SIM7600_NDIS modules
make[1]: Entering directory '/usr/src/linux-headers-6.8.0-52-generic'
warning: the compiler differs from the one used to build the kernel
  The kernel was built by: x86_64-linux-gnu-gcc-13 (Ubuntu 13.3.0-6ubuntu2~24.04) 13.3.0
  You are using:           gcc-13 (Ubuntu 13.3.0-6ubuntu2~24.04) 13.3.0
  CC [M]  /home/erico/bins/SIM7600_NDIS/simcom_wwan.o
/home/erico/bins/SIM7600_NDIS/simcom_wwan.c: In function ‘simcom_wwan_bind’:
/home/erico/bins/SIM7600_NDIS/simcom_wwan.c:85:39: error: assignment of read-only location ‘*dev->net->dev_addr’
   85 |                 dev->net->dev_addr[0] |= 0x02;  /* set local assignment bit */
      |                                       ^~
/home/erico/bins/SIM7600_NDIS/simcom_wwan.c:86:39: error: assignment of read-only location ‘*dev->net->dev_addr’
   86 |                 dev->net->dev_addr[0] &= 0xbf;  /* clear "IP" bit */
      |                                       ^~
/home/erico/bins/SIM7600_NDIS/simcom_wwan.c: In function ‘simcom_wwan_suspend’:
/home/erico/bins/SIM7600_NDIS/simcom_wwan.c:118:24: warning: unused variable ‘dev’ [-Wunused-variable]
  118 |         struct usbnet *dev = usb_get_intfdata(intf);
      |                        ^~~
/home/erico/bins/SIM7600_NDIS/simcom_wwan.c: In function ‘simcom_wwan_resume’:
/home/erico/bins/SIM7600_NDIS/simcom_wwan.c:136:1: warning: label ‘err’ defined but not used [-Wunused-label]
  136 | err:
      | ^~~
/home/erico/bins/SIM7600_NDIS/simcom_wwan.c:131:24: warning: unused variable ‘dev’ [-Wunused-variable]
  131 |         struct usbnet *dev = usb_get_intfdata(intf);
      |                        ^~~
/home/erico/bins/SIM7600_NDIS/simcom_wwan.c: At top level:
/home/erico/bins/SIM7600_NDIS/simcom_wwan.c:141:17: warning: no previous prototype for ‘simcom_wwan_tx_fixup’ [-Wmissing-prototypes]
  141 | struct sk_buff *simcom_wwan_tx_fixup(struct usbnet *dev, struct sk_buff *skb, gfp_t flags)
      |                 ^~~~~~~~~~~~~~~~~~~~
make[3]: *** [scripts/Makefile.build:243: /home/erico/bins/SIM7600_NDIS/simcom_wwan.o] Error 1
make[2]: *** [/usr/src/linux-headers-6.8.0-52-generic/Makefile:1925: /home/erico/bins/SIM7600_NDIS] Error 2
make[1]: *** [Makefile:240: __sub-make] Error 2
make[1]: Leaving directory '/usr/src/linux-headers-6.8.0-52-generic'
make: *** [Makefile:8: all] Error 2
```

## Key Changes

- Fixed the MAC address modification in simcom_wwan_bind():
- Added a temporary buffer mac_addr
    - Used eth_hw_addr_set() to properly set the MAC address
- Simplified the suspend/resume functions by removing unnecessary code:
    - Removed unused variables
    - Removed unused error labels
    - Simplified error handling
- Code style improvements:
    - Changed printk to dev_info and dev_dbg for better debugging
    - Improved formatting and consistency
    - Added function prototype for simcom_wwan_tx_fixup

