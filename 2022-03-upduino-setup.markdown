Setting up toolchain for Upduino 3.0 on Fedora 35

```
sudo dnf install fpga-icestorm fpga-verilog nextpnr python3-pip yosys
pip3 install apio
git clone git@github.com:tinyvision-ai-inc/UPduino-v3.0.git
```
(Previously installed `arachne-pnr`, but it has been replaced by `nextpnr`.)

### Build the RGB blink example

`cd RTL/UPduino-v3.0/RTL/blink_led`

To account for recent regression in Yosys mentioned here: https://github.com/im-tomu/fomu-workshop/pull/600
needed to change build command in Makefile for `rgb_blink.json` to:
    `yosys -q -p "read_verilog rgb_blink.v; synth_ice40 -json rgb_blink.json" rgb_blink.v`
(Added `read_verilog rgb_blink.v; `)

Then `make` seems to work.

### Upload binary to Upduino

```
iceprog rgb_blink.bin
init..
cdone: high
reset..
cdone: low
flash ID: 0xEF 0x70 0x16 0x00
file size: 104090
erase 64kB sector at 0x000000..
erase 64kB sector at 0x010000..
programming..
done.                 
reading..
VERIFY OK             
cdone: high
Bye.
```
