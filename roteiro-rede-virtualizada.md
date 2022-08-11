## Roteiro da Rede Virtualizada 
### Grupo 5

Para a nossa rede virtualizada. teremos a descrição das VMs, os IPs, hostnames, FQDNs e aliases listados abaixo.

* IP: nosso número de identificação única para cada máquina utilizada.
* hostname: nome da nossa máquina onde está vinculada ao IP.
* FQDN: endereço estático associado ao IP.
* aliases: nome atribuído ao IP.
 
## Configuração do serviço de nomes estáticos.

```
Tabela1: Definições de endereços IPs da Rede e Nomes de Hosts
-------------------------------------------------------------------------------------------------
|  DESCRICAO  |       IP        |      hostname     |          FQDN                 |   aliase  |
-------------------------------------------------------------------------------------------------
| VM1-PC1     | 192.168.24.67   |   grupo5-vm1-pc1  |santos.grupo5-924.ifalara.net  |    san    |
| VM2-PC1     | 192.168.24.69   |   grupo5-vm2-pc1  |coringao.grupo5-924.ifalara.net|    cor    |
| VM1-PC2     | 192.168.24.68   |   grupo5-vm1-pc2  |trikas.grupo5-924.ifalara.net  |    tri    |
| VM2-PC2     | 192.168.24.70   |   grupo5-vm2-pc2  |vascao.grupo5-924.ifalara.net  |    vas    |
| VM1-PC3     | 192.168.24.71   |   grupo5-vm1-pc3  |feto.grupo5-924.ifalara.net    |    feto   |
| VM2-PC3     | 192.168.24.73   |   grupo5-vm2-pc3  |marcao.grupo5-924.ifalara.net  |    mar    |
| VM1-PC4     | 192.168.24.72   |   grupo5-vm1-pc4  |felippo.grupo5-924.ifalara.net |    flpp   |
| VM2-PC4     | 192.168.24.74   |   grupo5-vm2-pc4  |juarez.grupo5-924.ifalara.net  |    jua    |
-------------------------------------------------------------------------------------------------
```



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
```
Tabela 2: Definições de endereços IPs da Rede 
---------------------------------
|  DESCRICAO  |       IP        |
---------------------------------
| rede        | 192.168.24.64   |
| máscara     | 255.255.255.240 |
| Gateway     | 192.168.24.65   |
| VM1-PC1     | 192.168.24.67   | 
| VM2-PC1     | 192.168.24.69   |
| VM1-PC2     | 192.168.24.68   |
| VM2-PC2     | 192.168.24.70   |
| VM1-PC3     | 192.168.24.71   | 
| VM2-PC3     | 192.168.24.73   |
| VM1-PC4     | 192.168.24.72   |
| VM2-PC4     | 192.168.24.74   |
---------------------------------
```
Após ter aberto o arquivo `.yaml` para edição, altere as configurações antigas para as referente a `Tabela 2`. Essas modificações deverão ser feitas em todas as VMs. Exemplo:

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

![image](https://user-images.githubusercontent.com/86027160/184209443-1bf0ba2c-65af-4cb6-a855-e0c4e437996c.png)

### PC1
* VM1-PC1
* VM2-PC1

### PC2
* VM1-PC2
* VM2-PC2

### PC3
* VM1-PC3
* VM2-PC3

### PC4
* VM1-PC4
* VM2-PC4






  ## Começando a instalar o ``SSH Server``
  
   1. Pré-requisitos: <br>
       1.1. Alterar na configuração da máquina a configuração de rede para ``NAT``. <br>
       1.2. Comentar as linhas do endereço IP estático.(No arquivo .YAML) <br>
       1.3. Ativar o DHCP. (No arquivo .YAML) <br>

       ![dhcp-false](https://user-images.githubusercontent.com/84058517/183922957-7aca9c24-ab96-416d-b34b-b518e8df316d.png)

   2. Instalando o SSH Server <br>
   * ### Certifique-se que a Máquina Virtual está conectada a internet <br>
       * Para atualizar as definições e versões de pacotes/bibliotecas dos repositórios do Ubuntu <br>
       ```
       sudo apt update
       ```
       * Para atualizar os pacotes com as novas definições setadas com o update <br>
       ```
       sudo apt upgrade -y
       ```
   * ### Instalando o SSH <br>
      ```
      sudo apt-get install openssh-server
      ```
    
       #### Após ter feito o processo inteiro, sem nenhum empecilho, prossiga:
       
       * Verifique se o ssh foi instalado corretamente: 

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
       
       ![pipipopo](https://user-images.githubusercontent.com/84058517/183930270-52ff96ea-7539-4d49-aa3d-fd17cc12e59b.png)
       * Tire os comentários do arquivo .YAML
       
       ![semcomentario](https://user-images.githubusercontent.com/84058517/183934739-31b1c0b8-7db6-4078-b6fb-efb875c21964.png)

       # Servidor concluído! Ultimos passos: 
       * Logar em outra máquina virtual(pode ser até em outro PC caso os dois ou mais estejam conectados via cabeamento):
       ```
       ssh usuario@ipderedeunico
       ```
       Exemplo: 
       ```
       ssh administrador@192.168.14.53
       ```
       
       Após isso, está pronto o funcionamento do servidor
