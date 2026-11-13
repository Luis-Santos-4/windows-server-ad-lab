# Windows Server Ad LAB

Repositório de documentação técnica dos laboratórios de Administração de Sistemas, com foco na preparação de ambientes virtuais, Windows Server, redes e serviços de infraestrutura.

## Laboratórios

### Laboratório 1 — Preparação do Ambiente Laboratorial

Preparação de um ambiente de virtualização com Oracle VirtualBox e Windows Server 2022.

**Trabalho realizado:**
- Criação da rede virtual NAT Network `LAB-AD`.
- Criação e instalação das máquinas virtuais `VM-DC01` e `VM-DC02`.
- Configuração dos nomes dos computadores e dos endereços IPv4 estáticos.
- Testes de conectividade à Internet e resolução DNS externa.
- Configuração de uma regra de firewall para permitir ping IPv4.
- Testes de comunicação entre os dois servidores.

**Documentação:** [Laboratório 1 — Preparação do Ambiente Laboratorial](laboratorio-01.md)

**Evidências:** [Pasta de capturas de ecrã](prints/)

## Tecnologias utilizadas

- Oracle VirtualBox
- Windows Server 2022 Standard Evaluation
- Windows PowerShell
- IPv4
- NAT Network

## Organização do repositório

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

## Nota

A documentação descreve os procedimentos executados e os resultados registados nas respetivas evidências. Os laboratórios seguintes serão documentados à medida que forem realizados e validados.
