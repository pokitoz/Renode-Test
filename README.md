# Renode-Test

- MIT - License
- Active community with with nice docs
- Emulator can help getting inside the system (step by step state,..)
  - Most of the time we don't need to emulate everything (AHB bus, BLE modulator, Interconnects)
  - We want to emulate peripherals (RNG, I2C,..), memory and code execution
- Example with BLE using nrf52840 https://renode.readthedocs.io/en/latest/tutorials/ble-simulation.html
- https://zephyr-dashboard.renode.io/renodepedia/boards/ there are many .repl available
  - Not all of them have BLE! Only nrf52840 for now.. See radio: Wireless.NRF52840_Radio
- Use convertor DTS to REPL https://github.com/antmicro/dts2repl: dts2repl
  - Use it on `build/zephyr/zephyr.dts`
- Available peripherals:
  - LIS2DW12 accelerometer sensor
- Example demo https://www.youtube.com/watch?v=AuDlYbwVF0g

(monitor) mach create "nrf52-0"
(nrf52-0) machine LoadPlatformDescription @output.repl
(nrf52-0) sysbus LoadHEX @build/zephyr/zephyr.hex
(nrf52-0) sysbus LoadELF @build/zephyr/zephyr.elf

- Start renode `renode --port 1234 --console --disable-xwt`
- UART:
  - using sysbus
  - emulation CreateServerSocketTerminal 4567 "uart-con"
  - connector Connect uart0 uart-con
  - telnet localhost -d 4567

- Use a script with `i @renode.resc`:
```
using sysbus

mach create "nrf52-0"
machine LoadPlatformDescription @output.repl
$bin?=

using sysbus
emulation CreateServerSocketTerminal 4567 "uart-con"
emulation SetGlobalQuantum "0.00001"
connector Connect uart0 uart-con
showAnalyzer uart0

macro reset
"""
    sysbus LoadELF @build/zephyr/zephyr.elf
"""
runMacro $reset
echo "Script loaded. Now start with with the 'start' command."
echo ""
```
