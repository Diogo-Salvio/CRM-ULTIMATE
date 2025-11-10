# Configuração do MySQL e DBeaver

## 📋 Pré-requisitos

1. **MySQL instalado** no seu sistema
2. **DBeaver instalado** (ferramenta de gerenciamento de banco de dados)
3. **Extensão PHP PDO MySQL** habilitada

## 🔧 Configuração do Laravel

O arquivo `.env` já está configurado com as seguintes configurações padrão:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=
```

### Personalizando a Configuração

Se você quiser usar credenciais diferentes, edite o arquivo `.env` na pasta `back`:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1          # Host do MySQL (geralmente localhost)
DB_PORT=3306                # Porta padrão do MySQL
DB_DATABASE=seu_crm_db     # Nome do banco de dados
DB_USERNAME=seu_usuario    # Usuário do MySQL
DB_PASSWORD=sua_senha      # Senha do MySQL
```

## 🗄️ Criando o Banco de Dados

### Opção 1: Via MySQL Command Line

1. Abra o terminal/command prompt
2. Conecte ao MySQL:
```bash
mysql -u root -p
```

3. Crie o banco de dados:
```sql
CREATE DATABASE seu_crm_db CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```

4. Saia do MySQL:
```sql
EXIT;
```

### Opção 2: Via DBeaver (Recomendado)

1. Abra o DBeaver
2. Crie uma nova conexão MySQL (veja instruções abaixo)
3. Conecte ao servidor MySQL
4. Clique com o botão direito em "Databases" → "Create New Database"
5. Digite o nome do banco (ex: `crm_db`)
6. Selecione charset: `utf8mb4` e collation: `utf8mb4_unicode_ci`
7. Clique em "OK"

## 🔌 Configurando Conexão no DBeaver

### Passo a Passo:

1. **Abra o DBeaver**

2. **Crie uma Nova Conexão:**
   - Clique no ícone "New Database Connection" (plug com +)
   - Ou vá em: `Database` → `New Database Connection`

3. **Selecione MySQL:**
   - Na lista de bancos de dados, selecione **MySQL**
   - Clique em **Next**

4. **Configure os Parâmetros de Conexão:**
   ```
   Host:     127.0.0.1 (ou localhost)
   Port:     3306
   Database: seu_crm_db (ou o nome que você escolheu)
   Username: root (ou seu usuário MySQL)
   Password: sua_senha (deixe vazio se não tiver senha)
   ```

5. **Teste a Conexão:**
   - Clique em **Test Connection**
   - Se aparecer um erro sobre driver, clique em **Download** para baixar o driver MySQL
   - Aguarde o download e teste novamente

6. **Finalize:**
   - Clique em **Finish**
   - A conexão aparecerá no painel esquerdo

## 🚀 Executando as Migrations

Após criar o banco de dados e configurar o `.env`, execute as migrations:

```bash
cd back
php artisan migrate
```

Isso criará todas as tabelas necessárias no banco de dados:
- `users`
- `password_reset_tokens`
- `failed_jobs`
- `personal_access_tokens`

## ✅ Verificando a Conexão

### Via Laravel Artisan:

```bash
php artisan db:show
```

### Via DBeaver:

1. Expanda sua conexão MySQL no painel esquerdo
2. Expanda "Databases"
3. Expanda o nome do seu banco de dados
4. Expanda "Tables"
5. Você deve ver as tabelas criadas pelas migrations

## 🔍 Dicas Úteis

### Verificar se o PHP tem suporte ao MySQL:

```bash
php -m | findstr pdo_mysql
```

Se não aparecer `pdo_mysql`, você precisa habilitar a extensão no `php.ini`:
```ini
extension=pdo_mysql
```

### Resetar o Banco de Dados:

Se quiser recriar todas as tabelas do zero:

```bash
php artisan migrate:fresh
```

**⚠️ ATENÇÃO:** Isso apagará todos os dados existentes!

### Ver Estrutura das Tabelas:

No DBeaver, você pode:
- Clicar com botão direito na tabela → **View Data** (ver dados)
- Clicar com botão direito na tabela → **View DDL** (ver estrutura SQL)
- Clicar com botão direito na tabela → **Generate SQL** → **SELECT** (gerar queries)

## 🐛 Solução de Problemas

### ❌ Erro: "Connection refused: getsockopt" ou "Communications link failure"

Este erro indica que **o MySQL não está rodando** ou não está acessível. Siga estes passos:

#### 1. Verificar se o MySQL está instalado

**No PowerShell (como Administrador):**
```powershell
Get-Service | Where-Object {$_.Name -like "*mysql*"}
```

**Ou verifique manualmente:**
- Pressione `Win + R`
- Digite `services.msc` e pressione Enter
- Procure por serviços com "MySQL" no nome

#### 2. Se o MySQL NÃO estiver instalado:

**Opção A: Instalar MySQL Standalone**
1. Baixe o MySQL Installer: https://dev.mysql.com/downloads/installer/
2. Escolha "MySQL Installer for Windows"
3. Durante a instalação:
   - Escolha "Developer Default" ou "Server only"
   - Configure uma senha para o usuário `root`
   - Anote a senha que você configurou!
4. Após instalar, o MySQL será iniciado automaticamente

**Opção B: Instalar via XAMPP (Mais fácil)**
1. Baixe o XAMPP: https://www.apachefriends.org/download.html
2. Instale o XAMPP
3. Abra o "XAMPP Control Panel"
4. Clique em "Start" ao lado do MySQL
5. O MySQL estará disponível na porta 3306
6. Usuário padrão: `root`, Senha: (vazio)

**Opção C: Instalar via WAMP**
1. Baixe o WAMP: https://www.wampserver.com/
2. Instale o WAMP
3. Clique no ícone do WAMP na bandeja do sistema
4. Vá em "MySQL" → "Service" → "Start/Resume Service"

#### 3. Se o MySQL ESTIVER instalado mas não rodando:

**Via Services (Serviços do Windows):**
1. Pressione `Win + R`
2. Digite `services.msc` e pressione Enter
3. Procure por "MySQL" ou "MySQL80" ou "MySQL57"
4. Clique com botão direito → "Start" (Iniciar)
5. Se não iniciar, verifique se está configurado como "Automático"

**Via PowerShell (como Administrador):**
```powershell
# Listar serviços MySQL
Get-Service | Where-Object {$_.DisplayName -like "*mysql*"}

# Iniciar o serviço (substitua MySQL80 pelo nome do seu serviço)
Start-Service MySQL80

# Ou se o nome for diferente:
Start-Service MySQL57
```

**Via XAMPP Control Panel:**
- Abra o XAMPP Control Panel
- Clique em "Start" ao lado do MySQL

**Via WAMP:**
- Clique no ícone do WAMP na bandeja
- Vá em "MySQL" → "Service" → "Start/Resume Service"

#### 4. Verificar se o MySQL está rodando:

**No PowerShell:**
```powershell
Test-NetConnection -ComputerName localhost -Port 3306
```

Se `TcpTestSucceeded` for `True`, o MySQL está rodando!

**Ou teste via comando:**
```powershell
mysql -u root -p
```

Se conectar, o MySQL está funcionando!

#### 5. Configurar o DBeaver após iniciar o MySQL:

1. Abra o DBeaver
2. Crie uma nova conexão MySQL
3. Configure:
   - **Host:** `127.0.0.1` ou `localhost`
   - **Port:** `3306`
   - **Database:** (deixe vazio por enquanto ou crie um banco primeiro)
   - **Username:** `root`
   - **Password:** (a senha que você configurou, ou vazio se usar XAMPP)
4. Clique em "Test Connection"
5. Se pedir para baixar o driver, clique em "Download"

### Erro: "Access denied for user"
- Verifique se o usuário e senha estão corretos no `.env`
- Se você instalou o MySQL agora, use a senha que configurou durante a instalação
- Se estiver usando XAMPP, a senha padrão é vazia (deixe em branco)
- Certifique-se de que o usuário MySQL tem permissões para acessar o banco

### Erro: "Unknown database"
- Certifique-se de que o banco de dados foi criado
- Verifique se o nome do banco no `.env` está correto
- **Dica:** No DBeaver, você pode deixar o campo "Database" vazio ao testar a conexão inicialmente

### DBeaver não conecta após iniciar o MySQL
- Baixe o driver MySQL no DBeaver (ele pede automaticamente ao testar)
- Verifique se o firewall não está bloqueando a porta 3306
- Tente usar `localhost` ao invés de `127.0.0.1` (ou vice-versa)
- Reinicie o DBeaver após iniciar o MySQL
- Verifique se não há múltiplas instalações do MySQL conflitando

