# Tutorial: Instalação e Configuração do FTP no Linux

---

## Passo 1: Instalar o Servidor FTP
1. **Atualize os pacotes do sistema:**
   ```bash
   sudo apt update
   ```

2. **Instale o ProFTPD:**
   ```bash
   sudo apt install proftpd -y
   ```

3. **Verifique o status do serviço:**
   ```bash
   sudo service proftpd status
   ```

---

## Passo 2: Configurar o ProFTPD
1. **Edite o arquivo de configuração principal:**
   ```bash
   sudo nano /etc/proftpd/proftpd.conf
   ```

2. **Habilite as seguintes opções (descomente e ajuste conforme necessário):**
   ```ini
   ServerName "Servidor FTP"
   DefaultRoot ~
   RequireValidShell off
   ```
   

3. **Salve e feche o arquivo:** Pressione `Ctrl+O`, depois `Enter` e `Ctrl+X`.

4. **Reinicie o serviço para aplicar as mudanças:**
   ```bash
   sudo service proftpd restart
   ```

---

## Passo 3: Configurar Usuários Locais (Opcional)
1. **Crie um usuário para o FTP:**
   ```bash
   sudo adduser ftpuser
   ```
   Siga as instruções para definir uma senha.

2. **Configure o diretório inicial do usuário:**
   ```bash
   sudo mkdir -p /home/ftpuser/ftp/upload
   sudo chown -R ftpuser:ftpuser /home/ftpuser/ftp
   sudo chmod -R 755 /home/ftpuser/ftp
   ```

