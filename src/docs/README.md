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

### Visualização Detalhada (Side Panel/Modal)
- Abertura por clique na linha ou botão "Ver detalhes"; mantém posição na lista
- Conteúdo:
  - Cabeçalho: ID, data/hora, status, ações rápidas (Exportar PDF, Copiar link, Imprimir)
  - Seções:
    - Resumo: veículo, motorista, posto, produto, quantidade, valor, método
    - Validações: regras aplicadas (ex.: limite diário, desvio de rota, volume vs. tanque), resultado pass/fail
    - Localização: mapa com rota opcional e distância do posto ao ponto do abastecimento
    - Documentos: fotos/tickets anexados; visualizar/baixar
    - Auditoria: histórico de alterações do registro (quem, quando, o quê)
    - Metadados: ID externo, integrações, bico/bomba, dispositivo, versão do app
  - Navegação dentro do painel: Próximo/Anterior registro
- Drilldown: links para páginas dedicadas de Veículo, Motorista e Posto; relatório completo do abastecimento

### Botões e Ações Principais
- Exportar: CSV, XLSX, PDF; opções: tudo, página atual, seleção, período; respeita filtros
- Filtrar: abre/expande área de filtros; atalho Ctrl/Cmd+F
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
```mermaid
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
