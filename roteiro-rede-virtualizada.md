## Roteiro da Rede Virtualizada 
### Grupo 5

Para a nossa rede virtualizada. teremos a descrição das VMs, os IPs, hostnames, FQDNs e aliases listados abaixo.

* IP: nosso número de identificação única para cada máquina utilizada.
* hostname: nome da nossa máquina onde está vinculada ao IP.
* FQDN: endereço estático associado ao IP.
* aliases: nome atribuído ao IP.
 
## Configuração do serviço de nomes estáticos.

```
Definições de endereços IPs da Rede e Nomes de Hosts
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

### Configurando as NICs das VMs
* Para que a VMs utilizem a mesma rede interna é necessário acessar as configurações de Rede de cada VM e selecionar o modo ``rede interna`` e definir o nome da rede, vamos escolher ``grupo5`` como nome da nossa rede virtual. Utilize o mesmo nome nas duas VMs.

![image](https://user-images.githubusercontent.com/86027160/183985883-c01e95f9-bdf2-49a7-8992-9568d17daf51.png)



### Fazendo login nas VMs

* Usuário da VM: ``administrador``
* Senha da VM: ``adminifal``

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
