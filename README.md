# Renode-Test

- Use convertor DTS to REPL https://github.com/antmicro/dts2repl
- UART:
  - emulation CreateServerSocketTerminal 4567 "uart-con"
  - connector Connect uart0 uart-con
  - telnet localhost -d 4567

