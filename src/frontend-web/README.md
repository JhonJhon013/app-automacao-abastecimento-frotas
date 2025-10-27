# Frontend Web - Sistema de Automação de Abastecimento de Frotas

## Propósito

Este diretório contém o código do frontend web da aplicação de automação de abastecimento de frotas. O frontend web é responsável por:

- **Interface de Administração**: Painel completo para gestão da frota
- **Dashboard e Relatórios**: Visualização de dados, gráficos e estatísticas
- **Gerenciamento de Veículos**: Cadastro, edição e monitoramento de veículos
- **Controle de Abastecimentos**: Registro e histórico de abastecimentos
- **Gestão de Motoristas**: Cadastro e gerenciamento de motoristas
- **Relatórios**: Geração de relatórios customizados e exportação de dados

## Estrutura Planejada

```
src/frontend-web/
├── public/           # Arquivos estáticos públicos
├── src/
│   ├── assets/       # Imagens, fontes, ícones
│   ├── components/   # Componentes reutilizáveis
│   ├── pages/        # Páginas da aplicação
│   ├── layouts/      # Layouts base
│   ├── services/     # Serviços de API e integrações
│   ├── contexts/     # Contextos do React (ou stores)
│   ├── hooks/        # Custom hooks
│   ├── utils/        # Funções utilitárias
│   ├── styles/       # Estilos globais
│   └── routes/       # Configuração de rotas
├── tests/            # Testes unitários e E2E
└── README.md         # Este arquivo
```

## Tecnologias Previstas

- **Framework**: React.js ou Next.js
- **Estilização**: Tailwind CSS, Material-UI ou Styled Components
- **Gerenciamento de Estado**: Context API, Redux ou Zustand
- **Gráficos**: Chart.js ou Recharts
- **Comunicação com API**: Axios ou Fetch API
- **Roteamento**: React Router

## Funcionalidades Principais

1. **Autenticação e Autorização**
   - Login/Logout
   - Controle de permissões por perfil

2. **Dashboard**
   - Visão geral da frota
   - Gráficos de consumo e custos
   - Alertas e notificações

3. **Módulos de Gestão**
   - Veículos
   - Motoristas
   - Abastecimentos
   - Relatórios

## Como Começar

1. Instalar dependências: `npm install` ou `yarn install`
2. Configurar variáveis de ambiente (`.env`)
3. Executar em modo desenvolvimento: `npm run dev` ou `yarn dev`
4. Build para produção: `npm run build` ou `yarn build`

## Status

🚧 **Em construção** - Aguardando primeira implementação
