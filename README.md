# pes_uarxtxp=> UNIVERSAL ASYNCHRONOUS TRANSMITTER RECEIVER PROTOCOL
This project simulates the designed UART Transmitter module which is used to transmit a data packet between devices using Asynchronous and Serial communication.The data length can vary from 5 bit to 8 bit but it must be decided before the communication begins along with the baud rate(in this case the baud rate is 115200).
# INTRODUCTION
UART is a Hardware based digital communication protocol used to transmit data between two device at an agreeded upon baud rate and data length.
It used to transmit binary data serially starting from LSB to MSB in the form of data frames with a start bit and a stop bit. It transmits the data asynchronously through configurable and agreed upon baud rates.Baud rate is the rate at which information is being share(in this case information is in terms of bits) (eq 1). Data frame is the arrangement of data packets with a start bit before the MSB and one or two stop bit at the end of data packet.Start bit is a high-to-low pulse which indicates the start of the transmission and a stop bit is a High pulse to indicate the end of the operation.

# APPLICATIONS

* Interfacing of Sensors and communication modules to the microprocessor or microcontroller
* Transferring data through PC serial port.
* Baud rate generation for numerous applications that helps to determine the speed of data transmission.

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
cd vrl
cd pes_uarxtxp
iverilog -o pes_uarxtxp_tb.vvp pes_uarxtxp_tb.v
vvp pes_uarxtxp_tb.vvp
gtkwave dump.vcd

```

![2efa9e72-6ffe-4012-b808-42abb2a7748d](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/33b9b4ca-c266-4639-b277-c006e0b95f49)

![b22cf34d-f7da-413d-bb76-9de39ab0ab0e](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/fa30d9d5-be20-45b5-ba01-52bcefdd7fcc)

#Synthesis
Invoke yosys
```
read_liberty -lib ../lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog UART_TX.v
synth -top iiitb_ptvm
dfflibmap -liberty lib/sky130_fd_sc_hd__tt_025C_1v80.lib
abc -liberty lib/sky130_fd_sc_hd__tt_025C_1v80.lib
stat
show
write_verilog iiitb_uarttx_netlist.v
iverilog -DFUNCTIONAL -DUNIT_DELAY=#1 my_lib/verilog_model/primitives.v my_lib/verilog_model/sky130_fd_sc_hd.v iiitb_uarttx_netlist.v iiitb_uarttx_tb.v
./a.out
gtkwave dump.vcd
```
![5a141c76-ed4e-49a5-942a-9fa61d57c82e](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/1055dad5-50a7-48f2-8e5c-31942038435b)

<img width="248" alt="Screenshot 2023-10-16 102053" src="https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/30383614-f728-4a44-9b30-4d868ca79dc0">


