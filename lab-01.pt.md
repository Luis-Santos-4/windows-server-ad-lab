# Laboratório 1 — Preparação do Ambiente Laboratorial

[English](lab-01.en.md) | **Português**

## 1. Objetivo

Neste laboratório preparei um ambiente de virtualização para trabalhar com Windows Server 2022. O objetivo foi criar duas máquinas virtuais, colocá-las na mesma rede virtual, configurar endereços IPv4 estáticos e confirmar que os servidores conseguem comunicar entre si e aceder a serviços externos.

A promoção de um servidor a Domain Controller e a criação da floresta, árvore e domínio ficam para o Laboratório 2.

## 2. Tecnologias utilizadas

- Oracle VirtualBox
- Windows Server 2022 Standard Evaluation
- Rede NAT Network do VirtualBox
- IPv4
- Windows PowerShell

## 3. Configuração da rede virtual

No VirtualBox criei a rede NAT Network `LAB-AD`. Escolhi uma rede dedicada ao laboratório para permitir a comunicação entre as máquinas virtuais e o acesso à Internet, sem as ligar diretamente à rede física. O DHCP ficou desativado porque os endereços IPv4 dos servidores foram configurados manualmente.

| Parâmetro | Configuração |
|---|---|
| Nome da rede | `LAB-AD` |
| Tipo | NAT Network |
| Prefixo IPv4 | `10.10.10.0/24` |
| DHCP | Desativado |
| IPv6 | Desativado |

**Evidência:**

<img src="prints/08-rede-lab.png" width="600" alt="Configuração da rede NAT Network LAB-AD">

## 4. Primeiro servidor — DC01

### 4.1. Criação da máquina virtual

Criei a máquina virtual `VM-DC01` no VirtualBox e associei-lhe a ISO do Windows Server 2022. A instalação foi feita manualmente, sem a opção de instalação não assistida, para seguir as etapas do assistente de instalação.

| Parâmetro | Configuração |
|---|---|
| Nome da máquina virtual | `VM-DC01` |
| Sistema operativo | Windows Server 2022 Standard Evaluation |
| Interface | Desktop Experience |
| Processadores virtuais | 2 |
| Memória RAM | 4096 MB |
| Disco virtual | 60 GB |
| Formato do disco | VDI |
| Alocação do disco | Dinâmica |
| Rede | `LAB-AD` |

**Evidências:**

<img src="prints/01-virtualbox.png" width="600" alt="Oracle VirtualBox">
<img src="prints/02-criacao-vm.png" width="600" alt="Criação da máquina virtual VM-DC01">
<img src="prints/03-disco-virtual.png" width="600" alt="Configuração do disco virtual da VM-DC01">
<img src="prints/04-edicao-windows-server.png" width="600" alt="Seleção da edição do Windows Server 2022">

### 4.2. Instalação e acesso inicial

Durante a instalação selecionei **Windows Server 2022 Standard Evaluation (Desktop Experience)**. Esta opção inclui a interface gráfica, necessária para acompanhar as tarefas do laboratório através das ferramentas do Windows Server.

Defini uma palavra-passe para a conta local `Administrator` e, depois de a instalação terminar, iniciei sessão no servidor.

**Evidências:**

<img src="prints/05-configuracao-administrador.png" width="600" alt="Definição da palavra-passe do Administrator">
<img src="prints/06-windows-server-instalado.png" width="600" alt="Windows Server 2022 instalado">

### 4.3. Nome do computador

Depois da instalação, alterei o nome do computador para `DC01`. Confirmei o nome através do comando:

```powershell
hostname
```

O resultado apresentado foi `DC01`.

### 4.4. Configuração IPv4

Configurei o adaptador de rede do `DC01` com os seguintes valores:

| Parâmetro | Valor |
|---|---|
| Endereço IPv4 | `10.10.10.10` |
| Máscara de sub-rede | `255.255.255.0` |
| Gateway predefinido | `10.10.10.1` |
| DNS preferencial inicial | `1.1.1.1` |
| DNS alternativo inicial | `8.8.8.8` |

Utilizei o comando seguinte para consultar a configuração completa do adaptador:

```powershell
ipconfig /all
```

Nesta fase, os DNS públicos foram usados para testar a resolução de nomes na Internet. Esta configuração é inicial; será revista quando o serviço DNS interno for configurado nas etapas seguintes do projeto.

**Evidências:**

<img src="prints/07-rede-vm-dc01.png" width="600" alt="Placa de rede da VM-DC01 ligada à LAB-AD">
<img src="prints/09-configuracao-ipv4.png" width="600" alt="Configuração IPv4 do DC01">

### 4.5. Testes de conectividade e DNS

No `DC01`, executei os seguintes comandos:

```powershell
ping 1.1.1.1
nslookup www.microsoft.com
```

O `ping 1.1.1.1` recebeu quatro respostas, sem perda de pacotes. O `nslookup www.microsoft.com` devolveu uma resposta DNS, confirmando que a resolução de nomes externos funcionava.

**Evidências:**

<img src="prints/10-teste-ping.png" width="600" alt="Teste de ping a 1.1.1.1 no DC01">
<img src="prints/11-teste-dns.png" width="600" alt="Teste de nslookup no DC01">

## 5. Segundo servidor — DC02

### 5.1. Criação e instalação

Criei uma segunda máquina virtual com o nome `VM-DC02`, utilizando a mesma ISO do Windows Server 2022 e os mesmos recursos virtuais definidos para a primeira máquina.

| Parâmetro | Configuração |
|---|---|
| Nome da máquina virtual | `VM-DC02` |
| Sistema operativo | Windows Server 2022 Standard Evaluation |
| Interface | Desktop Experience |
| Processadores virtuais | 2 |
| Memória RAM | 4096 MB |
| Disco virtual | 60 GB |
| Formato do disco | VDI |
| Alocação do disco | Dinâmica |
| Rede | `LAB-AD` |

Depois da instalação, alterei o nome do computador para `DC02`.

**Evidências:**

<img src="prints/12-criacao-vm2.png" width="600" alt="Criação da máquina virtual VM-DC02">
<img src="prints/13-memoria-virtual2.png" width="600" alt="Memória da VM-DC02">
<img src="prints/14-disco-virtual2.png" width="600" alt="Disco virtual da VM-DC02">
<img src="prints/15-maquina-virtual2.png" width="600" alt="Resumo da VM-DC02">
<img src="prints/17-troca-nome-dc02.png" width="600" alt="Alteração do nome do computador para DC02">

### 5.2. Configuração IPv4

Configurei o adaptador de rede do `DC02` com o endereço estático seguinte:

| Parâmetro | Valor |
|---|---|
| Endereço IPv4 | `10.10.10.11` |
| Máscara de sub-rede | `255.255.255.0` |
| Gateway predefinido | `10.10.10.1` |
| DNS preferencial inicial | `1.1.1.1` |
| DNS alternativo inicial | `8.8.8.8` |

Verifiquei a configuração com:

```powershell
ipconfig /all
```

**Evidências:**

<img src="prints/16-rede-vm-dc02.png" width="600" alt="Placa de rede da VM-DC02 ligada à LAB-AD">
<img src="prints/18-configurar-ipv4.png" width="600" alt="Configuração IPv4 do DC02">
<img src="prints/19-configuracao-ipv4-dc02.png" width="600" alt="Verificação da configuração IPv4 do DC02">

### 5.3. Testes de conectividade e DNS

No `DC02`, executei:

```powershell
ping 1.1.1.1
nslookup www.microsoft.com
```

O teste de conectividade recebeu quatro respostas, sem perda de pacotes. O `nslookup` também devolveu uma resposta DNS para `www.microsoft.com`.

**Evidência:**

<img src="prints/24-teste-ping-dns.png" width="600" alt="Testes de ping e DNS no DC02">

## 6. Comunicação entre os servidores

Com os dois servidores na rede `LAB-AD`, testei a comunicação através de ICMPv4.

### 6.1. Primeiro teste a partir do DC02

Inicialmente, o comando:

```powershell
ping 10.10.10.10
```

executado no `DC02` falhou, com quatro pedidos expirados e 100% de perda de pacotes.

**Evidência do resultado inicial:**

<img src="prints/20-erro-conexao.png" width="600" alt="Ping do DC02 para o DC01 sem resposta">

### 6.2. Regra de firewall no DC01

Para permitir pedidos de ping IPv4 de entrada no `DC01`, abri o PowerShell como administrador e executei:

```powershell
New-NetFirewallRule -DisplayName "Laboratorio 1 Permitir Ping IPv4" -Direction Inbound -Protocol ICMPv4 -IcmpType 8 -Action Allow
```

A saída do comando indicou que a regra foi criada e que o seu estado era `Enabled`.

Esta regra permite mensagens ICMPv4 do tipo 8 (Echo Request) de entrada, utilizadas pelo comando `ping`.

**Evidência:**

<img src="prints/21-regra-firewall.png" width="600" alt="Criação da regra de firewall para ICMPv4">

### 6.3. Repetição dos testes

Depois de criar a regra, repeti os testes nos dois sentidos:

- No `DC02`, executei `ping 10.10.10.10`. Foram recebidas quatro respostas, sem perda de pacotes.
- No `DC01`, executei `ping 10.10.10.11`. Foram recebidas quatro respostas, sem perda de pacotes.

Os resultados confirmam a comunicação IPv4 entre os dois servidores nos dois sentidos.

**Evidências:**

<img src="prints/22-conexao-maquinas.png" width="600" alt="Ping do DC02 para o DC01 com sucesso">
<img src="prints/23-ping-inverso-maquinas.png" width="600" alt="Ping do DC01 para o DC02 com sucesso">

## 7. Resumo da configuração

| Parâmetro | DC01 | DC02 |
|---|---|---|
| Nome da VM no VirtualBox | `VM-DC01` | `VM-DC02` |
| Nome do computador | `DC01` | `DC02` |
| IPv4 | `10.10.10.10` | `10.10.10.11` |
| Máscara | `255.255.255.0` | `255.255.255.0` |
| Gateway | `10.10.10.1` | `10.10.10.1` |
| DNS preferencial inicial | `1.1.1.1` | `1.1.1.1` |
| DNS alternativo inicial | `8.8.8.8` | `8.8.8.8` |
| Rede virtual | `LAB-AD` | `LAB-AD` |
| RAM | 4096 MB | 4096 MB |
| CPUs virtuais | 2 | 2 |
| Disco virtual | 60 GB, VDI dinâmico | 60 GB, VDI dinâmico |

## 8. Estado final e conclusão

No final desta etapa, as duas máquinas virtuais têm o Windows Server 2022 instalado, nomes de computador definidos e endereços IPv4 estáticos configurados na rede `LAB-AD`. Os testes registados confirmam o acesso à Internet, a resolução DNS externa e a comunicação entre `DC01` e `DC02` nos dois sentidos, depois de permitir ICMPv4 de entrada no `DC01`.

A preparação do ambiente para o Laboratório 1 está concluída de acordo com as evidências reunidas. A promoção do `DC01` a Domain Controller, a criação da floresta/domínio e a integração do `DC02` no domínio não foram realizadas nesta fase, pois pertencem ao Laboratório 2.

## 9. Organização das evidências

As capturas referidas neste documento estão guardadas na pasta `prints`
