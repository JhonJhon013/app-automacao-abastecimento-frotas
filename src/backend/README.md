# Backend - Sistema de Automação de Abastecimento de Frotas

## Propósito

Este diretório contém o código do backend da aplicação de automação de abastecimento de frotas. O backend é responsável por:

- **API RESTful**: Fornecer endpoints para comunicação com os frontends (web e mobile)
- **Lógica de Negócio**: Implementar as regras de negócio do sistema de abastecimento
- **Gerenciamento de Dados**: Controlar o acesso e persistência de dados de veículos, abastecimentos, motoristas e relatórios
- **Autenticação e Autorização**: Gerenciar segurança e controle de acesso
- **Integração**: Conectar com sistemas externos e APIs de terceiros quando necessário

## Estrutura Planejada

```
src/backend/
├── config/          # Configurações da aplicação
├── controllers/     # Controladores da API
├── models/          # Modelos de dados
├── routes/          # Definição de rotas
├── services/        # Lógica de negócio
├── middlewares/     # Middlewares (autenticação, validação, etc.)
├── utils/           # Utilitários e helpers
├── tests/           # Testes unitários e de integração
└── README.md        # Este arquivo
```

## Tecnologias Previstas

- **Node.js** com Express ou NestJS
- **Banco de Dados**: PostgreSQL ou MongoDB
- **Autenticação**: JWT
- **Documentação**: Swagger/OpenAPI

## Como Começar

1. Instalar dependências (após configuração inicial)
2. Configurar variáveis de ambiente
3. Executar migrations do banco de dados
4. Iniciar servidor de desenvolvimento

## Status

🚧 **Em construção** - Aguardando primeira implementação
