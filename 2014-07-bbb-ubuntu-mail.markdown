Installed Ubuntu 14.o4 to eMMC successfully.

Ethernet PHY appears to be flaky. On some boots, Ethernet fails, and appears in `dmesg`:

    [  399.182341] libphy: PHY 4a101000.mdio:00 not found
    [  399.187508] net eth0: phy 4a101000.mdio:00 not found on slave 0
    [  399.193839] libphy: PHY 4a101000.mdio:01 not found
    [  399.198983] net eth0: phy 4a101000.mdio:01 not found on slave 1

Power cycling suggested here: https://groups.google.com/d/msg/beagleboard/9mctrG26Mc8/4FjzQDdjrEIJ

Cycled the power, and it seems to work now.
