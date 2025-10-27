# Documentação - Sistema de Automação de Abastecimento de Frotas

## Wireframe: Login/Autenticação
Tela de entrada segura e acessível para todos os perfis, suportando autenticação por senha e provedores OAuth, com políticas de senha, estados de erro/bloqueio e fluxo completo de recuperação.

### Objetivos
- Permitir autenticação rápida e segura para Admin, Gerente e Motorista
- Reduzir atritos com opções "Lembrar-me" e SSO (Google/OAuth)
- Garantir conformidade (política de senha, reCAPTCHA, proteção contra brute force)
- Acessível (teclado, leitores de tela) e responsiva (mobile-first)

### Header
- Logo (alt="Automação Abastecimento") à esquerda
- Título da página: "Entrar" (h1, única instância por página)
- Link para Ajuda/FAQ e Política de Privacidade (abre em nova aba)
- Indicador de ambiente (se aplicável): Produção, Homologação

### Formulário de Login
- Campos:
  - Usuário/E-mail: input type=email, label visível "Usuário ou e-mail", autocomplete="username", placeholder opcional
  - Senha: input type=password, label "Senha", autocomplete="current-password", botão "Mostrar/ocultar" com aria-pressed
  - Seleção de Perfil: radio group ou select com opções: Administrador, Gerente, Motorista
  - Lembrar-me: checkbox, descrição: "Mantenha-me conectado neste dispositivo"
  - reCAPTCHA: widget compacto após os campos, com fallback acessível
- Botões:
  - Entrar: primário, largura cheia em mobile, desabilita durante envio
  - "Continuar com Google" (ou provedor OAuth): secundário com ícone do provedor
- Links auxiliares:
  - "Esqueci minha senha" (navega para fluxo de redefinição)
  - "Primeiro acesso? Solicitar conta" (opcional, se houver onboarding)

### Validações e Mensagens de Erro
- Validação cliente e servidor
  - Usuário/E-mail: obrigatório; formato e-mail válido quando e-mail; mensagens: "Informe seu usuário ou e-mail" / "E-mail inválido"
  - Senha: obrigatória; política ativa somente no cadastro/redefinição; no login apenas presença
  - Perfil: obrigatório (se o sistema exigir)
  - reCAPTCHA: obrigatório quando habilitado
- Erros por campo: texto abaixo do input, cor de erro, aria-describedby associando a mensagem ao campo
- Erros globais (banner):
  - "Credenciais inválidas. Verifique e tente novamente."
  - "Conta bloqueada por tentativas excessivas. Tente novamente em 15 minutos."
  - "Usuário pendente de aprovação pelo administrador."
  - "Acesso negado para o perfil selecionado."

### Política de Senha (exibida em cadastro/redefinição, referenciada aqui)
- Mínimo 8-12 caracteres, com pelo menos 3 dos 4: maiúsculas, minúsculas, números, símbolos
- Sem dados óbvios (nome, placa, CPF), sem sequências comuns (123456, qwerty)
- Expiração opcional para perfis administrativos (p.ex., 180 dias) com lembrete antecipado

### Feedback e Estados
- Loading: ao enviar, botão "Entrar" mostra spinner e texto "Entrando...", formulário desabilitado; aria-busy no form; aria-live polite para feedback
- Sucesso: redireciona para Home ou para a rota originalmente solicitada; mensagem de boas-vindas opcional
- Erro: mantém valores (exceto senha, conforme política); foca no primeiro campo inválido
- Tentativas e bloqueio: contador regressivo "Tente novamente em 14:59" com aria-live assertive
- Aprovação pendente: instrução para contatar o administrador ou verificar e-mail

### Acessibilidade
- Ordem de tab lógica; atalhos: Enter envia, Esc limpa erros não persistentes
- Labels explícitos e associação via for/id; tamanho de toque >= 44px em mobile
- Contraste AA/AAA, foco visível; não depender só de cor para estados
- Suporte completo a leitores de tela: headings, landmarks (main, form), role=alert para erros

### Responsividade
- Mobile-first: layout em coluna, inputs largura 100%
- Tablet: duas colunas opcionais (usuário/senha lado a lado), OAuth ao lado
- Desktop: card central com largura máxima ~420px; ilustração lateral opcional

### Fluxo de Redefinição de Senha
1) Esqueci minha senha (solicitação)
- Campo: e-mail ou usuário
- reCAPTCHA
- Botão: Enviar instruções
- Feedback: "Se encontrarmos sua conta, enviaremos um e-mail com instruções"
2) E-mail enviado
- Conteúdo: link com token de uso único e validade (ex.: 60 min)
- Proteção: não expõe se o e-mail existe
3) Criar nova senha
- Campos: Nova senha, Confirmar senha; indicador de força e requisitos atendidos
- Validações: match, política atendida; feedback em tempo real
- Botão: Salvar nova senha
4) Sucesso
- Mensagem: "Senha atualizada com sucesso"
- Ação: "Ir para login" e opção "Entrar automaticamente" se vier de sessão autenticada

### Estados Especiais: Bloqueio/Aprovação
- Bloqueio automático por tentativas falhas (ex.: 5 falhas em 15 min); desbloqueio por tempo ou admin
- Requerimento de aprovação: novos cadastros aguardam aprovação do admin; login bloqueado até aprovação
- 2FA opcional para Admin: após senha, prompt de código (app/OTP); lembrar dispositivo por 30 dias

### Considerações Técnicas
- Proteções: rate limiting, detecção de IP/ASN suspeitos, captcha adaptativo, proteção CSRF nos formulários
- Armazenamento seguro: senhas com hash forte (Argon2/bcrypt), cookies HttpOnly, Secure, SameSite=Lax/Strict
- Sessão: renovação de sessão após login; expiração inativa; logout explícito
- Auditoria: logs de login (sucesso/falha), motivo de bloqueio, origem (IP, user-agent)
- Internacionalização: mensagens e labels em i18n; fuso horário para timestamps de bloqueio

---

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
- Loading: skeletons de linhas; spinner em ações
- Vazio: ilustração, texto "Nenhum resultado para os filtros aplicados" e botão Limpar filtros
- Erro: banner com ID de correlação e ação Tentar novamente
- Seleção: contador de selecionados, ação de exportar/compartilhar selecionados

## Wireframe: Relatórios
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
