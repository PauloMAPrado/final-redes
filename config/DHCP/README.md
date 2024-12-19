<h3>DHCP</h3>
<hr>
<h4>> O que é DHCP?</h4>
<p>
  O DHCP (Dynamic Host Configuration Protocol) é um protocolo que opera nas camadas 2 e 3 do modelo OSI. Ele é amplamente utilizado para fornecer configuração IP dinâmica a hosts não configurados, proporcionando flexibilidade aos administradores de sistemas.
</p>
<br>
<hr>
<h4>> Instalação</h4>

## Passo 1: Instalar o Servidor DHCP
1. **Atualize os pacotes do sistema:**
   ```bash
   sudo apt-get update
   ```

2. **Instale o serviço DHCP:**
   ```bash
   sudo apt-get install isc-dhcp-server
   ```

3. **Abra o arquivo de configuração padrão:**
   ```bash
   sudo nano /etc/dhcp/dhcpd.conf
   ```

---

## Passo 2: Reiniciar o Serviço DHCP
1. **Reinicie o serviço para aplicar as configurações:**
   ```bash
   sudo service isc-dhcp-server restart
   ```

2. **Verifique o status do serviço:**
   ```bash
   sudo service isc-dhcp-server status
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
