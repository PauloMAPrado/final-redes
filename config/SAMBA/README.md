# Configuração do Servidor Samba no Linux


---

## **1. Instalação do Samba**

Para instalar o Samba, execute o comando:
```bash
sudo apt install samba -y
```

---

## **2. Criação de Usuários sem login no sistema**

Crie usuários que não possam fazer login no sistema e sem diretório home:
```bash
sudo adduser --disabled-login --no-create-home nome_do_usuario
```

---

## **3. Criação de grupos**

Crie grupos para os usuários:
```bash
sudo addgroup nome_do_grupo
```

Adicione os usuários aos grupos:
```bash
sudo usermod -a -G nome_do_grupo nome_do_usuario
```
Isso facilita o gerenciamento de permissões para recursos compartilhados.

---

## **4. Configuração de diretórios e permissões**

1. **Crie o diretório pai:**
   ```bash
   sudo mkdir /caminho/diretorio_pai
   ```

2. **Defina permissões de acesso:**
   ```bash
   sudo chmod 775 /caminho/diretorio_pai
   ```

3. **Crie subdiretórios:**
   ```bash
   sudo mkdir /caminho/diretorio_pai/subdiretorio
   ```

4. **Defina permissões para os subdiretórios:**
   ```bash
   sudo chmod -R 770 /caminho/diretorio_pai/subdiretorio
   ```

5. **Atribua o grupo correto ao subdiretório:**
   ```bash
   sudo chown -R root:grupo /caminho/diretorio_pai/subdiretorio
   ```

---

## **5. Configuração de senhas para usuários**

Configure a senha dos usuários para acesso ao Samba:
```bash
sudo smbpasswd -a nome_do_usuario
```

---

## **6. Configuração do arquivo smb.conf**

1. **Renomeie o arquivo de configuração padrão:**
   ```bash
   sudo mv /etc/samba/smb.conf /etc/samba/smb.conf.bkp
   ```

2. **Edite o arquivo smb.conf:**
   ```bash
   sudo vim /etc/samba/smb.conf
   ```

3. **Exemplo de configuração:**
   ```ini
   [global]
   netbios name = servidorSamba
   workgroup = WORKGROUP

   [arquivos]
   path = /home/samba
   writeable = yes
   available = yes
   force group = financeiro
   create mask = 0770
   directory mask = 0770
   valid users = @financeiro
   ```

   - **[global]**: Configurações gerais do servidor.
     - `netbios name`: Nome do servidor na rede.
     - `workgroup`: Grupo de trabalho.
   - **[arquivos]**: Configurações do compartilhamento.
     - `path`: Caminho do diretório compartilhado.
     - `writeable`: Permitir escrita.
     - `available`: Disponibilidade do compartilhamento.
     - `force group`: Força o grupo associado.
     - `create mask` e `directory mask`: Permissões de criação de arquivos/diretórios.
     - `valid users`: Lista de usuários/grupos com acesso.

4. **Salve e saia do editor:**
   ```bash
   :wq!
   ```

---

## **7. Reiniciar o Samba**

Reinicie o serviço para aplicar as configurações:
```bash
sudo systemctl restart smbd.service
```

---

