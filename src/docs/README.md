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
  - Registro: data/hora, usuário, antes/depois
  - Links para registros relacionados (ordens de serviço, abastecimentos, multas)

### Barra de Ações
- Cancelar: retorna para listagem; se houver alterações não salvas, dialog de confirmação
- Salvar: valida todos os campos; mostra spinner, bloqueia duplo clique, feedback ao concluir

### Regras de Validação
- Placa: formato válido, único
- Renavam: 11 dígitos com DV válido
- Modelo/Fabricante: obrigatórios
- Ano: número inteiro, dentro do intervalo permitido
- Frota: obrigatório e existente
- Status: obrigatório
- Observações: limite de caracteres
- Documentos: tipo e tamanho permitidos
- Vincular Motorista: se motorista inativo, impedir seleção com mensagem

### Mensagens de Erro/Sucesso
- Erro por campo (inline) e resumo no topo
  - "Placa em formato inválido. Exemplo: ABC1D23"
  - "RENAVAM inválido"
  - "Ano fora do intervalo permitido (1970–{ano_atual+1})"
  - "Placa já cadastrada em outro veículo"
  - "Motorista selecionado está inativo"
- Sucesso
  - "Caminhão salvo com sucesso"
- Avisos
  - "Documento próximo do vencimento"

### Responsividade
- Desktop: 2 colunas; documentos e histórico na lateral direita
- Tablet: 1–2 colunas fluídas; ações fixas no rodapé
- Mobile: 1 coluna; grupos colapsáveis; botões full-width; suporte a captura de câmera no upload

### Acessibilidade (A11y)
- Labels for/id; aria-describedby para mensagens; aria-live=assertive para erros de formulário
- Foco visível; contraste AA; navegação por teclado completa
- Atalhos: Alt+C (Cancelar), Alt+S (Salvar)
- Componentes com roles adequados (combobox, listbox, dialog)

### Fluxo Básico
1) Usuário acessa "Novo Caminhão"
2) Preenche campos obrigatórios
3) (Opcional) Vínculo de motorista e anexos
4) Clica em Salvar
5) Sistema valida; em caso de erro, destaca campos e posiciona o foco no primeiro erro
6) Em sucesso, grava e redireciona para Detalhe do Caminhão

### Estados da Tela
- Inicial: formulário vazio; salvar desabilitado até mínimo de obrigatórios preenchidos
- Preenchendo: botões ativos; alertas contextuais
- Validando: botão Salvar com spinner; campos desabilitados
- Sucesso: banner verde; redirecionamento/âncora para detalhe
- Erro: banner vermelho + erros inline
- Modo Edição: carrega dados existentes, permite alterar; logs no histórico

### Wireframe (Mermaid)
```mermaid
flowchart TB
  subgraph Header
    H1[Cadastro de Caminhão]
    H2[Breadcrumb: Início > Frota > Caminhões > Novo|Editar]
    H3[(Ajuda / Atalhos)]
  end
  subgraph Conteúdo
    subgraph Coluna Esquerda
      P1[Placa]
      P2[RENAVAM]
      P3[Modelo]
      P4[Fabricante]
      P5[Ano]
      P6[Cor]
      P7[Frota]
      P8[Status]
      P9[Vincular Motorista]
      P10[Observações]
    end
    subgraph Coluna Direita
      D1[Upload de Documentos]
      D2[Histórico do Caminhão]
    end
  end
  subgraph Ações
    A1((Cancelar))
    A2((Salvar))
  end
  Header --> Conteúdo --> Ações
```

### Regras Técnicas e Interações
- Máscaras: placa, números; normalização e trimming
- Autocomplete: mínimo 2 caracteres; setas+Enter; 5–10 sugestões
- Persistência: salvar otimista com retry; idempotência por chave natural (placa)
- Auditoria: registrar antes/depois de placa, renavam, frota, status, motorista
- Telemetria: eventos de validação falha/sucesso, upload, salvar, cancel
