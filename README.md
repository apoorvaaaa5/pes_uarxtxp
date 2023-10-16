# pes_uarxtxp
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
![183946249-70286928-a0ad-4af3-8ef9-b8f43dee4789](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/d4785eed-c153-4fe4-b1b9-7e9b8d8c807b)

The data packets are arranged with necessary bits like start bit, stop bit, parity checker etc to optimise the error, such an arrangement is known as data frame.This data frame is shared serially through the transmitter.

![183946295-b4377ab5-b7c9-43dc-965b-2b91989724f9](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/efbaf7bc-5125-4d1e-a035-f09d62f3fad7)

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

