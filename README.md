<h1>
  Administração de Redes de Computadores
</h1>

<br>

<h3>
  Sobre o Repositório
</h3>
<hr>
<p>
  O presente repositório tem por principal objetivo conter toda a documentação oficial do trabalho final da disciplina de Administração de Redes de Computadores, oferecida pelo Instituto Federal Goiano - Campus Ceres durante o 4º período do curso de Sistemas de Informação, visando a apresentação e documentação como formas de obtenção de nota para aprovação na disciplina citada anteriormente.
</p>

<br>

<h3>
  Professor Orientador
</h3>
<hr>
<div>
  <h5>Roitier Campos Gonçalves</h5>
  <p>
    Email: roitier.goncalves@ifgoiano.edu.br
    <br>
    Github: https://github.com/Roitier
  </p>
</div>

<br>

<h3>
  Alunos
</h3>
<hr>
<div>
  <h5>Paulo Martins Alves do Prado</h5>
  <p>
    Email: paulo.prado1@estudante.ifgoiano.edu.br
    <br>
    Github: https://github.com/PauloMAPrado
  </p>
</div>
<br>
<div>
  <h5>Sara Luiz de Farias</h5>
  <p>
    Email: sara.luiz@estudante.ifgoiano.edu.br
    <br>
    Github: https://github.com/saraafariass
  </p>
</div>

<br>

<h3>
  Preparando o Terreno
</h3>
<hr>
<h3>
  1. Instalação
</h3>
<p>
  Realize a instalação de Vagrant(v2.4.2) e do Oracle VirtualBox (v7.1.4).
  <br>
  OBS: Links de download disponíveis no final do documento
</p>

<h3>
  2. Clonagem
</h3>
<p>
  Com a instalação acima concluída, clone o repositório para sua máquina usando a seguinte linha de código em seu terminal:
</p>

```bash
git clone https://github.com/PauloMAPrado/final-redes.git
```

<h3>
  3. Iniciando
</h3>
<p>
  Ao clonar o repositório, ainda usando seu terminal, se dirija até a pasta que foi clonada contendo o Vagratfile e execute o seguinte comando:
</p>

```bash
vagrant up
```
<h3>
  4. Provisão
</h3>
<p>
  A provissão consiste na instalação do pacote original e a mudança do arquivo de configuração original para o arquivo de configuração presente no Vagrantfile, tornando o serviço funcional
</p>
<h3>
  4.1. DHCP
</h3>

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

<h3>
  4.2. DNS
</h3>

```bash
#Instalando o serviço DNS
apt install -y bind9 bind9-doc bind9utils
#Copiando o arquivo de opçoes
cp /vagrant_config/DNS/named.conf.options /etc/bind/named.conf.options
#Copiando o arquivo de configuração local
cp /vagrant_config/DNS/named.conf.local /etc/bind/named.conf.local
#Reiniciando o serviço
systemctl restart bind9
```
<br>

<h3>
  4.3. FTP
</h3>

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

<h3>
  4.4. Samba
</h3>

```bash
#Instalando pacotes necessários para Samba
apt-get install -y libcups2 samba-common cups samba
#Renomeando arquivo .conf
mv /etc/samba/smb.conf /etc/samba/smb.conf.bkp
#Copiando arquivo .conf
cp /vagrant_config/SAMBA/smb.conf /etc/samba/smb.conf
#Criando grupo de usuários
groupadd users
#Criando diretórios compartilhados do Samba
for dir in /home/shares/allusers /home/shares/anonymous; do
    if ! [ -d "$dir" ]; then
        mkdir -p "$dir"
        echo "Diretório $dir criado"
    else
        echo "Diretório $dir já existe"
    fi
done
#Definindo permissões
chown -R root:users /home/shares/allusers/
chmod -R 0771 /home/shares/allusers/
chown -R root:users /home/shares/anonymous/
chmod -R 0771 /home/shares/anonymous/
#Criando usuário no Samba e configurar senha
USER="client"
SENHA="teste"
useradd -m -G users "$USER"
(echo "$SENHA"; echo "$SENHA") | smbpasswd -a "$USER" -s
#Reiniciar o serviço do Samba
systemctl restart smbd.service
```
<br>
<h3>
  4.5. APACHE
</h3>

```bash
    #Instalar o APACHE
    sudo apt install -y apache2
    #Startando o serviço APACHE
    sudo systemctl start apache2
    #Ativando o cervidor
    sudo systemctl enable apache2
```

<br>

<h3>
  5. Acessando Novamente
</h3>
<p>
  Ao concluir os passos acima, você terá uma máquina virtual rodando pelo terminal de seu computador, para iniciá-la novamente, basta inserir a seguinte linha de código:
</p>

```bash
vagrant ssh
```

<br>

<h3>
  Links Relacionados
</h3>
<hr>

<h5>Vagrant</h5>
Documentação: https://developer.hashicorp.com/vagrant/docs
<br>
Download: https://developer.hashicorp.com/vagrant/docs/installation
<br>
<br>
  
<h5>Oracle VirtualBox</h5>
Documentação: https://docs.oracle.com/en/virtualization/virtualbox/
<br>
Download: https://www.oracle.com/br/virtualization/technologies/vm/downloads/virtualbox-downloads.html
<br>
<br>

<h5>APACHE</h5>
Documentação: https://httpd.apache.org/docs/
<br>
Download: https://httpd.apache.org/download.cgi
<br>
<br>

<h5>DHCP</h5>
Documentação: https://learn.microsoft.com/pt-br/windows-server/networking/technologies/dhcp/dhcp-top
<br>
<br>

<h5>DNS</h5>
Documentação:https://learn.microsoft.com/en-us/windows-server/networking/dns/dns-top
<br>
<br>

<h5>FTP</h5>
Documentação: https://docs.digibee.com/documentation/components/file-storage/ftp
<br>
<br>

<h5>NFS</h5>
Documentação: https://docs.kernel.org/admin-guide/nfs/index.html
<br>
<br>

<h5>SAMBA</h5>
Documentação: https://www.samba.org/samba/docs/
<br>
<br>
