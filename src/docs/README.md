# Documentação - Sistema de Automação de Abastecimento de Frotas

## Wireframe: Cadastro de Caminhão
Tela para criar/editar cadastro de caminhões com foco em consistência de dados, rastreabilidade e usabilidade em desktop e mobile. Inclui layout, fluxos, estados, validações, mensagens, upload de documentos, histórico, responsividade e acessibilidade.

### Objetivos
- Garantir identificação única e válida de cada caminhão
- Facilitar o vínculo com frota e motorista
- Centralizar documentos e históricos do veículo
- Reduzir erros com máscaras e validações em tempo real

### Estrutura do Layout
- Header  
  - Título: "Cadastro de Caminhão"  
  - Breadcrumb: Início > Frota > Caminhões > Novo | Editar  
  - Ações rápidas: Ajuda (?) e Atalhos (teclado)
- Conteúdo principal (grid responsivo)  
  - Coluna 1: Dados principais do caminhão  
  - Coluna 2: Upload de documentos e Histórico
- Barra de ações fixa no rodapé: Cancelar | Salvar

### Campos Principais
- Placa (obrigatório)  
  - Máscara BR: AAA#A## (Mercosul) e validação por UF opcional; normalização para maiúsculas  
  - Única no sistema; checar duplicidade ao desfocar
- Renavam (obrigatório)  
  - 11 dígitos; validação de dígito verificador; apenas números
- Modelo (obrigatório)  
  - Texto; sugestões por autocomplete (base interna)
- Fabricante (obrigatório)  
  - Select/autocomplete: Volvo, Scania, Mercedes-Benz, VW, Iveco, etc.
- Ano (obrigatório)  
  - Numérico; intervalo razoável: 1970..ano atual+1
- Cor (opcional)  
  - Select/autocomplete; permitir livre texto sob permissão
- Frota (obrigatório)  
  - Select da(s) frotas cadastradas; exibe código/nome; pesquisa por texto
- Status (obrigatório)  
  - Ativo, Inativo, Em manutenção, Baixado
- Vincular Motorista (opcional)  
  - Select com busca; mostrar CPF/ID e situação (ativo/inativo)
- Observações (opcional)  
  - Multilinha, 0–1000 caracteres

### Upload de Documentos
- Tipos aceitos: CRLV (PDF/JPG/PNG), Licenciamento, Foto do veículo, Outros
- Interações: arrastar e soltar, selecionar arquivo, múltiplos anexos
- Metadados: tipo do documento, data de emissão/validade (opcional)
- Ações por item: visualizar, baixar, renomear, definir como principal, remover
- Validações: tamanho máx (ex.: 10 MB/arquivo), tipos permitidos, verificação de duplicados por hash

### Histórico do Caminhão
- Aba/box "Histórico"  
- Eventos: mudanças de status, mudança de frota, vinculação/desvinculação de motorista, edições de campos-chave

---

## Wireframe: Cadastro de Motorista
Tela para criar/editar cadastro de motoristas com foco em validação de dados legais (CPF, RG, CNH), vínculo com frota/caminhão e gestão de documentos.

### Campos e validações
- Nome completo: obrigatório; 2–120 caracteres
- CPF: obrigatório; máscara 000.000.000-00; validação de DV; único no sistema
- RG: opcional; texto 3–20; UF emissor
- Data de nascimento: obrigatório; idade mínima configurável; validação de data
- Endereço + CEP: CEP com máscara; auto-preenchimento via API; número e complemento
- E-mail: regex robusta + normalização case-insensitive; domínio válido
- Telefone: DDD válido (ANATEL); remover máscara ao salvar
- CNH: número com 11 dígitos; validação de DV quando aplicável; categoria obrigatória; validade futura
- Frota: obrigatório; referência existente
- Status: obrigatório; se Inativo/Afastado, impedir vínculo com caminhão

### Mensagens
- Sucesso: "Motorista salvo com sucesso."
- Erros gerais: "Não foi possível salvar. Verifique os campos destacados."
- Erros por campo:  
  - CPF inválido/duplicado  
  - CNH vencida ou faltando em X dias  
  - E-mail inválido  
  - Telefone inválido  
  - Campos obrigatórios ausentes
- Alertas:  
  - "CNH vence em X dias."  
  - "Motorista inativo não pode ser vinculado a caminhão."

### Estados da tela
- Inicial: formulário vazio; salvar desabilitado até mínimos obrigatórios (nome, CPF, status)
- Preenchimento: validações em tempo real; tooltips de ajuda
- Processando: loading/spinner; bloqueio de inputs
- Sucesso: banner verde + redirecionar para Detalhe do Motorista ou permanecer com toast
- Erro: banner vermelho + foco no primeiro erro
- Edição: popula dados; registra diffs no histórico

### Acessibilidade
- Labels associados (for/id); aria-describedby para dicas/erros; aria-live=assertive para erros
- Foco visível; contraste AA; navegação por teclado; rotação de foco correta ao abrir dialogs
- Campos com role apropriado (combobox/listbox); mensagens não dependentes apenas de cor

### Fluxo principal
1) Usuário clica em Novo Motorista
2) Preenche Nome, CPF, Status, e dados mínimos de contato
3) (Opcional) Preenche CNH e vínculos (frota/caminhão) e anexa documentos
4) Clica em Salvar
5) Sistema valida (CPF/CNH, formatos, obrigatórios)
6) Em sucesso, grava e redireciona ou mantém na página exibindo toast

### Responsividade
- Desktop: 2 colunas (60/40); histórico e upload na direita
- Tablet: 1–2 colunas fluídas; ações fixas no rodapé
- Mobile: 1 coluna; seções colapsáveis; botões full-width; suporte a câmera no upload

### Wireframe (Mermaid)
```
mermaid
flowchart TB
  subgraph Header
    H1[Cadastro de Motorista]
    H2[Breadcrumb: Início > Frota > Motoristas > Novo|Editar]
    H3[(Ajuda / Atalhos)]
  end
  subgraph Conteúdo
    subgraph Coluna Esquerda
      C1[Nome completo]
      C2[CPF]
      C3[RG]
      C4[Data de nascimento]
      C5[Endereço]
      C6[Telefone]
      C7[E-mail]
    end
    subgraph Coluna Direita
      D1[CNH: categoria, validade, número, UF, foto]
      D2[Frota vinculada]
      D3[Caminhão vinculado]
      D4[Status]
      D5[Upload: CNH, Foto, Comprovante]
      D6[Histórico]
    end
  end
  subgraph Ações
    A1((Cancelar))
    A2((Salvar))
  end
  Header --> Conteúdo --> Ações
```

### Regras técnicas e interações
- Máscaras: CPF, telefone, CEP; normalização e trimming; uppercase quando aplicável
- Autocomplete/busca: mínimo 2 caracteres; setas+Enter; 5–10 sugestões
- Persistência: idempotência por CPF; retries exponenciais; controle de concorrência otimista (ETag/versão)
- Auditoria: registrar alterações em CPF, CNH, status, vínculos, e dados de contato
- Telemetria: eventos de validação falha/sucesso, upload, salvar, cancel

---

## Wireframe: Cadastro de Posto
Tela para cadastrar/editar postos de abastecimento, com foco em integridade fiscal (CNPJ), localização, contatos, integração com APIs de automação e gestão documental.

### Header
- Título: "Cadastro de Posto"
- Breadcrumb: Início > Abastecimento > Postos > Novo | Editar
- Ações rápidas: Ajuda (?), Atalhos (teclado), Ver no mapa

### Campos (com validações)
- Nome do posto: obrigatório; 2–120 caracteres
- CNPJ: obrigatório; máscara 00.000.000/0000-00; validação de DV; único no sistema
- Endereço: obrigatório; rua/avenida, número, complemento; CEP com máscara 00000-000 e auto-preenchimento
- Cidade: obrigatório; autocomplete a partir do CEP/UF; lista oficial IBGE
- UF: obrigatório; select de 27 UFs; validar com cidade
- Contato: nome do contato principal; 2–120 caracteres
- E-mail: regex robusta; normalização case-insensitive; MX opcional
- Telefone: máscara (##) #####-####; DDD válido (ANATEL); armazenar sem máscara
- Gerente responsável: referência a usuário/colaborador; select com busca; exibir cargo/status
- Status: obrigatório; Ativo, Inativo, Em homologação
- Integração API/Token: chave/token secreto; gerar/renovar; exibir parcialmente ofuscado; copiar para área de transferência
  - URL do endpoint do posto (opcional) e webhook de eventos (opcional)

### Upload de documentos e fotos
- Tipos: Contrato, Alvará, Inscrição Estadual, Fotos da fachada/bombas, Outros
- Interações: arrastar e soltar; múltiplos; preview de imagens; reordenação por drag
- Metadados: tipo, data de emissão/validade, observações
- Ações por item: visualizar, baixar, renomear, definir capa, remover
- Validações: tamanho máx 10–20 MB por arquivo; tipos permitidos (PDF/JPG/PNG/HEIC); prevenção de duplicados

### Histórico de operações
- Timeline com: criação, edições relevantes (CNPJ, endereço, status), geração/renovação de token, anexos adicionados/removidos
- Cada evento com: usuário, data/hora, diff resumido, IP/event source

### Barra de ações
- Cancelar: retorna à listagem sem salvar alterações (confirmar se houver mudanças não salvas)
- Salvar: valida e persiste; em sucesso, redireciona para Detalhe do Posto ou permanece com toast

### Regras de validação
- Obrigatórios: nome, CNPJ, endereço, cidade, UF, status
- CNPJ único: bloquear duplicidade; sugerir "Ir para posto existente"
- CEP válido: se CEP preencher cidade/UF automaticamente; permitir edição manual
- E-mail e telefone: formatos e normalização
- Webhook/endpoint: validar URL https; permitir HEAD de verificação opcional
- Token: comprimento mínimo 24; apenas base64url/hex; armazenar de forma segura

### Mensagens
- Sucesso: "Posto salvo com sucesso."
- Erro geral: "Não foi possível salvar. Verifique os campos destacados."
- Erros por campo: CNPJ inválido/duplicado, CEP inválido, UF incompatível, e-mail/telefone inválidos
- Alertas: "Token expira em X dias.", "Webhook não respondeu à verificação."

### Acessibilidade
- Labels for/id; aria-describedby para dicas/erros; aria-live=assertive para erros
- Foco visível; contraste AA; navegação por teclado; áreas de upload acessíveis (role="button")
- Botões com rótulos e ícones com aria-label; mensagens não dependentes de cor

### Responsividade
- Desktop: 2 colunas (65/35). Direita com Upload e Histórico
- Tablet: 1–2 colunas; barra de ações fixa
- Mobile: 1 coluna; seções colapsáveis; botões full-width; acesso à câmera no upload

### Mapa interativo (opcional)
- Exibir mapa com pin do endereço; atualizar ao alterar CEP/endereço
- Buscar coordenadas via geocoding; permitir ajuste manual do pin
- Mostrar raio de cobertura (opcional) e postos próximos

### Fluxo principal
1) Usuário clica em Novo Posto
2) Preenche Nome, CNPJ, Endereço, Cidade, UF, Status, contatos
3) (Opcional) Configura integração (endpoint/webhook) e gera token
4) Anexa documentos e fotos
5) Clica em Salvar
6) Sistema valida, persiste e registra no histórico

### Estados da tela
- Inicial: formulário vazio; salvar desabilitado até mínimos obrigatórios
- Preenchimento: validações em tempo real; CEP auto-completa; tooltip de ajuda
- Processando: spinner; desabilitar inputs; barra de ações em loading
- Sucesso: banner/alerta verde; redirecionar ou manter com toast e highlight do ID
- Erro: banner vermelho; foco no primeiro erro; scroll automático para seção com erro
- Edição: campos preenchidos; exibir data da última atualização e opção de renovar token

### Wireframe (Mermaid)
```
mermaid
flowchart TB
  subgraph Header
    H1[Cadastro de Posto]
    H2[Breadcrumb: Início > Abastecimento > Postos > Novo|Editar]
    H3[(Ajuda / Ver no mapa / Atalhos)]
  end
  subgraph Conteúdo
    subgraph Coluna Esquerda
      C1[Nome do posto]
      C2[CNPJ]
      C3[Endereço + CEP]
      C4[Cidade]
      C5[UF]
      C6[Contato]
      C7[E-mail]
      C8[Telefone]
      C9[Gerente responsável]
      C10[Status]
      C11[Integração: Endpoint/Webhook + Token]
    end
    subgraph Coluna Direita
      D1[Upload de documentos e fotos]
      D2[Histórico de operações]
      D3[Mapa interativo (opcional)]
    end
  end
  subgraph Ações
    A1((Cancelar))
    A2((Salvar))
  end
  Header --> Conteúdo --> Ações
``
