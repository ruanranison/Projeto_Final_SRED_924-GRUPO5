# Roteiro da Rede Virtualizada 
## Descrição

* Para a realização deste roteiro foi criado um ambiente de rede, no qual deve conter 6 máquinas virtuais com o S.O. Ubuntu Server.

* Para a nossa rede virtualizada precisamos de algumas configurações estáticas ao decorrer do roteiro, que se encontra [aqui](https://github.com/ruanranison/GRUPO5-SRED-924/blob/main/Configura%C3%A7%C3%A3o%20Base%20das%20VMs.md).

## Objetivo:

   * Configurar um servidor compartilhamento de arquivos usando o serviço Samba no linux
   * Acessar o **Gateway Server** via Putty no Windows e depois acessar os servidores **samba**.

# Passo a passo

Para que seja possível uma conexão entre as VMs e os PCs da nossa rede virtualizada, será necessário algumas configurações de acesso, configuração estática de IPs, etc.

* [Planilha de Acompanhamento da 924 (Grupo 5)](https://docs.google.com/spreadsheets/d/1pbw24Sg2nh0gQRG1wxN9MRf_ZY9LmZ3iT0STNHoUVq8/edit#gid=680415071)
* [Usuários e Senhas das VMs](https://drive.google.com/file/d/1MdV-bKWlw6sobG24lDvdSd3cdz1gXh2w/view)

## Instalação do SAMBA

Primeiramente vamos acessar a VM própria para o SAMBA. 

```
  su redes
```
> senha: admin@Lab92

Logo após vamos instalar as configurações de VPN através do OpenVPN3.

#### Pré-Requisitos para baixar o OpenVPN3

```bash
sudo apt install apt-transport-https -y
sudo apt install curl -y
sudo apt install gpg -y
```

```bash
curl -fsSL https://swupdate.openvpn.net/repos/openvpn-repo-pkg-key.pub | gpg --dearmor > ~/openvpn-repo-pkg-keyring.gpg
sudo mv openvpn-repo-pkg-keyring.gpg /etc/apt/trusted.gpg.d/openvpn-repo-pkg-keyring.gpg

curl -fsSL https://swupdate.openvpn.net/community/openvpn3/repos/openvpn3-focal.list > ~/openvpn3.list
sudo mv openvpn3.list /etc/apt/sources.list.d/openvpn3.list
sudo apt update
```

#### Instalar o OpenVPN3 

```bash
sudo apt install openvpn3
sudo apt install kmod-ovpn-dco
```

#### Importar o certificado para o OpenVPN 

* Baixe o arquivo com extensão ``.ovpn`` disponível no classroom
* Ou faça o download direto pelo terminal.

```bash
curl -fsSL https://www.dropbox.com/s/hb8ee3kiwkhutbl/vpn924.labredes.arapiraca.ifal.edu.br.ovpn?dl=0 > ~/vpn924.labredes.arapiraca.ifal.edu.br.ovpn
```


* ``CONFIG_FILE`` = arquivo de configuração .ovpn
* ``CONFIG_NAME``= nome da configuração

```bash
openvpn3 config-import --config CONFIG_FILE --name CONFIG_NAME --persistent
openvpn3 config-manage --show --config CONFIG_NAME --dco true
```
* exemplo
```bash
openvpn3 config-import --config vpn924.labredes.arapiraca.ifal.edu.br.ovpn --name vpn924.labredes --persistent
openvpn3 config-manage --show --config vpn924.labredes --dco true
```

### Manipulando os perfis de configração da VPN

#### Listando as os perfis de configuração instalados
```bash
openvpn3 configs-list
```

### Manipulando a conexão VPN

#### Listar as conexões abertas
```bash
openvpn3 sessions-list
```

#### Iniciar a conexão
```bash
openvpn3 session-start --config CONFIG_NAME
```
* Exemplo
```bash
openvpn3 session-start --config vpn924.labredes
```

### Acessar a VM do SAMBA

```bash
ssh administrador@10.9.2.123
```
> usuário: ruanranison.silva

> senha: 2019308308

### Instalando o SAMBA
```bash
sudo apt update
sudo apt install samba
```
### Nome da máquina

Conferir os nomes das MV conforme a [Planilha de Acompanhamento da 924 (Grupo 5)](https://docs.google.com/spreadsheets/d/1pbw24Sg2nh0gQRG1wxN9MRf_ZY9LmZ3iT0STNHoUVq8/edit#gid=680415071). Editar o nome da máquina

```bash
$ sudo hostnamectl set-hostname samba-srv
```
OBS: após o reboot o nome da máquina aparecerá no prompt do shell

```
Tabela 1: Definições da rede interna da turma 924
--------------------------------
|  DESCRICAO  |  IP            |
--------------------------------
| rede        | 10.9.24.0      |
| máscara     | 255.255.255.0  |
| Gateway     | 10.9.24.1      |
| Samba-SRV   | 10.9.24.5      |
| NameServer1 | 10.9.24.10     |
| NameServer2 | 10.9.24.11     |
--------------------------------
```
### Definir o IP da rede interna para o Samba-SRV

```bash
$ sudo nano /etc/netplan/00-installer-config.yaml
```

```
network:
    ethernets:
        enp0s3:
            addresses: [10.9.24.5/24]
            gateway4: 10.9.24.1
            dhcp4: false 
    version: 2
```

```bash
$ sudo netplan apply
$ ifconfig -a
$ ping 10.9.24.1
```











## Testes

* Aqui estão os testes realizados com [`ping`](https://github.com/ruanranison/GRUPO5-SRED-924/blob/main/testes_ping.md) e [`SSH`](https://github.com/ruanranison/GRUPO5-SRED-924/blob/main/testes_ssh.md) para todos os IPs, hostnames, FQDNs e aliases da rede.
