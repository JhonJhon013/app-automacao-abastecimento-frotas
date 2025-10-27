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

### 4. Guias de Desenvolvimento
- Setup do ambiente
- Padrões de código
- Convenções de commits
- Guia de contribuição
- Boas práticas

## Diagramas a Serem Criados
1. **Diagrama de Contexto (C4)**: Visão geral do sistema
2. **Diagrama de Container**: Componentes principais
3. **Diagrama de Componentes**: Estrutura interna
4. **Diagrama ER**: Modelo de dados
5. **Diagramas de Sequência**: Fluxos principais
   - Registro de abastecimento
   - Autenticação de usuário
   - Geração de relatórios
6. **Diagramas de Caso de Uso**: Interações do usuário
7. **Fluxogramas**: Processos de negócio

## Wireframes das Telas Principais

### 1. Painel Geral (Dashboard)

```mermaid
graph TD
    A[Header/Navigation Bar] --> B[Menu Lateral]
    A --> C[Área Principal]
    
    B --> B1[Dashboard]
    B --> B2[Abastecimentos]
    B --> B3[Frotas]
    B --> B4[Relatórios]
    B --> B5[Configurações]
    
    C --> C1[Seção KPIs]
    C --> C2[Seção Gráficos]
    C --> C3[Seção Resumos]
    
    C1 --> C1A[Total Abastecimentos]
    C1 --> C1B[Consumo do Mês]
    C1 --> C1C[Economia Gerada]
    C1 --> C1D[Veículos Ativos]
    
    C2 --> C2A[Gráfico Consumo Mensal]
    C2 --> C2B[Gráfico por Veículo]
    
    C3 --> C3A[Últimos Abastecimentos]
    C3 --> C3B[Alertas e Notificações]
```

**Descrição do Layout - Painel Geral:**
- **Header**: Barra superior com logo, título do sistema e menu do usuário
- **Menu Lateral**: Navegação principal fixa com ícones e labels
- **Área de KPIs**: Cards com métricas importantes (4 principais)
- **Seção de Gráficos**: Dois gráficos principais lado a lado
- **Área de Resumos**: Lista dos últimos abastecimentos e área de alertas

### 2. Registro de Abastecimento

```mermaid
graph TD
    A[Header/Navigation Bar] --> B[Menu Lateral]
    A --> C[Formulário Principal]
    
    C --> C1[Seção Identificação]
    C --> C2[Seção Dados Abastecimento]
    C --> C3[Seção Documentação]
    C --> C4[Botões Ação]
    
    C1 --> C1A[Campo Placa Veículo]
    C1 --> C1B[Campo Motorista]
    C1 --> C1C[Campo Data/Hora]
    
    C2 --> C2A[Tipo Combustível]
    C2 --> C2B[Quantidade Litros]
    C2 --> C2C[Preço por Litro]
    C2 --> C2D[Valor Total]
    C2 --> C2E[Odômetro]
    C2 --> C2F[Posto Combustível]
    
    C3 --> C3A[Upload Nota Fiscal]
    C3 --> C3B[QR Code Scanner]
    C3 --> C3C[Área Preview Arquivo]
    
    C4 --> C4A[Botão Cancelar]
    C4 --> C4B[Botão Salvar Rascunho]
    C4 --> C4C[Botão Registrar]
```

**Descrição do Layout - Registro de Abastecimento:**
- **Seção Identificação**: Campos básicos para identificar o veículo e contexto
- **Seção Dados**: Formulário principal com todos os campos do abastecimento
- **Seção Documentação**: 
  - Área de upload com drag & drop para nota fiscal
  - Botão para scanner QR Code da nota fiscal
  - Preview do arquivo enviado
- **Botões de Ação**: Três opções (Cancelar, Salvar como Rascunho, Registrar)

### 3. Fluxo de Interação Principal

```mermaid
sequenceDiagram
    participant U as Usuário
    participant D as Dashboard
    participant F as Formulário
    participant S as Sistema
    
    U->>D: Acessa Dashboard
    D->>U: Exibe KPIs e resumos
    U->>D: Clica "Novo Abastecimento"
    D->>F: Redireciona para formulário
    F->>U: Exibe formulário vazio
    U->>F: Preenche dados obrigatórios
    U->>F: Upload da nota fiscal
    F->>U: Valida campos em tempo real
    U->>F: Clica "Registrar"
    F->>S: Envia dados para backend
    S->>F: Confirma registro
    F->>D: Redireciona para dashboard
    D->>U: Atualiza KPIs com novo registro
```

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
✅ **Wireframes Básicos** - Adicionados diagramas iniciais para Painel Geral e Registro de Abastecimento
🚧 **Em construção** - Aguardando documentação detalhada e implementação
