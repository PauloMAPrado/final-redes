# Tutorial: Instalação e Configuração do NFS no Linux


## Passo 1: Instalar o Servidor NFS
1. **Atualize os pacotes do sistema:**
   ```bash
   sudo apt update
   ```

2. **Instale o servidor NFS:**
   ```bash
   sudo apt install nfs-kernel-server -y
   ```

---

## Passo 2: Configurar Diretórios para Compartilhamento
1. **Crie o diretório a ser compartilhado:**
   ```bash
   sudo mkdir -p /mnt/nfs_share
   ```

2. **Configure as permissões:**
   ```bash
   sudo chown nobody:nogroup /mnt/nfs_share
   sudo chmod 755 /mnt/nfs_share
   ```

3. **Edite o arquivo de configuração de exportação:**
   ```bash
   sudo nano /etc/exports
   ```

4. **Adicione a seguinte linha ao arquivo (substitua `<IP_CLIENTE>` pelo IP do cliente ou use `*` para qualquer cliente):**
   ```ini
   /mnt/nfs_share <IP_CLIENTE>(rw,sync,no_subtree_check)
   ```

5. **Salve e feche o arquivo:** Pressione `Ctrl+O`, depois `Enter` e `Ctrl+X`.

---

## Passo 3: Reiniciar o Serviço NFS
1. **Reinicie o servidor NFS para aplicar as configurações:**
   ```bash
   sudo service nfs-kernel-server restart
   ```

2. **Verifique o status do serviço:**
   ```bash
   sudo service nfs-kernel-server status
   ```

---

## Passo 4: Configurar o Cliente NFS
1. **No cliente, instale os pacotes necessários:**
   ```bash
   sudo apt install nfs-common -y
   ```

2. **Monte o compartilhamento do servidor no cliente:**
   ```bash
   sudo mount <IP_SERVIDOR>:/mnt/nfs_share /mnt
   ```
   Substitua `<IP_SERVIDOR>` pelo IP do servidor NFS.

3. **Verifique se o compartilhamento foi montado:**
   ```bash
   df -h
   ```

---

## Conclusão

Se encontrar problemas, verifique os logs do sistema para depuração:
```bash
sudo tail -f /var/log/syslog
```
