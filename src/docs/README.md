# Documentação - Sistema de Automação de Abastecimento de Frotas

## Propósito
Este diretório contém toda a documentação técnica e funcional do projeto de automação de abastecimento de frotas. A documentação é essencial para:
- **Entendimento do Sistema**: Visão completa da arquitetura e funcionalidades
- **Onboarding**: Facilitar a integração de novos desenvolvedores
- **Manutenção**: Documentação de decisões técnicas e padrões
- **Referência de API**: Especificações detalhadas dos endpoints
- **Diagramas e Fluxos**: Visualização da arquitetura e processos

## Estrutura Planejada
```
src/docs/
├── architecture/        # Documentação de arquitetura
│   ├── system-design.md  # Design geral do sistema
│   ├── database-schema.md # Esquema do banco de dados
│   └── diagrams/         # Diagramas UML, C4, etc.
├── api/                 # Documentação da API
│   ├── endpoints.md      # Lista de endpoints
│   ├── authentication.md # Autenticação e autorização
│   └── examples.md       # Exemplos de uso
├── frontend-web/        # Docs do frontend web
│   ├── components.md     # Componentes e estrutura
│   ├── screens.md        # Telas e fluxos
│   └── wireframes/       # Wireframes e mockups
├── frontend-mobile/     # Docs do frontend mobile
│   ├── screens.md        # Telas e navegação
│   ├── features.md       # Funcionalidades específicas
│   └── mockups/          # Mockups das telas
├── business/            # Regras de negócio
│   ├── requirements.md   # Requisitos funcionais
│   ├── user-stories.md   # Histórias de usuário
│   └── workflows.md      # Fluxos de trabalho
├── deployment/          # Deploy e infraestrutura
│   ├── setup.md          # Configuração inicial
│   ├── ci-cd.md          # Pipeline CI/CD
│   └── environments.md   # Ambientes (dev, staging, prod)
├── guides/              # Guias e tutoriais
│   ├── contributing.md   # Como contribuir
│   ├── coding-standards.md # Padrões de código
│   └── testing.md        # Guia de testes
└── README.md            # Este arquivo
```

## Tipos de Documentação
### 1. Documentação Técnica
- Arquitetura do sistema
- Diagramas de classes, sequência e componentes
- Especificações de API (Swagger/OpenAPI)
- Schema do banco de dados
- Fluxos de dados

### 2. Documentação Funcional
- Requisitos do sistema
- Histórias de usuário
- Casos de uso
- Regras de negócio
- Fluxos de processos

### 3. Documentação de Interface
- Wireframes
- Mockups
- Design System
- Guia de estilos
- Prototipação interativa

---

## Wireframe Detalhado — Tela: Painel Geral

Objetivo: visão executiva do sistema com KPIs, gráficos, lista recente de abastecimentos e alertas.

Resumo de layout:
- Header superior com logo, busca, ações rápidas e perfil
- Menu lateral fixo com navegação principal
- Área principal com: KPIs no topo, grade de gráficos, lista de abastecimentos recentes e área de alertas

Legenda de componentes:
- H = Header
- S = Sidebar (menu lateral)
- K = Card KPI
- G = Gráfico (linha/barras/pizza)
- L = Lista de abastecimentos
- A = Alertas/Notificações

```mermaid
flowchart TB
  %% Contêiner principal
  subgraph APP[Dashboard - Painel Geral]
    direction TB

    %% Header
    H[Header: Logo | Busca | Ações rápidas | Perfil]:::header

    %% Corpo com duas colunas: Sidebar + Conteúdo
    subgraph BODY
      direction LR
      S[Menu Lateral:\n- Painel Geral\n- Abastecimentos\n- Veículos\n- Postos\n- Relatórios\n- Configurações]:::sidebar

      subgraph MAIN[Conteúdo]
        direction TB
        %% Linha de KPIs
        subgraph KPI_ROW[KPIs]
          direction LR
          K1[KPI: Custo/mês]:::kpi
          K2[KPI: Litros/mês]:::kpi
          K3[KPI: Ticket médio]:::kpi
          K4[KPI: Desvio consumo]:::kpi
        end

        %% Grade de gráficos
        subgraph CHARTS[Gráficos]
          direction LR
          G1[Gráfico Linha: Consumo vs Tempo]:::chart
          G2[Gráfico Barras: Custo por veículo]:::chart
          G3[Pizza: Combustível por tipo]:::chart
        end

        %% Lista e alertas em duas colunas
        subgraph LIST_ALERTS[Registros e Alertas]
          direction LR
          L[Lista Abastecimentos Recentes\n- Data | Veículo | Litros | Custo | Posto\n- Ações: Ver, Editar]:::list
          A[Alertas/Anomalias\n- Odômetro inconsistente\n- Valor acima do teto\n- Desvio consumo]:::alert
        end
      end
    end
  end

  classDef header fill:#f5f7fb,stroke:#c9d2e3,color:#1f2937;
  classDef sidebar fill:#f8fafc,stroke:#d1d5db,color:#111827;
  classDef kpi fill:#ffffff,stroke:#e5e7eb,color:#111827;
  classDef chart fill:#ffffff,stroke:#e5e7eb,color:#111827;
  classDef list fill:#ffffff,stroke:#e5e7eb,color:#111827;
  classDef alert fill:#fff7ed,stroke:#fdba74,color:#9a3412;
```

Estrutura e estados esperados:
- Header: campo de busca global, ícone “+ Novo Abastecimento”, notificações e avatar
- Sidebar: itens com ícone, estado ativo e colapsável
- KPIs: cards responsivos (4 por linha em desktop, 2 em tablet, 1 em mobile), com indicador de variação (% e seta)
- Gráficos: placeholders com legendas e períodos (filtro: 7d, 30d, 90d)
- Lista de abastecimentos: paginação, ordenação por data desc, filtros rápidos (veículo, posto, período)
- Alertas: níveis (info/atenção/crítico), botões “ver detalhes” e “silenciar”

Detalhamento de dados por componente:
- KPIs: {titulo, valor, variacaoPct, periodo}
- Gráficos: {tipo, serie[], categorias[], periodo}
- Lista: {id, dataHora, veiculo, litros, custo, posto, status}
- Alertas: {id, severidade, mensagem, origem, criadoEm}

Acessibilidade e UX:
- Navegação por teclado: focos visíveis e ordem lógica
- Contraste AA em cards de alerta e KPIs
- Rótulos e descrições para leitores de tela em gráficos e ações
- Mensagens vazias: estados “sem registros” e “sem alertas” com CTAs relevantes

---

## Ferramentas Sugeridas
- **Diagramas**: Draw.io, Lucidchart, PlantUML, Mermaid
- **Mockups**: Figma, Adobe XD, Sketch
- **API Docs**: Swagger UI, Postman
- **Colaboração**: Notion, Confluence, GitHub Wiki

## Como Contribuir com a Documentação
1. Mantenha a documentação sempre atualizada
2. Use linguagem clara e objetiva
3. Inclua exemplos práticos quando possível
4. Mantenha diagramas versionados junto ao código
5. Revise periodicamente para garantir precisão

## Status
✅ **Wireframes Básicos** — Painel Geral documentado com diagrama Mermaid
🚧 **Em construção** — Aguardando documentação detalhada e implementação
