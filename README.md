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

![2efa9e72-6ffe-4012-b808-42abb2a7748d](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/33b9b4ca-c266-4639-b277-c006e0b95f49)

![b22cf34d-f7da-413d-bb76-9de39ab0ab0e](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/fa30d9d5-be20-45b5-ba01-52bcefdd7fcc)

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
![5a141c76-ed4e-49a5-942a-9fa61d57c82e](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/1055dad5-50a7-48f2-8e5c-31942038435b)

![62488ce0-88ad-4702-ac73-6195b30ad230](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/f0ac25df-c3a1-4ab3-bae9-78c5f48dc48d)

