# apstra configlets

* LACP Force up
When servers needs to pxe boot, the aggregated-ether-options force-up needs to be set. This will bring up the interface, even if no LACP initiations are done.
´´´
pto@dc5-2-a1-bl01> show configuration interfaces ae1 | display inheritance
description to.server1;
native-vlan-id 133;
mtu 9100;
esi {
    00:0a:00:00:00:00:04:00:00:04;
    all-active;
}
aggregated-ether-options {
    lacp {
        active;
        system-id 0a:00:00:00:00:04;
        ##
        ## 'force-up' was inherited from group 'lacp-force-up'
        ##
        force-up;
    }
}
unit 0 {
    family ethernet-switching {
        interface-mode trunk;
        vlan {
            members [ vn133 vn1411 vn1412 vn1413 ];
        }
    }
}

{master:0}
pto@dc5-2-a1-bl01>
}´´´
The configlet lace-force.up.json will provison the following on the leaf:
```
set groups lacp-force-up interfaces <ae*> aggregated-ether-options lacp force-up
set interfaces apply-groups lacp-force-up
```

I recomment do apply the configlet with a tag conditon, then you can just add a tag to the leaf you wish to apply lacp force-up
![lacp-configlet](lace-configlet.png)