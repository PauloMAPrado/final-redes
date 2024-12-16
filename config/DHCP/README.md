<h3>DHCP</h3>
<hr>
<h4>> O que é DHCP?</h4>
<p>
  O DHCP (Dynamic Host Configuration Protocol) é um protocolo que opera nas camadas 2 e 3 do modelo OSI. Ele é amplamente utilizado para fornecer configuração IP dinâmica a hosts não configurados, proporcionando flexibilidade aos administradores de sistemas.
</p>
<br>
<hr>
<h4>> Instalação</h4>

```bash
#Atualizando a máquina
apt-get update
#Instalando o serviço DHCP
apt-get install -y isc-dhcp-server
#Renomeando o arquivo .conf
mv /etc/dhcp/dhcpd.conf /etc/dhcp/dhcpd.conf.bkp
#Copiando o arquivo de configuração
cp /vagrant_config/DHCP/dhcpd.conf /etc/dhcp/dhcpd.conf
#Reiniciando o serviço
service isc-dhcp-server restart
```



<br>
<hr>
<h4>Código dhcp.conf</h4>

```bash
ddns-update-style none;
subnet 192.168.21.0 netmask 255.255.255.0
{
range 192.168.21.10 192.168.21.200;
option subnet-mask 255.255.255.0;
option domain-name "iftm.edu.br";
option domain-name-servers 8.8.8.8, 8.8.4.4;
option routers 192.168.X.254;
default-lease-time 600;
max-lease-time 7200;
}
```
