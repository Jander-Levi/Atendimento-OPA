# Atendimento G2G

Plataforma omnichannel para gestão de atendimentos, contatos, canais de comunicação, fluxos automatizados, agentes virtuais, campanhas e rotinas operacionais.

## Visão geral

O sistema é dividido em uma aplicação frontend, uma API modular em .NET e um worker para tarefas assíncronas. A solução utiliza comunicação em tempo real para o atendimento, persistência relacional, cache e armazenamento de arquivos.

## Arquitetura

### Frontend

Aplicação construída com React, TypeScript, Vite e Ant Design.

- `app/`: rotas, layout, tema e menu lateral.
- `shared/`: cliente da API, autenticação e conexão em tempo real.
- `features/`: funcionalidades organizadas por domínio de negócio.

Principais áreas:

- Empresa, departamentos, usuários e permissões
- Etiquetas, motivos, mensagens rápidas, pausas e períodos/feriados
- Clientes, fornecedores e leads
- Canais, Opa! Store e webhooks
- Fluxos e editor visual com React Flow
- Agentes virtuais
- Fila, chat, novos atendimentos e agenda
- Envio em massa
- Relatórios, auditoria e rotinas

### Backend

O backend utiliza .NET e organização modular. Cada módulo de negócio é separado nas camadas `Api`, `Application`, `Domain` e `Infrastructure`.

- `Atendimento.Api`: Minimal API, endpoints HTTP e hub `/hubs/atendimento`.
- `Atendimento.Worker`: envios em massa, backups, integrações e tarefas agendadas.
- `Shared/`: kernel, segurança, persistência, armazenamento e auditoria.
- `Modules/`: módulos de negócio independentes e evolutivos.

Módulos previstos:

- `Identity`: empresas, departamentos, usuários e permissões
- `Rules`: períodos, feriados, etiquetas, motivos, mensagens e pausas
- `Contacts`: clientes, fornecedores e leads
- `Channels`: WhatsApp Cloud, Telegram, IXC, HTTP, SFTP e webhooks
- `Flows`: fluxos, estágios, execução e variáveis
- `VirtualAgents`: agentes virtuais e suas configurações
- `Conversations`: fila, atendimentos, protocolos `AT-` e transferências
- `Broadcast`: campanhas e envios em massa
- `Scheduling`: agenda e compromissos
- `Reports`: relatórios e indicadores
- `Routines`: backups de arquivos, banco de dados e chaves

## Estrutura do projeto

```text
Atendimento-OPA/
├── frontend/
│   └── src/
│       ├── app/
│       ├── shared/
│       └── features/
├── src/
│   ├── Host/
│   │   ├── Atendimento.Api/
│   │   └── Atendimento.Worker/
│   ├── Shared/
│   └── Modules/
├── docker/
│   └── docker-compose.yml
├── publish/
├── deploy.sh
├── ATUALIZACAO-SISTEMA.md
└── README.md
```

## Infraestrutura

O ambiente é executado com Docker Compose e inclui:

- API
- Worker
- PostgreSQL
- Redis
- MinIO

O PostgreSQL armazena os dados transacionais, o Redis atende necessidades de cache e processamento, e o MinIO é utilizado para arquivos e artefatos persistidos.

## Requisitos

- Node.js e npm
- SDK do .NET compatível com a solução
- Docker e Docker Compose
- Git

As versões exatas devem ser definidas nos arquivos de configuração do frontend e do backend assim que o projeto for inicializado.

## Execução local

### Subir os serviços de infraestrutura

```bash
docker compose -f docker/docker-compose.yml up -d
```

### Executar o frontend

```bash
cd frontend
npm install
npm run dev
```

### Executar a API e o worker

Na raiz da solução, utilize os comandos padrão do .NET definidos pelos respectivos projetos:

```bash
dotnet restore
dotnet run --project src/Host/Atendimento.Api
dotnet run --project src/Host/Atendimento.Worker
```

Os comandos e variáveis de ambiente específicos podem ser ajustados conforme os arquivos de configuração de cada ambiente.

## Configuração

As credenciais e configurações sensíveis devem ser fornecidas por variáveis de ambiente ou pelos mecanismos de secrets da infraestrutura. Não inclua tokens, senhas ou chaves diretamente no repositório.

Entre as configurações esperadas estão:

- String de conexão do PostgreSQL
- Endpoint e credenciais do Redis
- Endpoint e credenciais do MinIO
- Configurações de autenticação
- Credenciais dos canais e integrações externas
- URLs públicas da API e do frontend

## Implantação

O processo de implantação é automatizado pelo script `deploy.sh`. Os artefatos publicados ficam em `publish/`, enquanto o histórico e os procedimentos de atualização ficam registrados em `ATUALIZACAO-SISTEMA.md`.

Antes de atualizar um ambiente, confirme:

1. Que os serviços de infraestrutura estão disponíveis.
2. Que os backups necessários foram executados.
3. Que as variáveis de ambiente estão configuradas.
4. Que as migrações de banco foram aplicadas.
5. Que API, worker e frontend estão na mesma versão compatível.

## Diretrizes de desenvolvimento

- Organize o código por domínio de negócio.
- Preserve a separação entre API, aplicação, domínio e infraestrutura.
- Mantenha regras de negócio fora dos endpoints.
- Registre operações relevantes no módulo de auditoria.
- Prefira contratos explícitos entre frontend, API e integrações.
- Adicione testes para regras de negócio e fluxos críticos de atendimento.

## Status do projeto

Este repositório contém a base de organização do Atendimento G2G. A implementação dos módulos, contratos da API, configurações de ambiente e pipelines de implantação deve ser evoluída de forma incremental, priorizando autenticação, contatos, canais e atendimento.