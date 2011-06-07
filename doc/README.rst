Making your own XTAG2
=====================

The information in this repository enables you to make your own XTAG2.
Try and avoid tha tif possible - you need to program the OTP and any mistake in
the process means that you have to replace the chip.

If you really want to build an XTAG2, for example to integrate it on a design,
then you need to go through the following steps:

#. Follow the hardware design in the `hw` directory. Use a 20-pin header if possible to bring out
   the JTAG wires of the L1 that you are using in your XTAG2. You will need that to program the OTP
   to make it an XTAG2. No need to wire up UART or XLINK on this connector - you just need the six
   JTAG signals and the chip RST signal.

#. Manufacture the board.

#. Connect an XTAG2 to the L1 on your XTAG2 - you may need an adapter cable if you put anything that
   is not a 20-pin header to carry your JTAG signals.

#. Burn the OTP. You do this by running::

     xrun --io 00.02.xe

   on the device that you are programming. Before you run this command make sure that the only device attached is the L1
   that you want to make into an XTAG2. Programming the OTP is irreversible. If you happen to have something else attached, it
   will burn the OTP image on that other L1, rendering it useless.

#. Plug the new XTAG2 into your computer using a USB cable - if it was already plugged in (because the L1 is USB powered),
   then unplug and plug it in to powercycle your new XTAG2.

#. It should come up as a device with id 'XXXXXXXXXXXXXXX' (try xrun --listdevices). If it doesn't, something has gone wrong.
   You may be able to debug it by using gdb and having a look inside your XTAG2.

#. Now unplug all XTAG2 devices except your new one, and run burnSerial in the burn_serial_app directory::

     burnSerial -x D07 0

   This states that your XTAG2 can be used as a JTAG key that has an XLINK and UART wired up. See the documentation in
   app_l1_jtag for more detail on the serial number assignment.