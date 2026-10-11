# Atlantic Bay Resort & Spa — Enterprise Data Network

A multi-site data network for a fictional resort complex, planned, simulated and configured in GNS3 with Cisco IOS/IOU routers.
The project was developed for the *Data Routing (Encaminhamento de Dados)* course, Computer Engineering degree (Networks and Systems Administration), ISEC.

## Overview

The resort is made up of a central administrative building and 7 units, spread across 3 geographic sectors (North, East and West):

- Edifício Administrativo Central (EAC)
- Hotel Principal (HP)
- Bungalows & Villas (BV)
- Centro de Conferências (CC)
- Spa & Wellness Center (SWC)
- Restauração (R)
- Área Desportiva e Lazer (ADL)
- Serviços Operacionais (SO)

Every building and unit has several routers and subnets, each with at least one end device, and the whole complex has Internet access through a simulated ISP.

## Technologies Used

- **GNS3 v2.2.58** — Network simulation
- **Cisco IOS/IOU** — Router operating system
- **OSPF, EIGRP, RIPv2** — Dynamic routing protocols
- **NAT/PAT, ACLs** — Internet access
- **IPv6 (6to4 tunnels)** — Transition strategy over IPv4

## Network Design

### IPv4 Addressing

- VLSM addressing from an assigned public block, organized in an addressing map of 80 subnets (10 per area, 8 areas).
- Private addressing (10.0.0.0/8) on the links between routers, to avoid wasting public address space.

### Routing

Three routing protocols are used, so that no protocol is repeated inside the same sector:

| Protocol | Areas |
|----------|-------|
| OSPF | Central Administrative Building, Spa & Wellness, Restauração |
| EIGRP | Hotel Principal, Operational Services, Conference Center |
| RIPv2 | Bungalows & Villas, Sports & Leisure Area |

- Border routers (e.g. SO1 and CC1) act as ASBRs, with mutual redistribution between protocols for end-to-end connectivity.
- Automatic summarization disabled on the distance-vector protocols (classless routing).

### Security

- MD5 authentication on all OSPF neighbors and key-chain authentication on RIP.
- Encrypted passwords, login banner (MOTD) and synchronous logging on all routers.

### Internet Access and Traffic Engineering

- NAT Overload (PAT) with ACLs on the edge routers, masking all private subnets.
- EIGRP offset-lists to prefer SO1 as the primary path, with SO3 as the backup path.

### IPv6

- IPv6 addressing and routing in the Conference Center (CC1 and CC2).
- Dynamic 6to4 tunnels to carry IPv6 traffic over the existing IPv4 infrastructure.

## Verification

- Routing tables analyzed on routers from the central building and from each unit (EAC-1, EAC-8, HP-1, BV-1, SO-1, SWC-1, CC-1, ADL-1, R-1).
- End-to-end connectivity tested with `ping 2.2.2.2` (simulated ISP) from every unit, with 100% success rate.

## How to Run

1. Install GNS3 v2.2.58 (or later) with IOU support.
2. Import the project file (`.gns3project`, portable format) in GNS3.
3. Make sure the Cisco IOU router image is available in your GNS3 server.
4. Start all devices and wait for the routing protocols to converge.
5. Console password for all routers: `cisco`.

## Author

Gabriel Cardoso Rodrigues — ISEC, Instituto Politécnico de Coimbra
