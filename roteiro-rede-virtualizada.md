# Roteiro da Rede Virtualizada 
## Descrição

* Para a realização deste roteiro foi criado um ambiente de rede, no qual deve conter 6 máquinas virtuais com o S.O. Ubuntu Server.

* Para a nossa rede virtualizada precisamos de algumas configurações estáticas ao decorrer do roteiro, que se encontra [aqui](https://github.com/ruanranison/GRUPO5-SRED-924/blob/main/Configura%C3%A7%C3%A3o%20Base%20das%20VMs.md).

## Objetivo:

   * Configurar um servidor compartilhamento de arquivos usando o serviço Samba no linux
   * Acessar o **Gateway Server** via Putty no Windows e depois acessar os servidores **samba**.

# Passo a passo

Para que seja possível uma conexão entre as VMs e os PCs da nossa rede virtualizada, será necessário algumas configurações de acesso, configuração estática de IPs, etc.

* [Planilha de Acompanhamento da 924 (Grupo 5)](https://github.com/ruanranison/Projeto_Final_SRED_924-GRUPO5/blob/main/planilha.md)
* [Usuários e Senhas das VMs](https://drive.google.com/file/d/1MdV-bKWlw6sobG24lDvdSd3cdz1gXh2w/view)
* [Instalação do SAMBA](https://github.com/ruanranison/Projeto_Final_SRED_924-GRUPO5/blob/main/samba.md)
* [Instalação do Gateway](https://github.com/ruanranison/Projeto_Final_SRED_924-GRUPO5/blob/main/gateway.md)
* [Instalação do NameServer1](https://github.com/ruanranison/Projeto_Final_SRED_924-GRUPO5/blob/main/dns-master.md)
* [Instalação do NameServer2](https://github.com/ruanranison/Projeto_Final_SRED_924-GRUPO5/blob/main/dns-slave.md)


## Testes

* Aqui estão os testes realizados com [`ping`](https://github.com/ruanranison/GRUPO5-SRED-924/blob/main/testes_ping.md) e [`SSH`](https://github.com/ruanranison/GRUPO5-SRED-924/blob/main/testes_ssh.md) para todos os IPs, hostnames, FQDNs e aliases da rede.
