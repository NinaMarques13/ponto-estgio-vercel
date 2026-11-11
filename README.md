# 🕐 Ponto-Estagio - Sistema de Controle de Ponto para Estagiários

Um sistema web completo para gerenciar o registro de ponto, dados de estagiários, turnos e geração de relatórios em PDF. Desenvolvido para órgãos públicos e instituições que necessitam controlar a presença de estagiários.

## ✨ Funcionalidades Principais

- ✅ **Autenticação de Administradores** - Login seguro com CPF e senha
- 📱 **Registro de Ponto com QR Code** - Captura automática de entrada/saída
- 👥 **Gestão de Estagiários** - Cadastro, edição e desativação de estagiários
- 🔄 **Controle de Turnos** - Associação de turnos aos estagiários
- 📊 **Listagem de Registros** - Visualização de presença por dia
- 📄 **Geração de Relatórios PDF** - Exportação de dados em PDF
- 📈 **Tabelas Interativas** - DataTables com filtros, busca e paginação
- 🔐 **Segurança** - Registro de IP e rastreamento de acessos

## 🛠️ Requisitos Técnicos

- **PHP** >= 8.2
- **Composer** >= 2.0
- **Node.js** >= 18.x
- **NPM** >= 9.x
- **Banco de Dados** - MySQL, PostgreSQL ou compatível
- **Servidor Web** - Apache, Nginx ou similar

## 📦 Dependências Principais

```json
{
  "laravel/framework": "^12.0",
  "barryvdh/laravel-dompdf": "^3.1",
  "yajra/laravel-datatables": "^12.0",
  "yajra/laravel-datatables-buttons": "^12.0"
}
```

## 🚀 Instalação

### 1. Clonar o Repositório

```bash
git clone https://github.com/NinaMarques13/ponto-estgio.git
cd ponto-estgio
```

### 2. Instalar Dependências

```bash
# Instalação completa (recomendado para primeira vez)
composer setup
```

Ou manualmente:

```bash
# Instalar dependências PHP
composer install

# Criar arquivo .env
cp .env.example .env

# Gerar chave da aplicação
php artisan key:generate

# Executar migrations
php artisan migrate

# Instalar dependências Node.js
npm install

# Build frontend
npm run build
```

### 3. Configurar Banco de Dados

Edite o arquivo `.env`:

```env
DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=ponto_estagio
DB_USERNAME=seu_usuario
DB_PASSWORD=sua_senha
```

### 4. Popular Banco de Dados (Opcional)

```bash
php artisan migrate:fresh --seed
```

## 📚 Uso

### Iniciar o Servidor de Desenvolvimento

```bash
# Opção 1: Modo desenvolvimento com hot-reload (recomendado)
composer dev
```

Isso irá iniciar simultaneamente:
- Servidor Laravel (port 8000)
- Queue listener
- Pail logs
- Vite dev server

```bash
# Opção 2: Servidor simples
php artisan serve
```

A aplicação estará disponível em `http://localhost:8000`

### Login de Administrador

1. Acesse `http://localhost:8000/admin/login`
2. CPF: Use os dados do seeder AdminSeeder
3. Senha: Configurada durante o setup

### Registrar Ponto com QR Code

1. Acesse a página de registro
2. Escaneie o QR code do estagiário
3. O sistema capturará automaticamente o horário e IP

## 📁 Estrutura do Projeto

```
ponto-estagio/
├── app/
│   ├── Domains/                  # Organização por domínio de negócio
│   │   ├── Admins/
│   │   │   ├── Controllers/      # Login e cadastro de administradores
│   │   │   └── Models/           # Modelo Admin
│   │   ├── ControleDePonto/
│   │   │   ├── Controllers/      # Registro e consulta de ponto
│   │   │   ├── Models/           # RegistroPonto e Turno
│   │   │   └── Services/         # Regras do controle de ponto
│   │   ├── Estagiarios/
│   │   │   ├── Controllers/      # Cadastro e listagem
│   │   │   ├── Models/           # Modelo Estagiario
│   │   │   ├── Requests/         # Validação das requisições
│   │   │   └── Services/         # Regras de estagiários
│   │   └── Eventos/
│   │       ├── Controllers/      # Operações de eventos
│   │       └── Services/         # Regras de eventos
│   ├── Http/
│   │   ├── Controllers/
│   │   │   └── Controller.php    # Controller base compartilhado
│   │   └── Middleware/           # Middlewares compartilhados
│   ├── Models/
│   │   └── User.php              # Modelo padrão do Laravel
│   └── Providers/                # Provedores da aplicação
├── database/
│   ├── migrations/               # Schema do banco de dados
│   ├── seeders/                  # Seeds para popular dados
│   └── factories/                # Factories para testes
├── resources/
│   ├── views/                    # Templates Blade
│   │   └── pages/
│   │       ├── inicio/           # Página inicial
│   │       ├── login/            # Login admin
│   │       ├── principal/        # Dashboard e funcionalidades
│   │       └── templates/        # Layouts
│   ├── css/                      # Estilos
│   └── js/                       # Scripts frontend
├── routes/
│   ├── web.php                   # Rotas da aplicação
│   └── console.php               # Comandos Artisan
├── public/
│   ├── js/                       # JavaScript compilado
│   ├── css/                      # CSS compilado
│   ├── img/                      # Imagens
│   └── index.php                 # Entry point
├── tests/
│   ├── Feature/                  # Testes de funcionalidades
│   └── Unit/                     # Testes unitários
└── artisan                       # CLI do Laravel
```

## 🗄️ Modelos de Dados

### Estagiario
- `nm_estagiarios` - Nome completo
- `cpf` - CPF do estagiário
- `nm_setor` - Setor de atuação
- `nr_telefone` - Telefone para contato
- `nm_email` - Email
- `ds_situacao` - Situação (ativo/inativo)

### RegistroPonto
- `estagiario_id` - FK para Estagiario
- `ds_motivo` - Motivo do registro
- `hr_registro` - Hora do registro
- `ip_registro` - IP do acesso
- `ds_observacao` - Observações adicionais

### Turno
- `estagiario_id` - FK para Estagiario
- Dados do turno de trabalho

### Admin
- `cpf` - CPF único
- `name` - Nome completo
- `email` - Email
- `password` - Senha (hash)
- `level` - Nível de acesso

## 🔐 Segurança

- Autenticação separada para administradores
- Registro de IP em cada ação
- Senhas armazenadas com hash (bcrypt)
- CSRF protection automático do Laravel
- SQL Injection prevention com Eloquent ORM

## 🧪 Testes

Executar testes:

```bash
# Todos os testes
php artisan test

# Apenas testes unitários
php artisan test --testsuite=Unit

# Apenas testes feature
php artisan test --testsuite=Feature

# Com cobertura de código
php artisan test --coverage
```

## 📊 Relatórios

### Gerar PDF

```bash
# Via interface web
GET /admin/export
```

### Exportar Dados

```bash
# Endpoint de exportação
GET /admin/export
```

## 🔧 Comandos Úteis

```bash
# Limpar caches
php artisan cache:clear
php artisan config:clear

# Otimizar aplicação
php artisan optimize

# Migrations
php artisan migrate           # Executar todas
php artisan migrate:rollback  # Reverter última
php artisan migrate:refresh   # Resetar e rodar todas

# Seeds
php artisan db:seed
php artisan db:seed --class=AdminSeeder

# Tinker (shell interativo)
php artisan tinker
```

## 📝 Variáveis de Ambiente

Arquivo `.env`:

```env
APP_NAME="Ponto Estagio"
APP_ENV=local
APP_DEBUG=true
APP_URL=http://localhost:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=ponto_estagio
DB_USERNAME=root
DB_PASSWORD=

MAIL_MAILER=smtp
MAIL_HOST=smtp.mailtrap.io
MAIL_PORT=465
MAIL_USERNAME=seu_usuario
MAIL_PASSWORD=sua_senha
MAIL_ENCRYPTION=tls
MAIL_FROM_ADDRESS=nao-responda@ponto-estagio.local
MAIL_FROM_NAME="Ponto Estagio"
```

## Deploy na Vercel

O deploy usa o runtime PHP comunitario `vercel-php` definido em `vercel.json`. Importe o repositorio na Vercel, use o preset **Other**, mantenha o comando de build como `npm run build` e nao defina um diretorio de saida. O runtime instala as dependencias PHP a partir do `composer.lock`; o build do Vite gera os assets em `public/build`.

Configure estas variaveis no projeto Vercel, tanto em Production quanto nos ambientes de preview que forem usar o banco:

```env
APP_ENV=production
APP_DEBUG=false
APP_URL=https://seu-dominio.vercel.app
APP_KEY=base64:...
DB_CONNECTION=pgsql
DB_URL=<URL externa do PostgreSQL no Render>
SESSION_DRIVER=database
CACHE_STORE=database
QUEUE_CONNECTION=sync
LOG_CHANNEL=stderr
VIEW_COMPILED_PATH=/tmp/views
```

Use a **External Database URL** do Render em `DB_URL`, nao a URL interna. Mantenha a URL completa, incluindo os parametros TLS fornecidos pelo Render, se houver. O banco continua hospedado no Render; a Vercel acessa esse banco pela conexao externa. Nao coloque credenciais no repositorio.

Defina uma `APP_KEY` segura como variavel de ambiente, sem comita-la. A chave atualmente registrada em `render.yaml` esta exposta no repositorio: gere uma nova e atualize o ambiente do Render e o da Vercel com a mesma chave. A troca invalida cookies/sessoes existentes.

Antes de liberar o site, confirme que as migrations ja foram executadas no PostgreSQL do Render. Se o banco estiver vazio, execute `php artisan migrate --force` uma vez em um ambiente conectado a ele; nao rode migrations a cada requisicao da funcao. As tabelas de sessao e cache ja estao nas migrations do projeto.

O filesystem local da Vercel e temporario. O projeto nao apresentou uso de armazenamento local na aplicacao; se adicionar uploads ou arquivos que precisem persistir, use um storage externo compativel com S3. Como a Vercel nao mantem um worker de fila permanente, `QUEUE_CONNECTION=sync` evita jobs parados; configure um worker externo caso passe a usar filas assincronas.

## 🤝 Contribuição

Para contribuir com o projeto:

1. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
2. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
3. Push para a branch (`git push origin feature/AmazingFeature`)
4. Abra um Merge Request

### Padrões de Código

- PSR-12 via Laravel Pint (`php artisan pint`)
- Nomes em português para modelos e banco de dados
- Comentários em português
- Variáveis descritivas

## 🐛 Troubleshooting

### Erro: "Class not found"
```bash
composer dump-autoload
```

### Erro de permissão em storage/
```bash
chmod -R 775 storage/
```

### Banco de dados desatualizado
```bash
php artisan migrate:fresh --seed
```

### Cache corrompido
```bash
php artisan cache:clear
php artisan config:clear
php artisan view:clear
```

## 📞 Suporte

Para reportar bugs ou solicitar features:
- Abra uma issue no GitLab
- Entre em contato: pm-est.nicolaspaiva@pm.pr.gov.br

## 📄 Licença

Este projeto está licenciado sob a MIT License - veja o arquivo LICENSE para detalhes.

## 👨‍💼 Autores

- **Nicolas Paiva** - Desenvolvimento inicial
- Equipe de Estagiários - PM/PR

---

**Versão:** 1.0.0  
**Última atualização:** Junho 2026  
**Status:** Em desenvolvimento ✨
