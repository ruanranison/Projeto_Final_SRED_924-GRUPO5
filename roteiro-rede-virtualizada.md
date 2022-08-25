# Roteiro da Rede Virtualizada 
### Grupo 5

## Descrição

* Para a realização deste roteiro foi criado um ambiente de rede, no qual deve conter 8 máquinas virtuais com o S.O. Ubuntu Server, seguindo a seguinte topologia:

## Topologia da Rede Virtualizada

![Diagrama sem nome drawio (1)](https://user-images.githubusercontent.com/86027160/186276175-ee9d8a35-d38d-49ca-9bc6-833ac9aa0f45.png)

> **Imagem feita a partir do draw.io**

Basicamente, a rede vai possuir 4 PCs conectados entre si a partir dos cabos ethernet e um switch físico, com suas respectivas configurações de rede. Dentro de cada PC será criado 2 VMs, onde todas as VMs criadas poderão se conectar através dos switches virtuais das VMs e do switch físico com seus cabos de rede (ethernet). Sendo assim será possível conectar-se entre elas. No nosso exemplo, fizemos login em usuários criados nestas VMs para evidenciar essa conexão. 

* Para a nossa rede virtualizada precisamos de algumas configurações estáticas ao decorrer do roteiro, que se encontra [aqui](https://github.com/ruanranison/GRUPO5-SRED-924/edit/main/Configura%C3%A7%C3%A3o%20Base%20das%20VMs.md).


# Passo a passo

Para que seja possível uma conexão entre as VMs e os PCs da nossa rede virtualizada, será necessário algumas configurações de acesso, configuração estática de IPs, etc.

> ### Faça o passo a passo em todas as VMs da rede!

## Criação das VMs da rede

Para a criação das VMs, vamos importar a imagem .OVA, ele será necessário para a construção da rede.

* .OVA é um pacote que contém arquivos usados para descrever uma máquina virtual, que inclui um arquivo descritor .OVF, manifesto opcional (.MF) e arquivos de certificado e outros arquivos relacionados. 

### Segue os passos de importação na Figura 1 e 2:

![image](https://user-images.githubusercontent.com/86027160/183978615-8366e422-6321-4bcb-b040-48e634fb4740.png)

![image](https://user-images.githubusercontent.com/86027160/183981533-6beca288-3e86-40e2-9c9d-94928950bb6d.png)

* Um ponto a ressaltar é na `configuração de "Política de Endereço MAC"`, onde é preciso que deixe como `Gerar novos endereços MAC para todas as placas de rede` para que para cada interface de rede seja criado um novo endereço MAC.

Após a criação das duas VMs, é obrigatório os seguintes passos em CADA Máquina Virtual (são 2 VMs para cada PC da rede):

### Iniciar as VMs

![image](https://user-images.githubusercontent.com/86027160/184192797-b0d35e24-68cd-4e50-95e1-9343476c68bc.png)


### Fazer login nas VMs

* Usuário da VM: ``administrador``
* Senha da VM: ``adminifal``

![image](https://user-images.githubusercontent.com/86027160/184193472-26d4289b-9dc4-47f5-9bbc-a35d6965760f.png)


### Instalando o pacote de rede

  ```
  sudo apt install net-tools -y
  ```
### Visualizar as interfaces de rede

```
ifconfig -a
```
![image](https://user-images.githubusercontent.com/86027160/184196077-eeee0a97-054e-4640-9d8c-703c564385dc.png)



### Configurando as NICs das VMs
* Para que a VMs utilizem a mesma rede interna é necessário acessar as configurações de Rede de cada VM e selecionar o modo ``rede interna`` e definir o nome da rede, vamos escolher ``grupo5`` como nome da nossa rede virtual. Utilize o mesmo nome nas duas VMs.

![image](https://user-images.githubusercontent.com/86027160/183985883-c01e95f9-bdf2-49a7-8992-9568d17daf51.png)
## Definições 

### Inserindo um nome para a rede ``hostname``

  ```
  sudo hostnamectl set-hostname nome-do-hostname
  ```

### Configuração estática de endereço IP na interface de rede
* Primeiramente é necessário modificar o arquivo que configura as interfaces de rede
* Esse arquivo é do tipo .YAML e fica no diretório ``/etc/netplan``
* Inicialmente, verifique o nome do arquivo em seu computador da seguinte forma:

  ```
  ls -la /etc/netplan
  ```
  
## Editando o .YAML
  
  ```
  sudo nano /etc/netplan/01-netcfg.yaml 
  ```

## Configuração dos endereços IPs
Após ter aberto o arquivo `.yaml` para edição, altere as configurações antigas para as referente a [`Tabela 1`](https://github.com/ruanranison/GRUPO5-SRED-924/blob/main/Configura%C3%A7%C3%A3o%20Base%20das%20VMs.md). Essas modificações deverão ser feitas em todas as VMs. Exemplo:

![image](https://user-images.githubusercontent.com/86027160/184198770-b253b52f-2d80-4292-b9f4-b92b4b5abb47.png)

* enps0s3: nome da interface que está sendo configurada. Verifique com o comando 'ifconfig -a'
* adresses: IP e Máscara do Host.
* gateaway: IP do Gateway  
* dhcp4 false -> cliente DHCP está desabilitado, logo o utilizará o IP do campo 'addresses'

### Aplicar as configurações
```
sudo netplan apply
```

## Servidor SSH

### Pré-Requisitos

* Mudar o tipo da rede para `NAT`
![image](https://user-images.githubusercontent.com/86027160/184208697-62d1f91d-6d32-4f2d-8f30-f021f2c00587.png)

* Comentar as linhas do endereço IP estático. (comenta usando `#`)
* Ativar o DHCP. (tornar ele como `true` ou `yes`)

![image](https://user-images.githubusercontent.com/86027160/184213039-2e35a024-b710-4560-be9d-5f63584029b4.png)

* Para atualizar as definições e versões de pacotes/bibliotecas dos repositórios do Ubuntu
```
sudo apt update
```
* Para atualizar os pacotes com as novas definições setadas com o update
```
sudo apt upgrade -y
```
![image](https://user-images.githubusercontent.com/86027160/184215812-9018c0cb-ea5c-4eb2-973f-e3a52ce3180b.png)

### Instalando o SSH <br>
```
sudo apt-get install openssh-server
```
* Verifique se o SSH foi instalado corretamente: 
  ```
  systemctl status ssh
  ```
* Verifique o status das portas do sistema:
  ```
  netstat -an | grep LISTEN. # verifique se a porta 22 está LISTENING
  ```
* Para garantir o funcionamento do SSH Server, é necessário habilita-lo no firewall. Faça-o: 
  ```
  sudo ufw allow ssh
  ```
* Ative o firewall: 
  ```
  sudo ufw enable
  ```
#### Após ter concluído o processo, volte para as configurações anteriores:
* Coloque a configuração de rede da VM como ``Modo Bridge``
![image](https://user-images.githubusercontent.com/86027160/184216647-8294ad5e-4325-4c9a-80d0-5580fb704f22.png)
* Tire os comentários do arquivo .YAML

## Configurações de Host-Only
* Configurações do adaptador1 do Host-Only
![image](https://user-images.githubusercontent.com/86027160/184218352-c7206bf4-12a6-474f-83ca-413e87c8b802.png)

* Configurações do adaptador2 do host-only
![image](https://user-images.githubusercontent.com/86027160/184218933-2f0a2180-9f4a-493a-b946-ae4e7d225ca8.png)

* Definindo o Host-Only

![image](https://user-images.githubusercontent.com/86027160/184417463-d30da1eb-18cb-4969-b9ef-694061d04df4.png)

* Definindo usuários (criar um usuário com um nome distinto para as duas VMs de cada PC)

![image](https://user-images.githubusercontent.com/86027160/184418883-41d9d7a0-fb5e-41ae-8163-54953dbb7686.png)


## Configuração estática dos nomes
* Edite o arquivo /etc/hosts conforme as definições da [Tabela 2](https://github.com/ruanranison/GRUPO5-SRED-924/blob/main/Configura%C3%A7%C3%A3o%20Base%20das%20VMs.md) a seguir:                                                                                                                                                         
```
sudo nano /etc/hosts
```

* Exemplo de Arquivo /etc/hosts:

```
127.0.0.1 localhost
127.0.1.1 vm1-pc1   #Nome da VM 
192.168.24.67 vm1-pc1 santos.grupo5-924.ifalara.net san
192.168.24.69 vm2-pc1 coringao.grupo5-924.ifalara.net cor
192.168.24.68 vm1-pc2 trikas.grupo5-924.ifalara.net tri
192.168.24.70 vm2-pc2 vascao.grupo5-924.ifalara.net vas
192.168.24.71 vm1-pc3 feto.grupo5-924.ifalara.net feto
192.168.24.73 vm2-pc3 marcao.grupo5-924.ifalara.net mar
192.168.24.72 vm1-pc4 felippo.grupo5-924.ifalara.net flpp
192.168.24.74 vm2-pc4 juarez.grupo5-924.ifalara.net jua

# The following lines are desirable for IPv6 capable hosts
::1     localhost ip6-localhost ip6-loopback
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
```

> **Fazer todos os passos do roteiro em todas as VMS!!!**

# Últimos passos: 

## Conexão dos cabos ethernet nos PCs utilizados 
Com o auxílio de um switch, conectamos todos os computadores entre si através do cabo ethernet, como mostrado nas respectivas imagens:

![20220812_170930-min](https://user-images.githubusercontent.com/86027160/186281131-6da9185c-fede-461a-a395-4ec751ff53b1.jpg)
![20220812_170937-min](https://user-images.githubusercontent.com/86027160/186281185-534d8cfb-ebcb-4009-8258-a9c0ec2b2bda.jpg)
![20220812_171012-min](https://user-images.githubusercontent.com/86027160/186281163-2322ff51-0b1e-441c-badf-13bfb6b6f010.jpg)

Logo após isso, já será possível conectar qualquer VM a outra dentro da rede.  

## "Pingar" para outra máquina virtual:

```
ping ipderedeunico 
```

Exemplo:
```
ping 192.168.24.70
```

## Logar em outra máquina virtual:
```
ssh usuario@ipderedeunico
```
Exemplo: 
```
ssh administrador@192.168.24.70
```

## Testes

* Aqui estão os testes realizados com [`ping`](https://github.com/ruanranison/GRUPO5-SRED-924/blob/main/testes_ping.md) e [`SSH`](https://github.com/ruanranison/GRUPO5-SRED-924/blob/main/testes_ssh.md) para todos os IPs, hostnames, FQDNs e aliases da rede.
