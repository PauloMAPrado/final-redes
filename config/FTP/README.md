<h3> FTP </h3>
<hr>
<h4>> O que é FTP?</h4>
<p>Um servidor FTP (File Transfer Protocol) é uma solução prática para compartilhar arquivos, permitindo armazenamento e acesso facilitado. É ideal para armazenar arquivos destinados à leitura.</p>
<br>
<hr>

<h4>Instalação</h4>

```bash
#Instalando o serviço FTP
apt-get install -y proftpd
#Criando o diretório que será compartilhado
if ! [ -d "~/share" ]; then
    mkdir -p ~/share
    echo "Diretório /share criado"
else
    echo "Diretório /share já existe"
fi
#Renomeando arquivo .conf
mv /etc/proftpd/proftpd.conf /etc/proftpd/proftpd.conf.bkp
#Copiando arquivo .conf
cp /vagrant_config/FTP/proftpd.conf /etc/proftpd/proftpd.conf
# Reiniciando o serviço
service proftpd restart
#Criando usuário "webmaster"
if ! id "webmaster" &>/dev/null; then
    useradd webmaster -d /root/share -s /bin/false
    echo "Usuário webmaster criado"
else
    echo "Usuário webmaster já existe"
fi
#Colocando o usuário "webmaster" como dono do diretório
chown webmaster -R ~/share
```

<br>
<hr>

<h4>> Código proftpd.conf </h4>

```bash
Include /etc/proftpd/modules.conf

UseIPv6 off

<IfModule mod_ident.c>
  IdentLookups off
</IfModule>

ServerName "FTP Server"
ServerType standalone
DeferWelcome off

DefaultServer on
ShowSymlinks on

TimeoutNoTransfer 600
TimeoutStalled 600
TimeoutIdle 1200

DisplayLogin welcome.msg
DisplayChdir .message true
ListOptions "-l"

DenyFilter \*.*/

DefaultRoot ~

RequireValidShell off

Port 21

<IfModule mod_dynmasq.c>
</IfModule>

MaxInstances 30

User proftpd
Group nogroup

Umask 022 022

AllowOverwrite on

TransferLog /var/log/proftpd/xferlog
SystemLog /var/log/proftpd/proftpd.log

<IfModule mod_quotatab.c>
QuotaEngine off
</IfModule>

<IfModule mod_ratio.c>
Ratios off
</IfModule>

<IfModule mod_delay.c>
DelayEngine on
</IfModule>

<IfModule mod_ctrls.c>
ControlsEngine off
ControlsMaxClients 2
ControlsLog /var/log/proftpd/controls.log
ControlsInterval 5
ControlsSocket /var/run/proftpd/proftpd.sock
</IfModule>

<IfModule mod_ctrls_admin.c>
AdminControlsEngine off
</IfModule>

 <Anonymous ~ftp>
   User ftp
   Group nogroup

   UserAlias anonymous ftp

   RequireValidShell off

   MaxClients 10

   DisplayLogin welcome.msg
   DisplayChdir .message
   <Directory ~/share>
     <Limit READ WRITE>
       AllowAll
     </Limit>
   </Directory>
 </Anonymous>
 
Include /etc/proftpd/conf.d/
```
