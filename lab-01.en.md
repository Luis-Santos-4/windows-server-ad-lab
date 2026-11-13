# Lab 1 — Lab Environment Setup

**English** | [Português](lab-01.pt.md)

## 1. Objective

In this lab, I set up a virtualization environment to work with Windows Server 2022. The goal was to create two virtual machines, place them on the same virtual network, configure static IPv4 addresses, and confirm that the servers can communicate with each other and reach external services.

Promoting a server to Domain Controller and creating the forest, tree and domain are part of Lab 2.

## 2. Technologies used

- Oracle VirtualBox
- Windows Server 2022 Standard Evaluation
- VirtualBox NAT Network
- IPv4
- Windows PowerShell

## 3. Virtual network configuration

In VirtualBox, I created the NAT Network `LAB-AD`. I chose a network dedicated to the lab so the virtual machines could communicate with each other and access the Internet without being connected directly to the physical network. DHCP was disabled because the servers' IPv4 addresses were configured manually.

| Parameter | Configuration |
|---|---|
| Network name | `LAB-AD` |
| Type | NAT Network |
| IPv4 prefix | `10.10.10.0/24` |
| DHCP | Disabled |
| IPv6 | Disabled |

**Evidence:**

<img src="prints/08-rede-lab.png" width="600" alt="LAB-AD NAT Network configuration">

## 4. First server — DC01

### 4.1. Creating the virtual machine

I created the `VM-DC01` virtual machine in VirtualBox and attached the Windows Server 2022 ISO to it. The installation was done manually, without the unattended installation option, in order to go through each step of the setup wizard.

| Parameter | Configuration |
|---|---|
| Virtual machine name | `VM-DC01` |
| Operating system | Windows Server 2022 Standard Evaluation |
| Interface | Desktop Experience |
| Virtual processors | 2 |
| RAM | 4096 MB |
| Virtual disk | 60 GB |
| Disk format | VDI |
| Disk allocation | Dynamic |
| Network | `LAB-AD` |

**Evidence:**

<img src="prints/01-virtualbox.png" width="600" alt="Oracle VirtualBox">
<img src="prints/02-criacao-vm.png" width="600" alt="Creating the VM-DC01 virtual machine">
<img src="prints/03-disco-virtual.png" width="600" alt="VM-DC01 virtual disk configuration">
<img src="prints/04-edicao-windows-server.png" width="600" alt="Selecting the Windows Server 2022 edition">

### 4.2. Installation and first sign-in

During installation, I selected **Windows Server 2022 Standard Evaluation (Desktop Experience)**. This option includes the graphical interface, which is needed to carry out the lab tasks using the Windows Server tools.

I set a password for the local `Administrator` account and, once the installation finished, signed in to the server.

**Evidence:**

<img src="prints/05-configuracao-administrador.png" width="600" alt="Setting the Administrator password">
<img src="prints/06-windows-server-instalado.png" width="600" alt="Windows Server 2022 installed">

### 4.3. Computer name

After installation, I changed the computer name to `DC01`. I confirmed the name with the command:

```powershell
hostname
```

The output was `DC01`.

### 4.4. IPv4 configuration

I configured the `DC01` network adapter with the following values:

| Parameter | Value |
|---|---|
| IPv4 address | `10.10.10.10` |
| Subnet mask | `255.255.255.0` |
| Default gateway | `10.10.10.1` |
| Initial preferred DNS | `1.1.1.1` |
| Initial alternate DNS | `8.8.8.8` |

I used the following command to check the adapter's full configuration:

```powershell
ipconfig /all
```

At this stage, public DNS servers were used to test name resolution on the Internet. This is an initial configuration and will be revised once the internal DNS service is set up in the next stages of the project.

**Evidence:**

<img src="prints/07-rede-vm-dc01.png" width="600" alt="VM-DC01 network adapter connected to LAB-AD">
<img src="prints/09-configuracao-ipv4.png" width="600" alt="DC01 IPv4 configuration">

### 4.5. Connectivity and DNS tests

On `DC01`, I ran the following commands:

```powershell
ping 1.1.1.1
nslookup www.microsoft.com
```

`ping 1.1.1.1` received four replies with no packet loss. `nslookup www.microsoft.com` returned a DNS response, confirming that external name resolution was working.

**Evidence:**

<img src="prints/10-teste-ping.png" width="600" alt="Ping test to 1.1.1.1 on DC01">
<img src="prints/11-teste-dns.png" width="600" alt="nslookup test on DC01">

## 5. Second server — DC02

### 5.1. Creation and installation

I created a second virtual machine named `VM-DC02`, using the same Windows Server 2022 ISO and the same virtual resources as the first machine.

| Parameter | Configuration |
|---|---|
| Virtual machine name | `VM-DC02` |
| Operating system | Windows Server 2022 Standard Evaluation |
| Interface | Desktop Experience |
| Virtual processors | 2 |
| RAM | 4096 MB |
| Virtual disk | 60 GB |
| Disk format | VDI |
| Disk allocation | Dynamic |
| Network | `LAB-AD` |

After installation, I changed the computer name to `DC02`.

**Evidence:**

<img src="prints/12-criacao-vm2.png" width="600" alt="Creating the VM-DC02 virtual machine">
<img src="prints/13-memoria-virtual2.png" width="600" alt="VM-DC02 memory">
<img src="prints/14-disco-virtual2.png" width="600" alt="VM-DC02 virtual disk">
<img src="prints/15-maquina-virtual2.png" width="600" alt="VM-DC02 summary">
<img src="prints/17-troca-nome-dc02.png" width="600" alt="Changing the computer name to DC02">

### 5.2. IPv4 configuration

I configured the `DC02` network adapter with the following static address:

| Parameter | Value |
|---|---|
| IPv4 address | `10.10.10.11` |
| Subnet mask | `255.255.255.0` |
| Default gateway | `10.10.10.1` |
| Initial preferred DNS | `1.1.1.1` |
| Initial alternate DNS | `8.8.8.8` |

I checked the configuration with:

```powershell
ipconfig /all
```

**Evidence:**

<img src="prints/16-rede-vm-dc02.png" width="600" alt="VM-DC02 network adapter connected to LAB-AD">
<img src="prints/18-configurar-ipv4.png" width="600" alt="DC02 IPv4 configuration">
<img src="prints/19-configuracao-ipv4-dc02.png" width="600" alt="Verifying the DC02 IPv4 configuration">

### 5.3. Connectivity and DNS tests

On `DC02`, I ran:

```powershell
ping 1.1.1.1
nslookup www.microsoft.com
```

The connectivity test received four replies with no packet loss. `nslookup` also returned a DNS response for `www.microsoft.com`.

**Evidence:**

<img src="prints/24-teste-ping-dns.png" width="600" alt="Ping and DNS tests on DC02">

## 6. Communication between the servers

With both servers on the `LAB-AD` network, I tested communication using ICMPv4.

### 6.1. First test from DC02

Initially, the command:

```powershell
ping 10.10.10.10
```

run on `DC02` failed, with four requests timing out and 100% packet loss.

**Evidence of the initial result:**

<img src="prints/20-erro-conexao.png" width="600" alt="Ping from DC02 to DC01 with no reply">

### 6.2. Firewall rule on DC01

To allow inbound IPv4 ping requests on `DC01`, I opened PowerShell as administrator and ran:

```powershell
New-NetFirewallRule -DisplayName "Laboratorio 1 Permitir Ping IPv4" -Direction Inbound -Protocol ICMPv4 -IcmpType 8 -Action Allow
```

The command output showed that the rule was created and that its status was `Enabled`.

This rule allows inbound ICMPv4 type 8 messages (Echo Request), which are used by the `ping` command.

**Evidence:**

<img src="prints/21-regra-firewall.png" width="600" alt="Creating the ICMPv4 firewall rule">

### 6.3. Repeating the tests

After creating the rule, I repeated the tests in both directions:

- On `DC02`, I ran `ping 10.10.10.10`. Four replies were received with no packet loss.
- On `DC01`, I ran `ping 10.10.10.11`. Four replies were received with no packet loss.

The results confirm IPv4 communication between the two servers in both directions.

**Evidence:**

<img src="prints/22-conexao-maquinas.png" width="600" alt="Successful ping from DC02 to DC01">
<img src="prints/23-ping-inverso-maquinas.png" width="600" alt="Successful ping from DC01 to DC02">

## 7. Configuration summary

| Parameter | DC01 | DC02 |
|---|---|---|
| VirtualBox VM name | `VM-DC01` | `VM-DC02` |
| Computer name | `DC01` | `DC02` |
| IPv4 | `10.10.10.10` | `10.10.10.11` |
| Subnet mask | `255.255.255.0` | `255.255.255.0` |
| Gateway | `10.10.10.1` | `10.10.10.1` |
| Initial preferred DNS | `1.1.1.1` | `1.1.1.1` |
| Initial alternate DNS | `8.8.8.8` | `8.8.8.8` |
| Virtual network | `LAB-AD` | `LAB-AD` |
| RAM | 4096 MB | 4096 MB |
| Virtual CPUs | 2 | 2 |
| Virtual disk | 60 GB, dynamic VDI | 60 GB, dynamic VDI |

## 8. Final state and conclusion

At the end of this stage, both virtual machines have Windows Server 2022 installed, computer names set, and static IPv4 addresses configured on the `LAB-AD` network. The recorded tests confirm Internet access, external DNS resolution, and communication between `DC01` and `DC02` in both directions, after allowing inbound ICMPv4 on `DC01`.

The Lab 1 environment setup is complete, as shown by the evidence collected. Promoting `DC01` to Domain Controller, creating the forest/domain and joining `DC02` to the domain were not done at this stage, as they are part of Lab 2.

## 9. Evidence organization

The screenshots referenced in this document are stored in the `prints` folder, next to this file:

```text

windows-server-ad-lab/
├── README.md           # Home page (English)
├── README.pt.md        # Home page (Portuguese)
├── lab-01.en.md        # Lab 1 (English)
├── lab-01.pt.md        # Lab 1 (Portuguese)
└── prints/             # Screenshots used as evidence
```
