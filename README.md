# Windows Server Ad LAB

Technical documentation for my Systems Administration labs, focused on setting up virtual environments, Windows Server, networking and infrastructure services.

## Labs

### Lab 1 — Lab Environment Setup

Setting up a virtualization environment with Oracle VirtualBox and Windows Server 2022.

**Work done:**
- Created the `LAB-AD` NAT Network.
- Created and installed the `VM-DC01` and `VM-DC02` virtual machines.
- Configured computer names and static IPv4 addresses.
- Tested Internet connectivity and external DNS resolution.
- Configured a firewall rule to allow IPv4 ping.
- Tested communication between the two servers.

**Documentation:** [Lab 1 — Lab Environment Setup](lab-01.md)

**Evidence:** [Screenshots folder](prints/)

### Lab 2 — Active Directory *(coming soon)*

Promoting `DC01` to Domain Controller, creating the forest and domain, and joining `DC02` to the domain.

## Technologies used

- Oracle VirtualBox
- Windows Server 2022 Standard Evaluation
- Windows PowerShell
- IPv4
- NAT Network

## Repository structure

```text
administracao-sistemas/
├── README.md
├── README.pt.md
├── laboratorio-01.md
└── prints/
    ├── 01-virtualbox.png
    ├── 02-criacao-vm.png
    └── ...
```

## Note

This documentation describes the procedures carried out and the results recorded in the corresponding evidence. The following labs will be documented as they are completed and validated.
