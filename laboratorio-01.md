# Laboratório 1 — Preparação do Ambiente Laboratorial

## 1. Objetivo

Preparar um ambiente de virtualização com Oracle VirtualBox e Windows Server 2022, criando as bases para uma arquitetura Cliente-Servidor Virtual.

O laboratório tem como objetivos configurar uma rede virtual, criar duas máquinas virtuais e instalar o Windows Server 2022 em ambas, garantindo a conectividade entre os servidores.

## 2. Tecnologias utilizadas

- Oracle VirtualBox
- Windows Server 2022
- Rede NAT (NAT Network)
- IPv4
- Windows PowerShell

## 3. Configuração da rede virtual

Foi preparada uma rede virtual dedicada ao laboratório, denominada `LAB-AD`.

| Parâmetro | Configuração |
|---|---|
| Nome | `LAB-AD` |
| Tipo | NAT Network |
| Rede IPv4 | `10.10.10.0/24` |
| DHCP | Desativado |
| IPv6 | Desativado |

Esta configuração permite preparar as máquinas virtuais para comunicarem entre si e acederem à Internet, mantendo o ambiente laboratorial separado da rede física.

## 4. Preparação do servidor DC01

### 4.1. Máquina virtual

Foi criada a máquina virtual `VM-DC01` para instalar o Windows Server 2022.

| Parâmetro | Configuração |
|---|---|
| Nome da máquina virtual | `VM-DC01` |
| Sistema operativo | Windows Server 2022 |
| Interface | Desktop Experience |
| Processadores virtuais | 2 |
| Memória RAM | 4096 MB |
| Disco virtual | 60 GB |
| Formato do disco | VDI |
| Alocação do disco | Dinâmica |
| Rede virtual | `LAB-AD` |

### 4.2. Instalação do Windows Server 2022

Foi instalado o Windows Server 2022 com interface gráfica na primeira máquina virtual.

Após a instalação, foi configurada a conta administrativa local e efetuado o início de sessão no sistema operativo.

### 4.3. Alteração do nome do servidor

O nome do computador foi alterado para `DC01`.

Para confirmar o nome do computador, foi utilizado o seguinte comando no PowerShell:

```powershell
hostname
```

O resultado obtido foi `DC01`.

### 4.4. Configuração IPv4

Foi configurada a seguinte informação de rede no DC01:

| Parâmetro | Valor |
|---|---|
| Endereço IPv4 | `10.10.10.10` |
| Máscara de sub-rede | `255.255.255.0` |
| Gateway predefinido | `10.10.10.1` |
| DNS preferencial inicial | `1.1.1.1` |
| DNS alternativo inicial | `8.8.8.8` |

A configuração foi consultada através do comando:

```powershell
ipconfig /all
```

Os servidores DNS públicos foram utilizados na configuração inicial, antes da implementação do serviço DNS interno do laboratório.

## 5. Validação

Foram executados testes de conectividade IP e resolução DNS no DC01. Ambos os testes foram concluídos com sucesso.

| Teste | Comando | Resultado |
|---|---|---|
| Conectividade IP com a Internet | `ping 1.1.1.1` | Sucesso — comunicação confirmada |
| Resolução DNS externa | `nslookup www.microsoft.com` | Sucesso — resolução de nomes confirmada |

Os resultados foram confirmados durante a execução dos testes no DC01.

## 6. Estado atual do laboratório

A preparação do primeiro servidor, DC01, encontra-se concluída.

Fica para a próxima fase a criação da segunda máquina virtual, `VM-DC02`, a instalação do Windows Server 2022 e a configuração do respetivo endereço IPv4.

Após a preparação dos dois servidores, será necessário testar a conectividade entre ambos.

A promoção do DC01 a Domain Controller e a criação da floresta, da árvore e do domínio pertencem ao Laboratório 2.

## 7. Evidências

7.1. Criação do ambiente de virtualização

O Oracle VirtualBox foi utilizado para preparar o ambiente de virtualização.



7.2. Criação da máquina virtual

Foi criada a máquina virtual VM-DC01, definindo o sistema operativo, o disco virtual e os restantes parâmetros de configuração.





7.3. Instalação do Windows Server 2022

Durante a instalação, foi selecionada a edição Windows Server 2022 Standard Evaluation com Desktop Experience.



Foi configurada a palavra-passe da conta administrativa local.



Após a instalação, o Windows Server 2022 ficou operacional.



7.4. Configuração da rede virtual

A máquina virtual foi associada à rede NAT LAB-AD, com o prefixo IPv4 10.10.10.0/24 e o servidor DHCP desativado.





7.5. Verificação da configuração IPv4

A configuração de rede do DC01 foi verificada através do comando ipconfig /all.

A captura seguinte apresenta o endereço IPv4 10.10.10.10, a máscara 255.255.255.0, o gateway 10.10.10.1 e os servidores DNS 1.1.1.1 e 8.8.8.8.



7.6. Testes de conectividade e resolução DNS

Foi executado o comando ping 1.1.1.1. Foram recebidas as quatro respostas, sem perda de pacotes.



Foi executado o comando nslookup www.microsoft.com. A resolução DNS foi bem-sucedida e devolveu os registos de endereço correspondentes.