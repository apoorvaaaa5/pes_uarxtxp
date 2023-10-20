# pes_uarxtxp=> UNIVERSAL ASYNCHRONOUS TRANSMITTER RECEIVER PROTOCOL
This project simulates the designed UART Transmitter module which is used to transmit a data packet between devices using Asynchronous and Serial communication.The data length can vary from 5 bit to 8 bit but it must be decided before the communication begins along with the baud rate(in this case the baud rate is 115200).
# INTRODUCTION
##What is UARTP?
UART stands for universal asynchronous receiver / transmitter and defines a protocol, or set of rules, for exchanging serial data between two devices. UART is very simple and only uses two wires between transmitter and receiver to transmit and receive in both directions. Both ends also have a ground connection. Communication in UART can be simplex (data is sent in one direction only), half-duplex (each side speaks but only one at a time), or full-duplex (both sides can transmit simultaneously). Data in UART is transmitted in the form of frames. The format and content of these frames is briefly described and explained.

* UART stands for universal asynchronous receiver / transmitter and is a simple, two-wire protocol for exchanging serial data.
* Asynchronous means no shared clock, so for UART to work, the same bit or baud rate must be configured on both sides of the connection.
* Start and stop bits are used to indicate where user data begins and ends, or to “frame” the data.
* An optional parity bit can be used to detect single bit errors.
* UART is still a widely used serial data protocol but has been replaced in some applications by technologies such as SPI, I2C, USB, and Ethernet in recent years.
  
# Where is UARTP used?

UART was one of the earliest serial protocols. The once ubiquitous serial ports are almost always UART-based, and devices using RS-232 interfaces, external modems, etc. are common examples of where UART is used.
In recent years, the popularity of UART has decreased: protocols like SPI and I2C have been replacing UART between chips and components. Instead of communicating over a serial port, most modern computers and peripherals now use technologies like Ethernet and USB. However, UART is still used for lower-speed and lower-throughput applications, because it is very simple, low-cost and easy to implement.
# Block/State diagram of UART Transmitter

![download](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/549e7533-afa6-40b5-b616-75842601db56)

The data packets are arranged with necessary bits like start bit, stop bit, parity checker etc to optimise the error, such an arrangement is known as data frame.This data frame is shared serially through the transmitter.

![images](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/d4fd2461-7f05-4355-a8c4-83ae3b5730f3)


The transmiter is an Finite State Machine with 4 stable state which are IDLE, START, DATA and STOP.Initially the sequential circuit is in the IDLE state and when the transmission is enabled than the system goes to the START state where it sends an High to Low pulse on the Rx pin and it switches to DATA state , in this state the data stored in the buffered is send one by one with LSB as the first bit and MSB as the last bit.Once the data is transmitted serially at a predetermined baud rate than the system moves to the STOP state where it mantains the Rx line to HIGH state which indicates the end of the operation

# Iverilog and yosys Installation
- To install iverilog and gtkwave we type the following
```
- sudo apt-get update
- sudo apt-get install iverilog gtkwave
```

- To install yosys we type the following
```
- git clone https://github.com/YosysHQ/yosys.git
- sudo apt install make
- sudo apt-get install build-essential clang bison flex \
   libreadline-dev gawk tcl-dev libffi-dev git \
   graphviz xdot pkg-config python3 libboost-system-dev \
   libboost-python-dev libboost-filesystem-dev zlib1g-dev
```
- Now we type ```cd yosys``` and go into the yosys folder.
- Now we type
```
sudo make install
```
# Simulation

```
cd vsd
cd VLSI
cd sky130RTLDesignAndSynthesisWorkshop
cd verilog_files
iverilog -o uart_tb.vvp uart_tb.v
vvp uart_tb.vvp
gtkwave dump.vcd

```

![4c5d1bf5-4010-411f-a433-d42f6397c8d4](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/d69e3983-075d-4950-88b1-3d8edd919afd)

![5d481715-25ec-493a-b81e-916c19fa57d6](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/84db2df7-5931-4901-8518-6e8d38b871de)

![c8736fd8-5a33-4a9b-9eb8-f019658b90da](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/cad6882a-55e9-4d2b-aa4f-36fd1b8f4dd2)


#Synthesis
Invoke yosys
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog UART_TX.v
synth -top UART_TX
dfflibmap -liberty lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty lib/sky130_fd_sc_hd__tt_025C_1v80.lib
stat
show
write_verilog uart_net.v
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 my_lib/verilog_model/primitives.v my_lib/verilog_model/sky130_fd_sc_hd.v uart_net.v uart_tb.v
./a.out
gtkwave dump.vcd
```
![ef0c7975-3ac5-495c-947c-3ceef0393e22](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/76cd6f65-f00d-4092-a5f0-7a719032982d)

![30d065fe-61fa-4260-bbdd-1c9cb9670d8a](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/7ed459fd-d66a-4718-b6e0-22ba88ace449)

![8680e22d-2f10-4247-b588-c1d214e609bf](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/f8e8c6df-7149-4f12-beef-984e5b564c56)

![950497a2-89dd-425a-9907-fc5bd8348da7](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/263f4365-37af-4c63-a4a7-efe389e74c44)

![019b2dd9-7a97-4f32-833b-9f243899dcb4](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/b03088f8-c749-4b66-8068-b4453d8f7eca)


