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

### Header
- Título: "Cadastro de Motorista"
- Breadcrumb: Início > Frota > Motoristas > Novo | Editar
- Ações rápidas: Ajuda (?), Atalhos, Exportar (quando em modo edição)

### Layout (grid responsivo)
- Coluna Esquerda: Dados pessoais e de contato
- Coluna Direita: CNH, vínculos, upload de documentos e histórico
- Rodapé fixo: barra de ações com Cancelar | Salvar

### Campos
- Nome completo (obrigatório)  
- Texto; 3–120 chars; capitalização de nomes; proibir números/símbolos indevidos
- CPF (obrigatório)  
- Máscara 000.000.000-00; validação de dígitos; único no sistema; checar duplicidade ao desfocar
- RG (obrigatório)  
- Texto/numérico conforme UF; permitir dígito/letra; 5–20 chars
- Data de nascimento (obrigatório)  
- Datepicker; idade mínima 18 anos
- Endereço (obrigatório)  
- Campos: CEP (mask 00000-000 + auto-preenchimento por API), Logradouro, Número, Complemento (opcional), Bairro, Cidade, UF (select)
- Telefone (obrigatório)  
- Máscara dinâmica: (00) 0000-0000 | (00) 00000-0000; DDI opcional
- E-mail (obrigatório)  
- Formato RFC; checagem MX opcional; evitar domínios descartáveis
- CNH  
- Categoria (select: A, B, C, D, E),  
- Validade (data obrigatória; alerta quando faltarem 60 dias),  
- Número (obrigatório; 11 dígitos),  
- UF emissora (select),  
- Foto (upload ou captura de câmera)
- Frota vinculada (obrigatório)  
- Select com busca; exibe código/nome; permite múltiplas frotas se regra do negócio permitir
- Caminhão vinculado (opcional)  
- Select com busca; exibe placa/modelo; bloqueia se motorista estiver inativo
- Status (obrigatório)  
- Ativo, Inativo, Afastado, Em treinamento

### Upload de documentos
- Tipos: CNH (frente/verso), Foto do motorista, Comprovante de residência (PDF/JPG/PNG)
- Interações: arrastar e soltar, seleção múltipla, captura por câmera (mobile)
- Regras: tamanho máximo 10 MB/arquivo; formatos permitidos; antivírus/scan; hash p/ evitar duplicatas
- Metadados por anexo: tipo, data de emissão, validade (quando aplicável), observação
- Ações: visualizar/preview, girar imagem, recortar rosto (foto), baixar, renomear, remover

### Histórico
- Timeline de eventos: criação, edição de dados sensíveis (CPF, CNH, status), mudança de frota/caminhão, uploads/remoções de documentos
- Cada evento com: data/hora, usuário, descrição, diffs relevantes

### Barra de ações (rodapé)
- Cancelar: retorna à listagem, confirma se houver alterações não salvas
- Salvar: validações síncronas e assíncronas; mostrar spinner; desabilita inputs durante processamento
- Atalhos: Alt+C (Cancelar), Alt+S (Salvar)

### Regras de validação
- Nome: 3–120 chars; somente letras, espaços e hífens
- CPF: dígitos válidos; não permitir CPF conhecido inválido (ex.: 000.000.000-00)
- RG: tamanho 5–20; caracteres alfanuméricos e .-X
- Data de nascimento: >= 18 anos; formato ISO ao salvar; timezone seguro
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
