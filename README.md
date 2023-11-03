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

![18432b16-2110-492d-a6af-97cb23533aed](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/53df3800-99d5-4ea3-92f5-7c22d2f28a61)

![950497a2-89dd-425a-9907-fc5bd8348da7](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/263f4365-37af-4c63-a4a7-efe389e74c44)

![019b2dd9-7a97-4f32-833b-9f243899dcb4](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/b03088f8-c749-4b66-8068-b4453d8f7eca)

![79cc09b9-f84e-431a-ad22-646add92e15a](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/8ab9a6fc-a3ae-4d2f-8e44-126a9e54514b)

![9719d9db-62f2-458f-9421-98a658cfd5fb](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/87d6d965-bead-4058-b0c3-4d02c78cec04)

# Physical Design
## Installation of ngspice magic and OpenLANE

**ngspice**
- Download the tarball from https://sourceforge.net/projects/ngspice/files/ to a local directory
```
cd $HOME
sudo apt-get install libxaw7-dev
tar -zxvf ngspice-41.tar.gz
cd ngspice-41
mkdir release
cd release
../configure  --with-x --with-readline=yes --disable-debug
sudo make
sudo make install
```

**magic**
```
sudo apt-get install m4
sudo apt-get install tcsh
sudo apt-get install csh
sudo apt-get install libx11-dev
sudo apt-get install tcl-dev tk-dev
sudo apt-get install libcairo2-dev
sudo apt-get install mesa-common-dev libglu1-mesa-dev
sudo apt-get install libncurses-dev
git clone https://github.com/RTimothyEdwards/magic
cd magic
./configure
sudo make
sudo make install
```

**OpenLANE**
```
sudo apt-get update
sudo apt-get upgrade
sudo apt install -y build-essential python3 python3-venv python3-pip make git

sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
sudo docker run hello-world
sudo groupadd docker
sudo usermod -aG docker $USER
sudo reboot 
# After reboot
docker run hello-world (should show you the output under 'Example Output' in https://hub.docker.com/_/hello-world)

- To install the PDKs and Tools
cd $HOME
git clone https://github.com/The-OpenROAD-Project/OpenLane
cd OpenLane
make
make test
```

## Steps to follow
make the directory as shown below and upload all the required files along with verilog code

![ee2d88a6-0ba7-4f45-9053-319f4dd585cd](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/006306b3-18f4-43af-8064-b44873564ec3)
![2e988de3-d3f1-40aa-b062-6e71216ecdca](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/a990f4ab-8250-42ed-9aba-afe68be653e6)

## Openlane flow
- Type ```make mount``` in the main Openlane folder.
- Then type ```./flow.tcl -interactive```.
- To prep the design type
```
prep -design UART_TX
```
![83ea922b-ae4e-4d28-9208-103c2dbb142d](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/168ea822-afc9-46cf-bdbb-02e4df38f323)

### Synthesis
- Type
```
run_synthesis
```
![9c7095de-bf8d-4cd5-a944-f32ea7499167](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/3be988a9-0f47-4b56-b62e-d0a3a1ad13fa)
![aee51d42-a8a2-4037-8cfd-1bd6dfae23c8](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/89b44c56-a82b-4c28-8cd3-3abff8c9cae4)

Flop ratio=25/112

### Floorplan
- Now to run the floorplan we type
```
run_floorplan
```
![20a7fb7f-4f23-4b24-a582-f3ad9e5f9d78](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/da3150bf-68ae-4fa6-a88c-71f7b22b688e)

- To view the design we type
```
magic -T /home/apoorva/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def UART_TX.def &
```
![bc6ec8b3-faaa-4724-af07-523d9257c9c7](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/1ae20332-820b-4957-8c2a-31436b392b0c)
![be36db5e-1d65-4dd4-9e68-ab0d77fcb8ad](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/c6e988b9-5411-4788-b6d7-92880ae02c6e)

### Placement
- Now to run the placement we type
```
run_placement
```
- To view the design we type
```
magic -T /home/apoorva/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def UART_TX.def &
```
![6318ed8e-097a-4110-b3e3-31305a9eaf21](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/fb87ec38-2b68-4d30-958b-cdf1ac119660)

### CTS(Clock Tree Synthesis)
- Now to run cts we type
```
run_cts
```
![499fd0d5-df3e-4aa1-ba64-7a1da4edc124](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/4dcc6489-a818-4e54-9168-1ede80f4c5a4)

#### Power Report

![51bb70da-2fd4-4a10-a43c-def355a3860f](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/fb44b730-16bb-4145-876c-27f079031501)

![8bfac8f2-3cc9-4b86-9045-5b7debd339f2](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/448d08d4-c296-4137-a7a7-db81eb428eda)
![04b87e02-d874-4e79-ba62-59443d14faa7](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/dac28ab1-1f77-4589-88b4-d486a27c3abd)
![34b84ed1-d3b5-42fb-ac3f-72c442b1ccc6](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/cfbc457d-8810-411b-b681-d82c7de2b7b7)
![e330a2a3-e955-44fc-9702-f9ef53d901ef](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/57059651-8061-4378-8564-2d852adab166)
![1dff4f35-54df-457a-a008-9f3dc24a80ad](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/07682761-08b5-4d20-a274-8dab8e75e9eb)

#### Skew report

As it is an asynchronous digital system you get below report

![e1056ab6-becc-41ad-9e34-9ba9998c7925](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/a9602f3e-b1df-49fa-8a0e-b4bbd635d042)

#### Area report

![faa1921f-acfd-4fbd-8800-98575450424f](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/b5c6f08e-1989-41af-a039-6856054b7e47)

### Routing
- Now to run routing we type
```
run_routing
```
- To view the design we type
```
magic -T /home/apoorva/.volare/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.nom.lef def UART_TX.def &
```
![8471278b-4298-4141-af4b-6d1105acb788](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/cc556f9f-1cb4-4187-aa73-bd9c5ae786f8)
![7cf833e3-7a27-4070-95d7-5d90f2775df3](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/430cc3b8-8d20-4169-903b-68d9bfb0e48e)
![d35750d9-f550-497a-ad8d-7d5273d77cb4](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/e71f173b-3d29-4e0d-9959-6fb24c9dbfe8)

#### Power report
![d20a125c-c8d8-44c5-8e26-2e3e639ef692](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/1fe9efd1-d760-4739-a978-7655ad1ed99d)
#### Congestion report
![d5ffffe1-6ab8-436b-8631-9501aed8d03a](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/af65ef6a-4d53-4915-8564-a74b956c954d)
#### Area report
![faa1921f-acfd-4fbd-8800-98575450424f](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/7ececa11-9d8d-4508-83bc-8524bacaba0e)
#### Summary report
![0c9daf09-e7e0-4176-ab5e-0d203391f091](https://github.com/apoorvaaaa5/pes_uarxtxp/assets/117642634/b217a99c-f4a9-4db3-b8ae-c875ecd82204)

Statistics
- Area= 1256 u^2
- Total power= 1.42 e^04 watts
- Internal power= 8.38 e^05 watts
- Switching power=5.84 e^05 watts
- Leakage power= 6.42 e^10 watts
