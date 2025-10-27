# Documentação - Sistema de Automação de Abastecimento de Frotas

## Wireframe: Histórico/Consultas
Tela para consulta e auditoria de abastecimentos com foco em produtividade, rastreabilidade, exportação e análise. Inclui header, filtros avançados, busca global, tabela/lista paginada, detalhamento lateral, estados de loading/vazio/erro, acessibilidade, responsividade, atalhos e drilldown.

### Objetivos
- Encontrar rapidamente registros por período, veículo, motorista, posto, valor e status
- Permitir exportação e compartilhamento dos resultados filtrados
- Oferecer detalhamento rico sem sair da lista (side panel/modal)
- Sustentar auditoria com trilhas de alterações e validações

### Header
- Título: "Histórico / Consultas"
- Breadcrumb: Início > Abastecimento > Histórico / Consultas
- Ações no header: Exportar (.CSV/.XLSX/.PDF), Ajuda (?), Atalhos (teclado), Atualizar
- Indicadores: Contagem de resultados, Janela de datas aplicada, Última atualização

### Barra de Busca Global
- Campo único com placeholder: "Busque por placa, motorista, posto, ID de abastecimento..."
- Suporta: placa (detecção por padrão AAA#A##), CPF parcial, ID, texto livre
- Autocomplete (opcional): últimos termos, placas e postos frequentes
- Atalho: Ctrl/Cmd+K foca a busca

### Filtros Avançados
- Botões principais: Filtrar (abre/expande filtros), Limpar filtros, Aplicar
- Campos:
  - Data: intervalo [início..fim] com presets (Hoje, 7d, 30d, Este mês, Personalizado)
  - Veículo: select/autocomplete por placa, frota, identificação interna
  - Motorista: select/autocomplete (nome, CPF, status)
  - Posto: select/autocomplete (nome, CNPJ, cidade/UF)
  - Valor: faixa mínima/máxima; moeda BRL com máscara
  - Status: múltipla seleção (Aprovado, Pendente, Reprovado, Suspeito, Cancelado)
  - Método de pagamento (opcional): Cartão frota, Voucher, PIX, Dinheiro
  - Bomba/Bico (opcional): select com dependência do Posto
  - Combustível (opcional): Diesel S10, S500, Gasolina, Etanol, Arla32
- Contadores de chips: cada filtro aplicado aparece como chip descartável
- Persistência: lembrar filtros por usuário nas próximas visitas (local storage)

### Tabela/Lista Paginada
- Colunas padrão:
  - Data/Hora
  - ID
  - Placa (veículo) + Frota
  - Motorista
  - Posto (cidade/UF)
  - Produto
  - Quantidade (L)
  - Valor (R$)
  - Status (com badge e tooltip)
  - Ações (Ver detalhes, Ver rota, Marcar/sinalizar)
- Recursos:
  - Ordenação por qualquer coluna
  - Reordenação de colunas (opcional) e mostrar/ocultar colunas
  - Paginação 10/25/50/100 por página; navegação: « ‹ página › »; total de páginas
  - Scroll infinito (opcional) em mobile
  - Seleção múltipla de linhas para exportar apenas selecionadas
  - Linha com indicador visual se houver alerta/suspeita (ícone e cor de borda esquerda)

- Limpar filtros: remove todos os chips e reseta para presets; confirmação opcional
- Atualizar: recarrega dados preservando filtros; atalho Ctrl/Cmd+R (escopo local)

### Comportamentos de Estado
- Loading:
  - Shimmers/skeletons na tabela e no painel lateral
  - Ícone girando no botão Aplicar
  - Mensagem aria-live="polite": "Carregando resultados"
- Vazio:
  - Ilustração leve + texto: "Nenhum registro encontrado"
  - Dicas: ajuste datas, remova filtros restritivos, verifique termos
  - Botão: Limpar filtros
- Erro:
  - Banner vermelho com código/mensagem amigável e ID de correlação
  - Ação: Tentar novamente; detalhe técnico colapsável para suporte
  - aria-live="assertive"; foco movimentado para o banner

### Acessibilidade
- Navegação total por teclado; ordem de tab lógica; foco visível
- Tabela com roles adequados (table, rowgroup, row, columnheader, gridcell)
- Botões com rótulos e aria-pressed/expanded quando aplicável
- Chips de filtro com botão de remoção acessível (aria-label específico)
- Mensagens não dependentes apenas de cor; contrastes AA
- Atalhos documentados e desativáveis pelo usuário

### Responsividade
- Desktop: layout em 2 painéis (lista à esquerda, detalhes em side panel à direita)
- Tablet: lista full-width; painel abre como sobreposição
- Mobile: lista em cartões empilhados; filtros em sheet inferior; paginação por scroll

### Atalhos de Teclado
- Ctrl/Cmd+K: foco na busca global
- Ctrl/Cmd+F: abrir/fechar filtros
- Ctrl/Cmd+Enter: aplicar filtros
- Ctrl/Cmd+Shift+E: exportar
- ↑/↓: navegar entre linhas; Enter: abrir detalhes; Esc: fechar painel

### Exemplos de Estados (Mermaid)
```
mermaid
flowchart TB
  subgraph Header
    H1[Histórico / Consultas]
    H2[Chips de filtros aplicados]
    H3[(Exportar | Filtrar | Limpar | Atualizar)]
  end
  subgraph Lista
    L1[Tabela paginada]
    L2{Estados}
    L2 -->|Loading| L3[Shimmers]
    L2 -->|Vazio| L4[Nenhum registro]
    L2 -->|Erro| L5[Banner + Tentar novamente]
  end
  subgraph Detalhe
    D1[Side panel]
    D2[Resumo]
    D3[Validações]
    D4[Localização]
    D5[Documentos]
    D6[Auditoria]
  end
  Header --> Lista --> Detalhe
```

### Observações Técnicas
- Paginação lado-servidor; filtros e ordenação server-side para escalabilidade
- Exportação assíncrona com notificação quando pronta (para grandes volumes)
- Debounce em busca e filtros; cancelamento de requisições anteriores
- Feature flags para módulos opcionais (mapa, reordenação de colunas)
- Telemetria: tempo para primeira resposta, taxa de erro, termos de busca

---

## Wireframe: Relatórios
Tela para análise e comunicação de indicadores de abastecimento com foco em insights visuais, comparações e exportação. Cobre header, seleção de tipo, filtros avançados, área de visualização (gráficos interativos e tabela), exportar/compartilhar, ajustes de período e agrupamento, estados (loading, vazio, erro), responsividade e acessibilidade, com exemplos práticos e fluxo de navegação.

### Objetivos
- Fornecer visão consolidada de gastos, consumo, desempenho e comparativos por período
- Permitir exploração por diferentes agrupamentos (dia/semana/mês, veículo, motorista, posto, produto)
- Facilitar exportação em múltiplos formatos e compartilhamento com stakeholders
- Permitir salvar e reutilizar configurações de relatórios (favoritos)

### Header
- Título: "Relatórios"
- Breadcrumb: Início > Relatórios
- Ações: Exportar (.CSV/.XLSX/.PDF/.PNG), Compartilhar (link, e-mail), Salvar como favorito, Atualizar
- Indicadores rápidos: Período selecionado, Agrupamento atual, Filtros ativos (chips)

### Seleção de Tipo de Relatório
- Tipos disponíveis (tabs ou cards com ícone):
  - Gastos: total R$, custo por km, custo por litro
  - Consumo: km/L, L/100km, variação vs. período anterior
  - Desempenho: eficiência por veículo/motorista, abastecimentos fora do padrão
  - Comparativo: posto A vs. B, rotas, produtos, frota X vs. Y
  - Exportação: assistente para gerar datasets personalizados sem visualização
- Interações:
  - Troca de tipo mantém filtros e período, mas altera widgets/visualizações
  - Tooltip explicando cada tipo com exemplos de perguntas respondidas

### Filtros Avançados
- Controles principais: Abrir filtros, Aplicar, Limpar
- Campos:
  - Período: intervalo [início..fim] com presets (Hoje, 7d, 30d, MTD, YTD, Últimos 12 meses, Personalizado)
  - Agrupamento: Dia, Semana, Mês, Trimestre, Ano
  - Veículo: múltipla seleção por placa/frota; busca
  - Motorista: múltipla seleção; busca
  - Posto: múltipla seleção; dependente de cidade/UF opcional
  - Produto: Diesel S10, S500, Gasolina, Etanol, Arla32
  - Centro de custo/Projeto (opcional)
  - Faixa de valor e quantidade (opcional)
- Chips exibem filtros aplicados com ação de remover individualmente
- Persistência por usuário e opção de "Salvar preset de filtros"

### Área de Visualização
- Layout adaptável com grade de widgets:
  - Gráficos interativos: colunas, linhas, área, pizza/donut, barras empilhadas, heatmap de calendário
  - Interações: hover com tooltips detalhados, highlight por série, esconder/mostrar série na legenda, zoom por seleção, pan, reset zoom
  - Cross-filter: clicar em um ponto/segmento aplica filtro contextual (ex.: clicar em um posto filtra os demais widgets)
  - Drilldown: duplo clique em um ponto (ex.: mês -> semana -> dia -> lista de abastecimentos)
- Tabela de apoio:
  - Tabela sumarizada com mesmas dimensões do agrupamento
  - Colunas: período, métrica principal, variação %, top 3 contribuintes, outliers
  - Ações por linha: ver detalhes, abrir consulta no Histórico (link profundo)
- Cards de KPIs no topo: Total gasto, Média km/L, Custo por km, Nº abastecimentos; com delta vs. período anterior e setas de tendência

### Exportar e Compartilhar
- Exportar:
  - Formatos: CSV, XLSX, PDF (relatório formatado), PNG (gráfico)
  - Opções: intervalo de datas, colunas/métricas, agrupamento, separador decimal, fuso horário
  - Lote assíncrono para grandes volumes com notificação quando pronto
- Compartilhar:
  - Gerar link de visualização somente leitura com expiração
  - Enviar por e-mail com mensagem personalizada
  - Incorporar (iframe) para intranet com token scopo limitado

### Ajustes de Período e Agrupamento
- Controle fixo no topo direito: seletor de período + dropdown de agrupamento
- Botões rápidos: -1 período, +1 período, "Hoje", "Este mês", "Últimos 12M"
- Sincronização: alterações refletem imediatamente nos widgets (com debounce)

### Estados (Loading, Vazio, Erro)
- Loading: skeletons de KPIs e placeholders de gráficos; spinner em Exportar desativado
- Vazio: ilustração e dicas (tente ampliar o período, remover filtros restritos)
- Erro: banner com ID de correlação, ação Tentar novamente e detalhes técnicos recolhíveis

### Responsividade
- Desktop: 3 colunas de widgets; tabela em aba ao lado dos gráficos
- Tablet: 2 colunas; legendas colapsáveis; drag para rolar na área do gráfico
- Mobile: 1 coluna; KPIs em carrossel; filtros em sheet; tabela em acordeão

### Acessibilidade
- Gráficos com descrições textuais alternativas (aria-describedby) e tabela equivalente exportável
- Navegação por teclado em tabs de tipo e nos widgets; foco visível
- Cores com contraste AA; não depender apenas de cor para indicar série/estado
- aria-live para atualizações de métricas; labels claros em controles

### Ações e UX Detalhadas
- Hover em KPI mostra breakdown por top 3 veículos/postos
- Clique em legenda alterna visibilidade da série; shift+clique isola uma série
- Seleção em gráfico cria filtro temporário (chip) com opção de fixar
- Duplo clique no ponto abre drilldown; botão "Voltar" retorna ao nível anterior
- Botão "Abrir no Histórico" leva para a tela de Histórico com filtros sincronizados
- Salvar como favorito guarda: tipo, filtros, período, agrupamento e layout dos widgets

### Exemplos
- Gastos: barras empilhadas por produto por mês, linha de custo total; tabela com top 5 postos por gasto
- Consumo: linhas de km/L por veículo, heatmap de eficiência por dia da semana
- Desempenho: scatter de km/L vs. custo por km por motorista com linha de tendência
- Comparativo: colunas lado a lado de custo por litro por posto; boxplot por produto
- Exportação: wizard com seleção de campos, preview de colunas e validação de tamanho

### Navegação
- Tabs superiores alternam tipos de relatório
- Breadcrumb permite retornar à Home
- Links de ação levam ao Histórico/Consultas com filtros equivalentes (deep link)
- Favoritos acessíveis por dropdown no header; último favorito aberto por padrão (opcional)

### Considerações Técnicas
- Endpoints agregados com cache e paginação por dimensão (para drilldown)
- Cálculo de deltas/variações no backend para consistência; timezone-aware
- Suporte a grandes períodos via amostragem/ downsampling automático, com aviso ao usuário
- Feature flags para gráficos pesados; lazy loading dos widgets fora da viewport
