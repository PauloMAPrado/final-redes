<h3>DNS</h3>
<hr>
<h4>> O que é DNS?</h4>
<p>
  O DNS (Domain Name Server) é um dos principais serviços de rede TCP/IP. Sua função é traduzir nomes de domínio, como (link unavailable), em endereços IP, como 200.242.140.10. Além disso, o DNS realiza a resolução reversa, convertendo endereços IP em nomes correspondentes.
</p>
<br>
<hr>
<h4>> Instalação</h4>


## Passo 1: Instalar o DNS
1. **Atualize os pacotes do sistema:**
   ```bash
   sudo apt-get update
   ```

2. **Instale o DNS:**
   ```bash
   sudo apt install bind9

   ```

3. **Verifique o status do bind**
   ```bash
   service bind9 status
   ```

---

## Passo 2: Configurar o servidor DNS com o bind
1. **Edite o arquivo de configuração do BIND: Abra o arquivo de configuração principal do BIND:**
   ```bash
   sudo nano /etc/bind/named.conf
   ```

2. **Adicione a zona DNS para o seu domínio:**
   ```bash
   zone "exemplo.local" {
    type master;
    file "/etc/bind/db.exemplo.local";
};
   ```

3. **Crie o arquivo de zona:**
   ```bash
   sudo nano /etc/bind/db.meudominio.local
   ```

4. **Adicione as configurações da zona DNS: Exemplo de configuração para o arquivo db.exemplo.local:**
   ```bash
   $TTL    604800
@       IN      SOA     ns.exemplo.com. root.exemplo.com. (
                        2         ; Serial
                        604800    ; Refresh
                        86400     ; Retry
                        2419200   ; Expire
                        604800 )  ; Negative Cache TTL
;
@       IN      NS      ns.exemplo.com.
@       IN      A       192.168.1.100
ns      IN      A       192.168.1.100
www     IN      A       192.168.1.101
ftp     IN      A       192.168.1.102
   ```



<br>
<hr>
<h4>> Código named.conf.local</h4>

```bash
zone "exemplo.com" {
    type master;
    file "/etc/bind/db.exemplo.com";
};
```

<br>
<hr>
<h4>> Código named.conf.options</h4>

```bash
options {
    directory "/var/cache/bind";

    // Escutar em todas as interfaces
    listen-on {any;};
    listen-on-v6 {any;};

    // Permitir consultas de qualquer lugar
    allow-query {any;};

    // Encaminhadores (opcional, pode usar DNS públicos)
    forwarders {
        8.8.8.8;
        8.8.4.4;
    };

    dnssec-validation auto;

    auth-nxdomain no;
    listen-on {any;};
};
```

<br>
<h4>> Código db.exemplo.com</h4>

```bash
$TTL    604800
@       IN      SOA     ns.exemplo.com. root.exemplo.com. (
                        2         ; Serial
                        604800    ; Refresh
                        86400     ; Retry
                        2419200   ; Expire
                        604800 )  ; Negative Cache TTL
;
@       IN      NS      ns.exemplo.com.
@       IN      A       192.168.1.100
ns      IN      A       192.168.1.100
www     IN      A       192.168.1.101
ftp     IN      A       192.168.1.102
```

<br>
<h4>> Código db.192.168.1</h4>

```bash
$TTL    604800
@       IN      SOA     ns.exemplo.com. root.exemplo.com. (
                        1         ; Serial
                        604800    ; Refresh
                        86400     ; Retry
                        2419200   ; Expire
                        604800 )  ; Negative Cache TTL
;
@       IN      NS      ns.exemplo.com.
100     IN      PTR     ns.exemplo.com.
101     IN      PTR     www.exemplo.com.
102     IN      PTR     ftp.exemplo.com.
```
