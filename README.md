# Departmental Network Design, Configuration and Connectivity Testing

## Project Overview

This project demonstrates the design, configuration, and testing of a small departmental computer network using **Cisco Packet Tracer**.

The network supports three departments:

- Information Technology (IT)
- Sales
- Human Resources (HR)

Each department has its own network switch and IPv4 subnet. A central Cisco router connects the departmental networks, while a server is located within the IT department.

The project focuses on network topology design, Ethernet connections, IPv4 addressing, DHCP configuration, router interface verification, and connectivity testing.

## Project Objectives

The main objectives of this project are to:

- Design a departmental network topology using Cisco Packet Tracer.
- Connect the IT, Sales, and HR departments through a central router.
- Connect computers and a server to their departmental switches.
- Allocate separate IPv4 subnets to each department.
- Configure appropriate default gateway addresses.
- Verify DHCP address assignment on an IT computer.
- Check router interface addresses and operational status.
- Test network connectivity using Command Prompt.
- Document the network design and configuration results.

## Network Topology

The network uses a centralised design in which a router connects to three departmental switches. Each switch provides network connectivity for the devices within its department.

The network consists of:

- **1 Cisco 2911 router** to provide gateway interfaces and route traffic between departmental networks.
- **3 Cisco 2960 switches** to connect computers and the server.
- **6 PCs** to represent departmental users.
- **1 server** connected to the IT departmental network.

### Network Structure

```text
                    Cisco 2911 Router
                     /      |      \
                    /       |       \
              IT Switch  Sales Switch  HR Switch
                / | \       / | \       / | \
              PC  PC Server PC PC       PC PC

                IT       Sales        HR
             192.168.10.0/24
                       192.168.20.0/24
                                    192.168.30.0/24
```

This diagram illustrates the intended network structure, with the router connecting the three departmental networks.

## Network Devices and Their Functions

| Device | Model | Quantity | Function |
|---|---|---:|---|
| Router | Cisco 2911 | 1 | Provides gateways and routes traffic between networks. |
| Switch | Cisco 2960 | 3 | Connects devices within each department. |
| PC | PC-PT | 6 | Provides network access for departmental users. |
| Server | Server-PT | 1 | Provides a server resource within the IT subnet. |

## Network Cabling and Connections

Copper straight-through Ethernet cables are used to connect the router to the departmental switches and the switches to the end devices.

The router uses three GigabitEthernet interfaces, with each interface assigned to a departmental subnet.

| Department | Router Interface | Gateway IP Address |
|---|---|---|
| IT | `GigabitEthernet0/0` | `192.168.10.1` |
| Sales | `GigabitEthernet0/1` | `192.168.20.1` |
| HR | `GigabitEthernet0/2` | `192.168.30.1` |

Correct cabling and active interfaces are essential for network communication. Each PC and the server must also be connected to the appropriate departmental switch.

## IPv4 Addressing and Subnet Allocation

The network is divided into three IPv4 subnets, each using a `/24` prefix and a subnet mask of `255.255.255.0`.

| Department | Network Address | Subnet Mask | Usable Host Range | Default Gateway |
|---|---|---|---|---|
| IT | `192.168.10.0/24` | `255.255.255.0` | `192.168.10.1–192.168.10.254` | `192.168.10.1` |
| Sales | `192.168.20.0/24` | `255.255.255.0` | `192.168.20.1–192.168.20.254` | `192.168.20.1` |
| HR | `192.168.30.0/24` | `255.255.255.0` | `192.168.30.1–192.168.30.254` | `192.168.30.1` |

The network address ending in `.0` identifies the subnet, while the address ending in `.255` is the broadcast address for each `/24` subnet. Neither is assigned to an ordinary host.

The default gateway is the router interface a device uses to send traffic to destinations outside its local subnet.

## DHCP Configuration

**Dynamic Host Configuration Protocol (DHCP)** allows devices to obtain their network settings automatically.

In this project, the IT PC was configured to obtain its IPv4 settings through DHCP.

The configuration screenshot displayed the following information:

- **IPv4 address:** `192.168.10.6`
- **Subnet mask:** `255.255.255.0`
- **Default gateway:** `192.168.10.1`
- **DNS server:** `192.168.10.2`
- **Address assignment method:** DHCP

The screenshot displayed the message **"DHCP request successful."**

This confirms that the IT PC received network settings through a DHCP service. However, it does not establish that DHCP is working on every computer or in every department.

## IP Address Verification

The `ipconfig` command was used on the IT PC to inspect its network configuration.

```bash
ipconfig
```

This command displays network information, including the IPv4 address, subnet mask, and default gateway.

The captured output confirmed the following settings:

- **IPv4 address:** `192.168.10.6`
- **Subnet mask:** `255.255.255.0`
- **Default gateway:** `192.168.10.1`

These values place the PC within the IT subnet and identify the router interface it uses as its default gateway.

## Network Connectivity Testing

Connectivity was tested using the `ping` command from the IT PC.

The command used was:

```bash
ping 192.168.10.5
```

The test checked whether the target device at `192.168.10.5` responded to network requests.

### Ping Test Results

| Test Statistic | Result |
|---|---:|
| Packets sent | 4 |
| Packets received | 4 |
| Packets lost | 0 |
| Packet loss | 0% |
| Minimum round-trip time | 0 ms |
| Maximum round-trip time | 1 ms |
| Average round-trip time | 0 ms |

The four successful replies indicate that the target device responded to all four ICMP echo requests during the test.

The **0% packet loss** result confirms successful connectivity between the source PC and the target IP address at the time of testing.

Further tests are needed to confirm connectivity to each departmental gateway and between devices in different subnets.

## Router Configuration and Interface Verification

The router provides the gateway interfaces for the three departmental networks.

The following Cisco IOS command was used to inspect the router's interfaces:

```text
show ip interface brief
```

This command displays interface names, assigned IP addresses, and operational status.

### Router Interface Results

| Interface | IP Address | Status | Protocol | Department |
|---|---|---|---|---|
| `GigabitEthernet0/0` | `192.168.10.1` | Up | Up | IT |
| `GigabitEthernet0/1` | `192.168.20.1` | Up | Up | Sales |
| `GigabitEthernet0/2` | `192.168.30.1` | Up | Up | HR |
| `Vlan1` | Unassigned | Administratively down | Down | Not used as a departmental gateway |

The `up/up` status indicates that the three GigabitEthernet interfaces and their line protocols were active when the screenshot was captured.

The interface addresses also match the default gateways in the IPv4 addressing plan.

This confirms the configured gateway addresses and interface status, but it does not independently prove that every departmental network can communicate successfully.

## Testing and Troubleshooting

Network testing helps identify configuration problems that may prevent devices from communicating.

The following checks are recommended for this project.

### 1. Physical Connectivity

- Confirm that the router connects to all three departmental switches.
- Check that every PC connects to the correct switch.
- Confirm that the server connects to the IT switch.
- Check the link indicators in Cisco Packet Tracer.

### 2. IP Addressing

- Confirm that IT devices use the `192.168.10.0/24` subnet.
- Confirm that Sales devices use the `192.168.20.0/24` subnet.
- Confirm that HR devices use the `192.168.30.0/24` subnet.
- Check the subnet mask and default gateway on each device.
- Investigate any duplicate IP addresses.

### 3. DHCP Verification

- Confirm that DHCP requests succeed.
- Check that assigned IP addresses belong to the correct subnet.
- Verify the default gateway and DNS settings.
- Check DHCP scopes for other departments if they are intended to use DHCP.

### 4. Connectivity Verification

- Use `ipconfig` to inspect PC network settings.
- Ping each PC's default gateway.
- Test communication between devices on the same subnet.
- Test communication between devices in different subnets.
- Check IP addresses, subnet masks, gateways, router interfaces, and physical connections when tests fail.

The available evidence confirms a successful DHCP request on the IT PC, a successful ping to `192.168.10.5`, and active router interfaces. The remaining tests should be completed before considering the entire network fully verified.

## Recommendations

The following improvements would help make the network more reliable and easier to maintain:

- **Maintain a consistent topology:** Ensure the final network diagram clearly shows the router connected to all three departmental switches.
- **Document IP addresses:** Keep a record of the IP addresses assigned to each PC and the server.
- **Verify DHCP scopes:** Ensure DHCP settings match the correct subnet and gateway.
- **Prevent address conflicts:** Make sure static IP addresses do not overlap with dynamically assigned addresses.
- **Test every department:** Run connectivity tests from PCs in IT, Sales, and HR.
- **Keep configuration evidence:** Save screenshots of the topology, router interfaces, DHCP settings, and connectivity results.
- **Verify server services:** Confirm that DNS or other services are enabled and configured if the server is intended to provide them.

## Project Screenshots

The original assignment contains screenshots documenting the network design and configuration process.

The following evidence should be included in the project repository:

- Network topology showing the router, switches, PCs, and server.
- DHCP configuration and successful address assignment.
- IT PC network settings.
- Router configuration and interface verification.
- Command Prompt output showing IP configuration.
- Successful ping test results.

Each screenshot should be placed under the relevant section, with a short explanation of what it demonstrates.

## Conclusion

This project demonstrates the design and configuration of a small departmental network using Cisco Packet Tracer.

The network includes a Cisco 2911 router, three Cisco 2960 switches, six PCs, and one server. The IT, Sales, and HR departments use separate IPv4 subnets, with the router providing the corresponding gateway addresses.

The available screenshots provide evidence of successful DHCP address assignment on the IT PC, correct IPv4 settings, a successful ping test with no packet loss, and three active router interfaces.

The project provides practical experience in network topology design, Ethernet connections, IPv4 addressing, DHCP, router interface verification, and connectivity testing.

Further testing of the departmental connections, DHCP configuration, server services, and interdepartmental communication is recommended to verify the complete network.

---

## Technologies Used

- **Cisco Packet Tracer**
- **Cisco 2911 Router**
- **Cisco 2960 Switches**
- **IPv4 Addressing**
- **DHCP**
- **DNS Address Configuration**
- **Cisco IOS CLI**
- **Windows Command Prompt**
- **Ping Connectivity Testing**

## Project Status

**Core configuration documented; additional network verification recommended.**
