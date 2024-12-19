
## Passo 1: Instalar o Servidor Apache

1. **Instale o Apache:**
   ```bash
   sudo apt install apache2 -y
   ```

2. **Verifique o status do serviço:**
   ```bash
   sudo service apache2 status
   ```

---


## Passo 2: Configurar o Firewall (se necessário)
1. **Permita conexões HTTP e HTTPS no firewall:**
   ```bash
   sudo ufw allow 80
   sudo ufw allow 443
   ```

2. **Recarregue o firewall:**
   ```bash
   sudo ufw reload
   ```

---

## Passo 3: Testar o Servidor Apache
1. **Abra um navegador e acesse o endereço do servidor:**
   ```

---

## Passo 5: Hospedar um Site
1. **Crie um arquivo HTML simples:**
   ```bash
   sudo nano /var/www/html/index.html
   ```
   Adicione o seguinte conteúdo:
   ```html
   <!DOCTYPE html>
   <html>
   <head>
       <title>Meu Servidor Apache</title>
   </head>
   <body>
       <h1>Servidor Apache Configurado com Sucesso!</h1>
   </body>
   </html>
   ```

3. **Recarregue o navegador para verificar o conteúdo do site.**
