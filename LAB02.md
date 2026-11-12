# Lab 02 - Local Area Network

## Devices:

- 4 PCs
- Switch CISCO SF350
- Cables,...

## Practice:

### 1. Login

- Use a console cable to connect to the switch from a PC (baud rate: 115200, 8 data bits, no parity, 1 stop bit), then log in with the provided account and password.

### 2. Create a VLAN

After logging in, you will be in **privileged mode** (`switch#`).

> **Note:** The switch CLI does not support comments. Type only the commands in the code blocks below. Do not copy any explanation into the terminal.

#### 2.1. How to get the interfaces' names?

- In privileged mode:

```sh
show interfaces status
```

- The Cisco Sx350 series names its ports in the unit/slot/port format:
  - 10/100 ports: `fa1/0/1`, `fa1/0/2`,...
  - Gigabit ports: `gi1/0/1`, `gi1/0/2`,...
- Other types of interfaces:
  - VLAN: `vlan 11`, `vlan 67`,...
  - Other types: we won't use them in this lab.

#### 2.2. Create a VLAN

- Enter **Global Configuration Mode** (`switch(config)#`) from privileged mode:

```sh
configure terminal
```

- Create a VLAN. `vlan database` changes the prompt to `switch(config-vlan)#`, and `exit` returns to `switch(config)#`:

```sh
vlan database
vlan 67
exit
```

- **(Optional)** Give it a name. `interface vlan 67` changes the prompt to `switch(config-if)#`, and `exit` returns to `switch(config)#`:

```sh
interface vlan 67
name lab
exit
```

#### 2.3. Assign ports to the VLAN (access port)

> You must be in **Global Configuration Mode** (`switch(config)#`).

- For each port that your PCs are connected to, replace `fa1/0/1` with your interface name and `67` with the VLAN ID you created earlier. The VLAN must be given by its **ID**, not its name.
- `interface fa1/0/1` changes the prompt to `switch(config-if)#`, and `exit` returns to `switch(config)#`:

```sh
interface fa1/0/1
switchport mode access
switchport access vlan 67
exit
```

- Repeat the block above for every port. Alternatively, configure several ports at once with `interface range`:

```sh
interface range fa1/0/1-4
switchport mode access
switchport access vlan 67
exit
```

- When you are done, go back to privileged mode (`switch#`):

```sh
exit
```

#### 2.4. (Optional) Check the configuration

> You must be in **privileged mode** (`switch#`).

```sh
show vlan
show interfaces switchport fa1/0/1
show mac address-table
```

- `show vlan`: VLAN 67 must contain `fa1/0/1`,... (or the ports you assigned) as untagged.
- `show mac address-table`: after the PCs ping each other, the MAC addresses of the PCs appear in VLAN 67.

> The configuration is used only during the class session. You don't need to (and shouldn't) save the configuration.

### 3. On your PCs:

- Manually assign a static IP address and subnet mask to each PC, following the teacher's instructions.
- A default gateway is not needed, so leave it empty. If your OS requires one, enter an unused IP address in the same subnet (one that no PC is using).
- Set the network connection profile to **Private Network**.
- **TURN OFF THE FIREWALL FOR PRIVATE NETWORKS** (otherwise Windows blocks ping/ICMP).
- Test the connection by pinging the other PCs' IP addresses, for example `ping <IP of another PC>`.
- Turn the firewall back on after the lab.
