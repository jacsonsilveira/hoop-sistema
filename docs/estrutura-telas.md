# Sistema All-in-One Hoop
## Documento de Estrutura: Telas e Dados

Versão 3.8 | Documento de referência, não de implementação

## Sumário

| Parte | O que tem |
|---|---|
| Decisões | 114 decisões fechadas, em tabela |
| 1 | Mapa de navegação |
| 2 | Telas internas, uma a uma |
| 3 | Link de aprovação do cliente |
| 4 | Dicionário de dados |
| 5 | Status e enumerações |
| 6 | Matriz de permissões |
| 7 a 9 | Consequências das decisões e das rodadas de protótipo |
| 10 | Linguagem visual: tokens, cor, tipografia, componentes |
| 11 | Padrões de interação |
| 12 | Vocabulário único e contratos entre planejamento e demanda |
| 13 | Do documento ao código: ordem de construção, migração e telas de borda |

## Decisões estruturais fechadas

| # | Decisão | Definido |
|---|---|---|
| 1 | Armazenamento de arquivos | A plataforma armazena (modelo híbrido, ver 3.4) |
| 2 | Lead e cliente | Mesma tabela, diferenciados por `tipo` |
| 3 | Controle de tempo | Só estimativa, sem timesheet |
| 4 | Aprovação | Um link público por planejamento e contato, permanente no mês, sem login na v1 |
| 5 | Multi-tenant | Desde o schema, `organizacao_id` em todas as tabelas |
| 6 | Notificações | Só dentro da plataforma na v1 |
| 7 | Fluxo de aprovação | Não existem etapas. Cada parte tem status próprio e independente |
| 8 | Limite de armazenamento | Sem limite por arquivo ou por cliente |
| 9 | Vídeo | Reprodução nativa dentro da plataforma e do link de aprovação |
| 10 | Aprovação parcial | Permitida, item a item |
| 11 | Prazo de retorno do cliente | Só sinaliza internamente, cobrança é feita pelo time |
| 12 | Reajuste de contrato | Guarda histórico completo de valores |
| 13 | Exclusão | Não existe. Tudo é arquivado |
| 14 | Onboarding automático | Não. Criar cliente não dispara pastas nem projeto automático |
| 15 | Unidade de trabalho | A demanda, com uma ou mais **sub-demandas** quando o trabalho tem etapas |
| 16 | Responsáveis | Uma demanda tem um ou mais responsáveis, com mínimo de um |
| 17 | Nomenclatura | Nada é fixo. Demandas e checklists são livres, com modelos salvos em Configurações |
| 18 | Conclusão automática | A demanda pode ter **critério de conclusão** selecionável, que calcula o checklist sozinho |
| 19 | Planejamento | Criar o planejamento cria as demandas do mês a partir de um template |
| 20 | Comentário do cliente | Cai na publicação, nunca na demanda |
| 21 | Planejamento vivo | Editável o mês inteiro: datas remanejáveis, publicações extras, cancelamento e migração |
| 22 | Padrão de interação | Página para objeto, modal para tarefa curta, inline para clique único. Painel lateral descartado |
| 23 | Peças por formato | O formato define quantas peças e quais papéis (carrossel N slides, Reels vídeo e capa) |
| 24 | Plataforma, formato e veiculação | Três campos independentes. "Canal" deixou de existir |
| 25 | Destinos | Uma publicação vai para várias plataformas. Cada uma é um destino com registro de aprovação próprio |
| 26 | Conteúdo único | Uma publicação tem um texto e um conjunto de peças, válidos para todas as plataformas. Texto diferente vira publicação duplicada |
| 27 | Formato em família | O formato é escolhido uma vez, em família (vídeo vertical, carrossel, imagem), e cada plataforma exibe o nome nativo |
| 28 | Visualização do planejamento | Cartões, lista e calendário |
| 29 | Partes da publicação | Tema, Copy, Legenda e Criativo. Tema é opcional por cliente |
| 30 | Status por parte | Rascunho, Em aprovação, Ajuste, Aprovado. Reversível a qualquer momento |
| 31 | Quem muda o status | Cliente e Hoop. Fica registrado quem marcou, para o ok que chega por WhatsApp |
| 32 | Visibilidade | Rascunho é o que esconde do cliente. Não existe flag de "liberado" |
| 33 | Nome da publicação | É o tema. Não existe título interno |
| 34 | Ordem canônica | Tema, Copy, Legenda, Criativo, em todas as telas |
| 35 | Copy | Uma por publicação, em texto rico. Legenda continua texto puro |
| 36 | Preview | O criativo é exibido dentro de um preview realista do post |
| 37 | Histórico | Mudanças de status e mensagens na mesma linha do tempo, por publicação |
| 38 | Link do cliente | Lista e calendário, em versão celular e web |
| 39 | Shell | Menu lateral por função, sem seletor global de cliente. Busca global e sino no header |
| 40 | Projeto | Lista de demandas com status. As entidades Etapa e Frente deixam de existir |
| 41 | Demanda | Abre em modal, editável por inteiro: título, status, prazo, responsáveis, descrição, checklist, anexos e comentários |
| 42 | Checklist | Cada item tem responsável e prazo próprios. Marcar todos fecha a demanda |
| 43 | Anexos | Demandas e publicações têm anexos e links. Anexo é material de apoio interno, o cliente não vê |
| 44 | Download do criativo | Disponível no editor e na aprovação, sempre o arquivo original |
| 45 | Cartão | Um só layout de cartão para demanda, planejamento, proposta e lead |
| 46 | Kanban | Padrão de visualização em projetos, planejamentos, propostas e leads, sempre com lista como alternativa |
| 47 | Proposta | Tem escopo, condições, versões e histórico. Negociação gera versão nova, nunca sobrescreve |
| 48 | Lead | Existe sem proposta. Nem toda conversa vira orçamento |
| 49 | Calendário | Todo cartão leva ao objeto de origem: demanda, planejamento ou lançamento |
| 50 | Contrato | Abre em modal com escopo, condições, histórico e lançamentos gerados |
| 51 | Sub-demanda | Uma demanda pode ter vários checklists, cada um com nome, responsáveis, prazo e critério |
| 52 | Etapas do planejamento | Briefing, Conteúdo, Design, Publicação (e Tráfego quando houver), dentro de uma demanda só |
| 53 | Natureza do checklist | Espelho da aprovação (travado) ou controle interno (editável). Declarada em cada sub-demanda |
| 54 | Design system | Tokens do DS da Hoop: Poppins, roxo #7A5AF8, Phosphor, grid de 4px, raio de 8px |
| 55 | Cor de estado | Seis tons com significado único: cinza, roxo, azul, amarelo, vermelho, verde |
| 56 | Cor de número | Só colore quando existe limite declarado. Valor solto fica neutro |
| 57 | Cor no calendário | A cor diz o estado, o ícone diz a categoria |
| 58 | Tipografia | Sete papéis nomeados, de 26px (número) a 11px (rótulo micro) |
| 59 | Largura | Conteúdo ocupa a tela. Só texto corrido tem limite de 72 caracteres por linha |
| 60 | Tema | Claro por padrão, com alternador. Escuro fica para a v2 com rampa própria |
| 61 | Avatar | Toda pessoa aparece com avatar ao lado do nome, com cor fixa por pessoa |
| 62 | Registro | Comentário e log são blocos separados, nas telas internas e no link do cliente |
| 63 | Ordem | Comentário e log sempre do mais novo para o mais velho. Campo de escrever no topo |
| 64 | Log | Recolhido por padrão. Registra status, prazo, responsável, checklist, anexo e checklist criado ou removido |
| 65 | Carimbo | Todo registro tem data e hora completas, em UTC no servidor |
| 66 | Prazo em template | Template guarda número. A criação calcula a data absoluta |
| 67 | Planejamento | Produzido no mês anterior. Só a publicação acontece no mês de referência |
| 68 | Perfil | Cada pessoa edita foto, @usuario, contato e preferências. Cargo é da gestão |
| 69 | Vocabulário | Um termo por conceito. "Frente do mês" é sub-demanda; "entregável" é publicação |
| 70 | Estados | Demanda e parte têm máquinas separadas e não se fundem |
| 71 | Espelhamento | A parte é a fonte da verdade; o item de checklist é reflexo |
| 72 | Desatualizada | Não é estado, é sinalizador sobre `aprovado`, e reabre o item espelhado |
| 73 | Volume | Partes exigidas e publicações contratadas vêm do contrato, não do template |
| 74 | Máscara | Valor, telefone, CEP, CPF e CNPJ têm máscara e validação local |
| 75 | CNPJ | Formato alfanumérico de 2026: 12 posições alfanuméricas e 2 dígitos numéricos |
| 76 | Consulta externa | CNPJ e CEP preenchem cadastro; campo segue editável e falha não bloqueia |
| 77 | Fluidez | Salvamento automático, desfazer no lugar de confirmar, edição em lote e paleta de comandos |
| 78 | Automações | Sete, ligáveis em Configurações, cada uma com gatilho declarado |
| 79 | Veiculação | Orgânico e impulsionado são sinalizadores independentes; pelo menos um ligado |
| 80 | Verba | Publicação impulsionada tem verba própria, somada no escopo e lançada no financeiro |
| 81 | Ordenação | Toda tabela ordena por coluna; kanban não, porque a ordem ali é o status |
| 82 | Memória de filtro | Filtro é lembrado por tela, nunca global |
| 83 | Registro de origem | Toda edição registra de onde veio: lista, kanban, lote ou modal |
| 84 | Editor no shell | O planejamento abre dentro do app e devolve o resumo que espelha o checklist |
| 85 | Recuperação de senha | Resposta idêntica existindo ou não a conta, para não vazar carteira de clientes |
| 86 | Primeiro acesso | Só nome, usuário e senha. O resto do perfil fica para depois |
| 87 | Versão de arquivo | Substituir cria versão nova; restaurar gera outra por cima. Nada é sobrescrito |
| 88 | Link de aprovação | Vale enquanto o planejamento está aberto; expirado oferece pedir um novo |
| 89 | Permissão por ação | 36 ações contra 5 perfis, com quatro níveis, e não só permissão por tela |
| 90 | Escopo próprio | "O que é seu" filtra por responsabilidade, na mesma tela |
| 91 | Margem | Custo e margem só para Administrador e Financeiro |
| 92 | Prazo e responsável | Operador não altera; pede ao gestor |
| 93 | Último administrador | O sistema recusa deixar a organização sem administrador |
| 94 | Destinatário de notificação | Responsáveis pela demanda e pela sub-demanda. Menção só para quem foi mencionado |
| 95 | Relatório | Passa por revisão antes do envio, e o envio da v1 é manual |
| 96 | Receita bruta e líquida | Todo lançamento de receita guarda taxa, alíquota e imposto; o resultado usa o líquido |
| 97 | Repasse | Custo de fornecedor amarrado à receita que ele entrega |
| 98 | Retirada de sócio | Natureza própria, fora do custo operacional |
| 99 | Nota fiscal | Registrada no sistema, emitida no portal; o sistema confere nota contra receita |
| 100 | Caixa | Saldo mensal em paralelo à competência |
| 101 | Custo da pessoa | Custo mensal e vínculo, para derivar custo por hora |
| 102 | Precificação por hora | Serviço guarda horas por área e valor hora |
| 103 | Rentabilidade por cliente | Margem depois do trabalho feito, diferente da margem da proposta |
| 104 | Alíquota por competência | Derivada da guia (valor ÷ receita bruta do mês) e congelada no lançamento |
| 105 | Guia do Simples | Lançada como fato; cria a despesa e dispara a conferência contra o provisionado |
| 105b | Sem cálculo tributário | O sistema não interpreta anexo, fator R nem faixa. RBT12 é só informação |
| 109 | Conversa ao lado | Comentários e histórico em coluna própria à direita nos modais, empilhados no celular |
| 110 | Situação na lista | Receitas, despesas e recorrentes mudam de situação sem abrir o modal |
| 111 | Busca sem acento | Comparação normalizada em todo o sistema |
| 112 | Link do cliente | O modal só gera e copia; quais partes o cliente aprova é configuração do cliente |
| 113 | Dados reais | Protótipo alimentado com a operação de 2026, conferido contra a planilha centavo a centavo |
| 106 | Período do financeiro | Um seletor no topo vale para todas as abas: mês, 3, 6, ano, ano anterior e intervalo |
| 107 | Comparação | Todo número do financeiro mostra a variação contra o período anterior de mesmo tamanho |
| 108 | Fechamento de mês | Mês fechado vira só leitura; mudança depois exige estorno |

---

## Como ler este documento

Cada tela está descrita em 5 blocos:

- **Objetivo**: a pergunta que a tela responde ou a ação que ela permite
- **Dados exibidos**: o que aparece na tela
- **Campos de entrada**: o que o usuário preenche (quando aplicável)
- **Ações**: o que dá pra fazer a partir dali
- **Depende de**: quais telas ou entidades precisam existir antes

No final tem o **Dicionário de Dados** (todas as entidades e seus campos) e a **Matriz de Permissões**.

---

# PARTE 1: MAPA DE NAVEGAÇÃO

## Aplicação interna (time Hoop)

Menu lateral fixo:

```
Dashboard
CRM
  Leads
  Propostas
Clientes
  Lista de clientes
  Contratos
Demandas
  Meu trabalho
  Projetos
  Planejamentos (fees)
Calendário
Arquivos
Financeiro
  Visão geral
  Lançamentos
  Projeção
  Relatórios
Time
Notificações (ícone no topo, não no menu)
Configurações
  Organização
  Usuários e permissões
  Templates de projeto
  Catálogo de serviços
  Integrações
```

## Aplicação externa (aprovação do cliente)

Sem menu e sem login na v1. O cliente entra por um link com token que aponta direto para o que ele precisa aprovar:

```
/aprovacao/{token}   → planejamento ou entregável específico
```

**Total: 29 telas internas + 3 telas externas.** A demanda de planejamento não tem tela própria: ela abre o editor de planejamento (4.5) com o foco de quem clicou.

---

# PARTE 2: TELAS INTERNAS

## 0. Shell da aplicação

Tudo que vem depois mora dentro deste esqueleto.

**Menu lateral**, agrupado por função: Dashboard e Meu trabalho no topo; Comercial (Leads, Propostas); Operação (Clientes, Planejamentos, Projetos, Calendário, Arquivos); Gestão (Financeiro, Time, Configurações). Itens com pendência mostram contador. No celular vira barra horizontal rolável.

**Sem seletor global de cliente.** Na agência a mesma pessoa atravessa cinco clientes numa manhã. Um filtro global obrigaria a trocar de contexto o tempo todo e esconderia metade do trabalho. O contexto de cliente vem da tela do cliente; as telas de trabalho mostram tudo e filtram por dentro.

**Header:** título da tela, **busca global** (cliente, publicação, demanda, arquivo) com atalho de teclado, botão Criar, sino de notificações com contador e painel, e avatar.

**Notificações** são o único canal de aviso da v1, e valem só para dentro: o cliente continua sendo avisado manualmente por WhatsApp.

---


---

## 1. DASHBOARD

### 1.1 Dashboard geral

**Objetivo:** responder "como está a Hoop hoje" em 10 segundos, antes de qualquer clique.

**Dados exibidos:**

Bloco Financeiro (cards)
- Receita realizada no mês (R$)
- Receita prevista do mês (R$, vinda dos contratos ativos)
- Despesas do mês (fixas + variáveis)
- Resultado do mês (previsto vs realizado)
- MRR atual (soma dos contratos de recorrência ativos)

Bloco Comercial
- Leads em aberto (quantidade)
- Propostas enviadas aguardando resposta (quantidade e R$ total)
- Taxa de conversão dos últimos 90 dias (%)

Bloco Produção
- Demandas atrasadas (quantidade, com destaque vermelho)
- Demandas com deadline nos próximos 7 dias
- Entregáveis aguardando aprovação do cliente (quantidade + há quantos dias)
- Entregáveis reprovados ou com ajuste pendente

Bloco Time
- Carga por pessoa (nome + quantidade de demandas abertas + quantas atrasadas)

Bloco Atividade
- Últimas 10 movimentações do sistema (quem fez o quê, quando)

**Ações:**
- Clicar em qualquer número leva para a tela filtrada correspondente
- Filtro global de período (mês atual, trimestre, ano)

**Depende de:** praticamente tudo. É a última tela a ser construída.

---

## 2. CRM

### 2.1 Leads

**Duas visualizações:** kanban (padrão) e lista.

**Kanban:** Novo, Contato feito, Reunião marcada, Proposta enviada, Em negociação, Ganho, Perdido. Cartões arrastáveis, com contagem e soma por coluna.

**Cartão** (mesmo layout de todos os cartões do sistema): empresa no título, contato e interesse no subtítulo, selos de valor estimado e dias parado, responsável à direita. Lead parado há mais de dez dias ganha marcação, porque sem isso o funil vira cemitério.

**Lista:** tabela com empresa, contato, interesse, valor estimado, estágio e dias parado.

**Lead existe sem proposta.** Valor estimado pode ficar em branco, e o cadastro não exige orçamento: nem toda conversa vira proposta, e forçar isso enche o sistema de orçamento fantasma.

---

### 2.2 Detalhe do lead (modal)

Empresa editável no cabeçalho, contato, interesse, estágio, valor estimado, responsável e origem. Abaixo, as propostas vinculadas (com link para cada uma) e o histórico de interações.

**Ações:** salvar, criar proposta já vinculada, marcar como perdido com motivo obrigatório.

---

### 2.3 Propostas

**Duas visualizações:** kanban (padrão) e lista.

**Kanban:** Rascunho, Enviada, Em negociação, Aprovada, Recusada. Arrastar muda a situação e **entra no histórico da proposta**.

**Cartão:** cliente no título, serviço e número com a versão no subtítulo, selos de valor, margem e parcelas. Margem abaixo de 40% ganha marcação.

**Lista:** número com versão e data, cliente, serviço, valor com margem, condição (parcelas e prazo) e situação com dias parado.

**Por que a margem aparece na listagem:** ver "R$ 2.800 · 40%" ao lado de "R$ 34.000 · 48%" muda a conversa sobre qual proposta perseguir. Hoje esse número mora na cabeça do time ou numa planilha.

---

### 2.4 Detalhe da proposta (modal)

**Identificação:** número, versão, cliente, data de criação, situação.

**Condições, todas editáveis:** situação, responsável, validade em dias, tempo de contrato, número de parcelas e valor total.

**Descrição do escopo:** texto livre. É o que o cliente lê e o que evita a discussão de "não era isso que estava combinado".

**Itens:** serviço, quantidade e valor, com total.

**Custos** (nunca aparecem para o cliente): fixos e variáveis, com custo total. Calculados na tela: margem bruta em reais, margem em percentual e **valor por parcela**.

**Histórico e versões.** Toda mudança de valor, parcela ou situação entra no log com autor e data. "Gerar nova versão" incrementa a versão e registra o que mudou, sem apagar a anterior.

**Por que versão importa:** negociação muda escopo, valor e parcela várias vezes. Sem versão, a proposta vira um documento que ninguém sabe qual é o vigente, e o cliente lembra do número antigo.

**Ações:** salvar, gerar nova versão, exportar PDF, marcar como aprovada (dispara a criação do contrato e do projeto a partir do template do serviço).

---

## 3. CLIENTES

### 3.1 Lista de clientes

**Objetivo:** visão geral da carteira.

**Dados exibidos:**
Tabela ou cards com: logo, nome da empresa, tipo de relação (recorrência, projeto, ambos, inativo), valor mensal recorrente (R$), responsável pela conta, quantidade de projetos ativos, demandas atrasadas, status de aprovação pendente (indicador), cliente desde (data)

**Totalizadores:** total de clientes ativos, MRR total, ticket médio

**Filtros:** status, tipo de relação, responsável, segmento

**Ações:** novo cliente, abrir detalhe, arquivar

---

### 3.2 Detalhe do cliente

**Objetivo:** ser a página de referência da conta. Tudo que existe sobre aquele cliente.

Organizada em abas:

**Aba Visão geral**
- Logo, nome fantasia, razão social, CNPJ, segmento, porte
- Endereço completo, telefone, email geral, site, redes sociais (Instagram, LinkedIn, Facebook, TikTok, YouTube)
- Responsável pela conta (usuário Hoop)
- Cliente desde, status
- Resumo: contratos ativos, MRR, projetos em andamento, saldo financeiro (a receber)
- Observações e informações estratégicas (tom de voz, pessoa de contato preferida, restrições)

**Aba Contatos**
- Lista: nome, cargo, telefone, email, WhatsApp, aniversário, é aprovador? (sim/não), tem acesso ao portal? (sim/não)
- Ação: convidar para o portal do cliente

**Aba Contratos**
- Lista de contratos: número, tipo, escopo resumido, valor, vigência (início e fim), status (ativo, encerrado, suspenso, em renovação), data de reajuste
- Ação: novo contrato, abrir detalhe (3.3)

**Aba Projetos e planejamentos**
- Lista de projetos: nome, tipo, status, fase atual, responsável, prazo, % concluído
- Lista de planejamentos mensais (se for fee): mês, status, publicações totais, aprovadas, pendentes

**Aba Financeiro**
- Lançamentos vinculados ao cliente: data, descrição, valor, status (previsto, faturado, pago, atrasado)
- Totais: faturado no ano, a receber, inadimplência

**Aba Arquivos**
- Biblioteca do cliente (ver 3.4): pastas, arquivos, miniaturas, espaço usado
- Links externos do Drive para material bruto

**Aba Histórico**
- Log de tudo que aconteceu na conta: mudanças, aprovações, reuniões registradas, quem e quando

**Ações:** editar, arquivar, criar projeto, criar contrato, criar lançamento

---

### 3.3 Detalhe do contrato (modal)

Abre a partir da aba Contratos do cliente.

**Cabeçalho:** número, cliente, tipo e situação.

**Condições, editáveis:** tipo, valor, vigência, dia de vencimento, índice e data de reajuste, renovação automática.

**Escopo contratado:** texto livre, herdado da proposta.

**Histórico do contrato:** assinatura, reajustes, aditivos, entrada em renovação e encerramento. Reajuste e aditivo **não sobrescrevem o valor**: geram registro e recalculam apenas os lançamentos futuros ainda não faturados.

**Lançamentos gerados:** a lista de parcelas ou mensalidades com situação.

**Documento:** o PDF assinado, na biblioteca do cliente, pasta Contratos.

**Ações:** salvar, aplicar reajuste, renovar (cria contrato novo herdando os dados), encerrar (com motivo e data).

---

### 3.4 Biblioteca de arquivos

**Objetivo:** ser o repositório da plataforma. Como a decisão foi armazenar os arquivos aqui, esta tela substitui a navegação no Drive no dia a dia.

**Modelo híbrido recomendado (ver nota estratégica na Parte 7):**
- A plataforma armazena: criativos aprovados e em aprovação, peças de publicação, entregáveis finais, documentos, contratos assinados, briefings. Ou seja, tudo que precisa aparecer dentro de uma tela ou de um link de aprovação.
- Fica fora: material bruto de produção (raw de vídeo, PSD e AI pesados, sessões de foto completas). Esses continuam no Drive e entram como link de referência.

**Dados exibidos:**
- Navegação por cliente e por pasta. As pastas são criadas manualmente ou aplicando um modelo de estrutura em um clique (Marca, Contratos, Briefings, Entregas, Publicações). Nada é criado automaticamente na criação do cliente.
- Grade ou lista com: miniatura, nome, tipo, tamanho, duração (vídeo), versão, enviado por, data, vínculo (projeto, publicação ou contrato)
- Busca por nome, cliente, tipo e período
- Indicador de espaço usado por cliente e total da organização (sem limite imposto, só visibilidade)

**Ações:** upload (arrastar), aplicar modelo de estrutura de pastas, criar pasta, renomear, mover, baixar, gerar link de compartilhamento, substituir por nova versão, arquivar

**Regras técnicas:**
- Sem limite de tamanho por arquivo nem cota por cliente. Upload em partes (chunked) é obrigatório, senão arquivo grande falha no meio.
- Imagem: geração automática de miniatura e versão otimizada para exibição
- Vídeo: processamento em segundo plano gerando versão web (MP4/H.264) e frame de capa. O arquivo original é preservado intacto.
- Arquivo em processamento aparece na tela com indicador, não trava o fluxo
- Nada é excluído de fato, só arquivado

---

## 4. DEMANDAS

### 4.1 Meu trabalho

**Objetivo:** a tela que cada pessoa abre de manhã.

**Unidade listada:** a demanda, um card por demanda. O card é **personalizado para quem olha**: mostra o prazo e o progresso da parte que é daquela pessoa, e é esse prazo que define em qual bloco de urgência ele cai. A mesma demanda aparece com prazos diferentes na tela de cada um, sem duplicar nada.

**Agrupamento por urgência:** Atrasadas, Hoje, Esta semana, Depois, Aguardando terceiros.

**Card:** título, cliente, contexto (projeto ou planejamento), prazo, progresso, e em linha discreta quem mais está na demanda. Demanda atrasada ganha marca na borda.

**Visualizações:** lista agrupada e kanban por urgência.

**Ações:** abrir (leva ao modal da demanda ou ao editor de planejamento, conforme a origem), marcar concluída, reatribuir, criar demanda avulsa.

---

### 4.2 Lista de projetos

Tabela com: nome, tipo, cliente, demanda atual (a que está em Fazendo), prazo final, responsável, e a contagem de demandas concluídas. Filtros por cliente, tipo, responsável e situação.

---

### 4.3 Detalhe do projeto

**Objetivo:** conduzir o projeto do início ao fim. O projeto é uma **lista de demandas**, vinda do template e editável, sem camada de etapa.

**Cabeçalho:** cliente, responsável, período, andamento (X de Y demandas) e contrato vinculado.

**Duas visualizações da mesma lista, ambas por status:**
- **Kanban** (padrão): Backlog, A fazer, Fazendo, Com o cliente, Feito. Cartões arrastáveis entre colunas, com contagem de checklist, prazo, anexos, comentários e responsáveis. Cada coluna tem "+ cartão".
- **Lista**: as mesmas demandas agrupadas pelos mesmos status.

**Personalização:** as demandas nascem do template do tipo de projeto (identidade visual, audiovisual, e assim por diante) e podem ser renomeadas, reordenadas, removidas, e novas podem ser criadas. Demanda criada fora do template aparece marcada como avulsa.

**Automatismos:**
- Marcar todos os itens do checklist move a demanda para Feito
- Desmarcar um item traz de volta para Fazendo
- Arrastar para Feito marca o checklist inteiro

**Abaixo:** briefing do projeto.

---

### 4.4 Detalhe da demanda (modal)

Vale para demanda de projeto e de planejamento. A diferença está no critério dos checklists, não na tela.

**Cabeçalho:** título editável, cliente, contexto (projeto ou planejamento), fechar.

**Campos:** status, prazo de entrega, responsáveis (um ou mais, chips, mínimo de um), descrição livre.

**Sub-demandas (etapas):** a demanda tem um checklist simples ou vários. Cada sub-demanda carrega nome, responsáveis, prazo, critério de conclusão, progresso e o seu próprio checklist. A sub-demanda de quem está olhando aparece marcada.

**Conversão automática nos dois sentidos:** demanda com checklist simples que ganha um segundo vira demanda com etapas, e o checklist antigo vira a primeira. Se sobrar uma só, ela volta a ser checklist simples e a palavra "etapa" some da tela. O conceito só aparece quando existe mais de um.

**Checklist:** cada item tem texto, responsável e prazo próprios, e caixa de conclusão.

**Anexos e links, comentários, e as ações** (salvar, cancelar, duplicar, remover) seguem como antes.

**Fechamento automático:** a demanda só vai para Feito quando todos os checklists fecham, não quando o primeiro termina.

---

### 4.4.1 As três naturezas de checklist

Declarada em cada sub-demanda, e visível embaixo do nome dela.

| Natureza | Critério | Editável | Exemplo |
|---|---|---|---|
| Interno da equipe | manual | sim | Briefing: agendar reunião, realizar, registrar ideias, fechar temas |
| Interno por publicação | manual | sim | Publicação: um item por post, marcado por quem publicou |
| Espelho da aprovação | legenda aprovada, criativo aprovado | não | Conteúdo e Design: um item por publicação, fecha quando o cliente aprova |

**Por que o espelho é travado:** se o designer pudesse marcar a caixinha na mão, o painel e a realidade divergiriam em uma semana. O checklist espelho é medidor, não interruptor. A aprovação mora na publicação, em um lugar só.

**Por que o interno existe:** briefing é uma reunião com o cliente que gera as pautas, acontece antes de qualquer publicação existir e não tem item por post. Publicação é o oposto: tem um item por post, mas não depende do cliente, é controle de quem colocou no ar.

**Nota de evolução:** quando entrar a integração com a Meta, a etapa de publicação deixa de ser manual e passa a fechar sozinha. É a mesma sub-demanda mudando de natureza, e o campo de critério já aguenta isso sem mexer no modelo.

---

### 4.5 Editor de planejamento (fee recorrente)

**Objetivo:** substituir o Google Apresentações. É a tela que resolve sua dor principal, e é onde o conteúdo é escrito, não só organizado.

**O que acontece ao criar o planejamento:**
1. Você escolhe um template de planejamento (ou começa em branco)
2. Nasce o planejamento do mês
3. Nasce **uma demanda** ("Planejamento de setembro"), com as sub-demandas do template
4. As sub-demandas padrão são as etapas do mês: **Briefing, Conteúdo, Design, Publicação**, e **Tráfego** nos fees com mídia. Todas editáveis ali mesmo: renomear, trocar responsáveis, mudar prazo, adicionar ou remover.
5. Cada publicação criada vira um item do checklist das sub-demandas que trabalham por publicação. Briefing não, porque é reunião e acontece antes de existir publicação.

**Dados exibidos:**

Cabeçalho
- Cliente, mês de referência, tipo (orgânico, campanha paga, ambos)
- Fluxo de aprovação do cliente: etapa única ou duas etapas
- Status geral: em produção, enviado para aprovação, em ajustes, aprovado, publicado
- Etapas do mês: nome, responsáveis, prazo, progresso (ex: Conteúdo, Julia, 05/09, 6 de 12)
- Volume contratado x publicações do mês x entregas somando as plataformas (ex: 12 contratadas, 12 publicações, 20 entregas), com destaque quando excede e quebra por plataforma
- Contadores separados por trilha: conteúdo (aprovadas / em ajuste / pendentes) e criativo (idem)
- Data de envio ao cliente, prazo de retorno
- Link do cliente: um só para o mês, com token, validade, quantidade de acessos, último acesso, botão copiar e revogar

**Planejamento é vivo, não é um documento fechado.** Data de publicação muda, entra post que ninguém previu, cliente pede uma peça extra no dia 20. O editor precisa aguentar tudo isso depois do envio ao cliente, sem quebrar o que já foi aprovado:

- **Datas remanejáveis a qualquer momento**, inclusive depois da aprovação. Mudar a data não invalida a aprovação de conteúdo nem de criativo, só registra no histórico ("12/09 para 15/09, alterado por Julia"). Só alteração de conteúdo ou de peça reabre aprovação.
- **Publicações avulsas.** Toda publicação carrega uma origem: planejada ou extra. Publicação criada depois do envio entra como extra, já marcada como pendente de aprovação, e aparece no link que o cliente já tem. Não precisa gerar link novo.
- **O denominador se mexe.** Entrar uma publicação nova muda o progresso de 8 de 12 para 8 de 13 nas etapas que trabalham por publicação, e reabre a que já estava concluída. Isso é comportamento correto, não bug: se entrou trabalho, a demanda não está mais pronta.
- **Cancelar em vez de excluir.** Publicação cancelada sai da contagem e do link do cliente, mas continua no histórico com motivo.
- **Mover para o mês seguinte.** Publicação não publicada pode migrar para outro planejamento, levando junto legenda, peça e histórico de aprovação.
- **Controle de escopo.** O cabeçalho mostra o volume contratado e o volume real do mês (ex: 12 contratadas, 15 no mês). É a informação que hoje some e vira trabalho não cobrado.
- **Log de alterações do planejamento**, visível ao time: o que mudou, quem mudou, quando. Serve para a conversa com o cliente quando ele diz que nunca pediu aquilo.

**Visualização:** cartões ou lista, sem filtro por natureza de trabalho. O conteúdo é sempre completo nas duas: peça e texto aparecem juntos, porque quem escreve precisa ver a arte e quem faz arte precisa ler o texto. Cartões servem para julgar a peça, lista serve para escanear datas, plataformas e o que está faltando. O calendário é a terceira visualização, quando o que importa é a distribuição no mês.

Filtrar a grade por natureza de trabalho foi testado no protótipo e descartado: era o conceito interno vazando para a interface, e o próprio cartão já resolve mostrando as duas coisas.

**Regra do checklist calculado:** a demanda do planejamento tem um critério de conclusão, e cada item (uma publicação) fecha sozinho quando esse critério é satisfeito: legenda preenchida, peça anexada, aprovado pelo cliente. Ninguém marca caixinha à mão, então ninguém consegue mentir para o sistema. Demanda de projeto usa critério manual e o checklist funciona do jeito comum.

Grade de publicações
- Cartão: miniatura e contagem de peças, data, plataformas, formatos, título interno, trecho da legenda, estado de aprovação por destino, selos de exceção (texto por plataforma, patrocinado, extra, cancelada)
- Linha da lista: data, miniatura reduzida, título com plataformas e formatos, trecho da legenda, estados e selos alinhados à direita

Painel lateral de resumo
- Progresso por etapa do mês (briefing, conteúdo, design, tráfego, publicação)
- Publicações que voltaram com ajuste do cliente
- Prazos estourados

**Ações:**
- Criar publicação (abre o card já em edição, escrevendo direto ali)
- Duplicar publicação
- Remanejar data (arrastar no calendário ou editar no card)
- Cancelar publicação (com motivo)
- Mover publicação para outro planejamento
- Copiar o link do cliente e liberar os itens prontos (não existe envio por etapa: o que está pronto aparece no link que ele já tem)
- Segurar um item fora do link enquanto não estiver pronto
- Marcar publicação como publicada
- Exportar planejamento em PDF

**Depende de:** 3.3 (contrato de recorrência), 4.6

---

### 4.6 Detalhe da publicação (modal)

**Objetivo:** criar e editar a publicação inteira sem sair do mês. Abre em modal de duas colunas, com navegação entre publicações no cabeçalho.

**Coluna esquerda: controle**
- **Status de aprovação** (no topo, é a informação mais importante da tela): uma linha por parte, com seletor de quatro estados e, abaixo, quem marcou e quando
- Formato (família) e plataformas (chips, incompatíveis desabilitadas)
- Data e veiculação

**Coluna direita: criação, na ordem canônica**
1. **Tema** · a pauta. É também o nome da publicação em todas as telas. Não existe título interno.
2. **Copy** · texto das peças e roteiro de cenas. Editor com negrito, itálico e lista. Sem limite de caracteres, porque aqui cabe o roteiro de um carrossel de dez peças ou de um vídeo de três cenas.
3. **Legenda** · texto do post, com contador do menor limite entre as plataformas selecionadas. Texto puro, porque as plataformas não aceitam formatação.
4. **Criativo** · lista de peças anexadas uma a uma, reordenáveis, com remoção. Sem slots vazios: peça só existe depois de anexada.
5. **Histórico e conversa** · a linha do tempo da publicação, com campo de resposta ao cliente

**Ações:** salvar, salvar e ir para a próxima, duplicar, mover para o mês seguinte, cancelar

---

### 4.6.1 As quatro partes e seus status

Toda publicação tem quatro partes. Tema é opcional por cliente, configurado no link de aprovação.

| Parte | O que é | Existe quando |
|---|---|---|
| Tema | a pauta, e o nome da publicação | o campo tem texto |
| Copy | texto das peças e roteiro | o campo tem texto |
| Legenda | texto do post | o campo tem texto |
| Criativo | as peças | há pelo menos uma peça anexada |

**Status possíveis, iguais para as quatro:**

| Status | Significa | Quem vê |
|---|---|---|
| Rascunho | a Hoop ainda está produzindo | só a Hoop |
| Em aprovação | está com o cliente | os dois |
| Ajuste | o cliente pediu mudança | os dois |
| Aprovado | fechado | os dois |

**Regras:**
- Os quatro status são independentes. Não existe ordem obrigatória, bloqueio ou etapa: o cliente pode aprovar a legenda antes de a arte existir.
- Reversível sempre. Aprovado pode voltar para ajuste, e vice-versa, por qualquer um dos dois lados.
- **Rascunho é o controle de visibilidade.** A publicação aparece no link do cliente quando ao menos uma parte sai do rascunho, e ele vê apenas as partes que saíram.
- **Os dois lados mudam o status.** O cliente pelo link, a Hoop pelo modal. O registro guarda quem marcou, o que resolve o ok que chega por WhatsApp sem obrigar o cliente a entrar na plataforma.
- Escolher Ajuste pede o motivo, que entra no histórico como mensagem.
- Cada parte guarda a versão aprovada. Alterar a parte depois de aprovada marca "mudou depois de aprovado", em vez de apagar a aprovação: o cliente vê que aquilo já teve um ok e foi mexido.
- O status da publicação é calculado: Aprovado quando todas as partes com conteúdo estão aprovadas, Ajuste pedido quando qualquer uma está em ajuste, e a contagem parcial no resto do tempo.

---

### 4.6.2 Histórico e conversa

Uma linha do tempo por publicação, misturando dois tipos de evento em ordem cronológica:
- **Mudança de status**: "Copy: Ajuste, Renata Alves, 06/09"
- **Mensagem**: texto livre, do cliente ou da Hoop, com autor e data

Fica no rodapé das duas telas (modal do editor e link do cliente), com campo de escrita em cada uma. É a conversa da publicação, não da rodada de aprovação: continua existindo depois de o ajuste ser feito e o item reaprovado.

**Por que juntos:** separados, o motivo do ajuste ficava longe da decisão. Juntos, a leitura é "voltou para ajuste porque ela pediu isso, e a Julia respondeu aquilo".

---

### 4.6.3 Peças e formatos

O formato é escolhido uma vez, em família, e cada plataforma publica com o nome dela.

| Família | Plataformas | Nome nativo | Peças |
|---|---|---|---|
| Imagem | Instagram, Facebook, LinkedIn, YouTube | Imagem no feed / Imagem / Imagem / Post da comunidade | 1 |
| Carrossel | Instagram, Facebook, LinkedIn, TikTok | Carrossel / Carrossel / Carrossel em PDF / Carrossel de fotos | 2 ou mais, ordem importa |
| Story | Instagram, Facebook | Stories | 1 |
| Vídeo vertical | Instagram, TikTok, YouTube, Facebook | Reels / Vídeo / Shorts / Reels | vídeo + capa |
| Vídeo horizontal | YouTube, Facebook, LinkedIn | Vídeo | vídeo + capa |
| Artigo | LinkedIn | Artigo | imagem de capa |

- Peças são anexadas uma a uma e reordenáveis. Não existem slots vazios esperando arquivo.
- Trocar o formato descarta as peças e remove plataformas incompatíveis, com aviso.
- Uma publicação tem um texto e um conjunto de peças para todas as plataformas. Texto diferente vira publicação duplicada.

---

### 4.7 Lista de planejamentos (fees)

**Objetivo:** visão macro de todos os fees do mês.

**Dados exibidos:**
Tabela: cliente, mês, tipo, status, publicações totais, aprovadas, pendentes, data de envio, dias aguardando retorno do cliente, responsável

**Filtros:** mês, cliente, status

**Ações:** criar planejamento do mês, duplicar do mês anterior, abrir

---

## 5. CALENDÁRIO

### 5.1 Calendário geral

**Objetivo:** enxergar tempo. Não tem dado próprio: é uma visão sobre o que já existe.

**Camadas ligáveis:** publicações, prazos de demanda, financeiro, reuniões e contratos (renovação e reajuste).

**Todo cartão leva ao objeto de origem.** Prazo de demanda abre a demanda no mesmo modal do projeto; publicação abre o planejamento daquele mês, com a publicação identificada e as demandas do mês acessíveis a partir dali. Calendário que não é clicável vira enfeite.

**Visualizações:** mês, semana, dia e lista. Fim de semana com fundo diferente. No celular as fichas encolhem.

**Por que serve para os dois lados:** o time enxerga acúmulo e buraco na distribuição do mês, e a mesma visão existe no link do cliente.

---

## 6. FINANCEIRO

Cinco abas.

### 6.1 Visão geral
- Cartões: receita realizada, receita prevista, despesas, resultado do mês, MRR e custo fixo mensal
- **Ponto de equilíbrio:** duas barras comparando custo fixo e MRR, com o percentual de cobertura. É o número que decide se dá para contratar alguém, e só existe porque despesa recorrente é uma entidade.
- Composição da receita: recorrência x projeto

### 6.2 Receitas e 6.3 Despesas
Tabelas separadas, cada uma com os próprios totais: total do mês, recebido ou pago, em aberto e em atraso. Na de despesas, o que vem de uma recorrência aparece marcado como fixa.

**Por que separadas:** juntas, num único "lançamentos", ninguém enxerga o que entra e o que sai sem filtrar. Separadas, cada aba responde uma pergunta.

### 6.4 Recorrentes
Cadastro das despesas fixas: descrição, categoria, periodicidade, dia de vencimento, valor, data de início e situação (ativa ou pausada).

- Cada recorrente ativa **gera o lançamento do mês sozinha**, no dia do vencimento
- Pausar interrompe sem apagar o histórico
- Os cartões separam o custo mensal do anual diluído: hospedagem de R$ 2.400 por ano não é R$ 2.400 de custo em março, é R$ 200 por mês

### 6.5 Projeção
Doze meses à frente, separando recorrência contratada de parcela de projeto, com bloco de risco: contratos que vencem no período, meses sem parcela de projeto e o custo fixo que continua rodando.

---

## 7. TIME

### 7.1 Lista do time
Cartão por pessoa com foto, nome, cargo, demandas abertas e a carga das próximas quatro semanas contra a **capacidade individual** (não é fixa em 40h: PJ de meio período tem 24h). Carga acima da capacidade ganha marcação.

### 7.2 Detalhe da pessoa (modal)

**Editável:** nome, cargo, área, email, telefone, vínculo, capacidade semanal, perfil de acesso e situação.

**Leitura, puxada do resto do sistema:** carga por semana, demandas abertas (clicáveis, abrem a demanda), projetos em que atua e contas sob responsabilidade.

**Ação prevista:** registrar ausência. Férias e afastamento precisam tirar a capacidade da pessoa do planejamento no período, senão o time parece ter braço que não tem.

**Nota:** carga é soma de horas estimadas, nunca apontamento. Serve para planejar, não para medir produtividade. É a consequência da decisão 3.

---

## 7.3 Central de notificações

**Objetivo:** substituir o "avisa no grupo" dentro da plataforma. Na v1 não existe email nem WhatsApp automático, então esta tela é o único canal.

**Dados exibidos:**
- Ícone de sino no topo com contador de não lidas
- Lista: tipo, título, descrição curta, origem (cliente e projeto), data e hora, lida ou não
- Agrupamento por não lidas e lidas
- Filtro por tipo

**Tipos de notificação na v1:**
- Demanda atribuída a você
- Você foi mencionado em um comentário
- Demanda vence hoje ou está atrasada
- Cliente aprovou um item
- Cliente solicitou ajuste
- Cliente acessou o link de aprovação pela primeira vez
- Prazo de retorno do cliente estourado (alerta interno, a cobrança é feita pelo time)
- Arquivo grande terminou de processar (vídeo pronto para envio)
- Planejamento inteiro aprovado
- Contrato vencendo em 30 dias
- Lançamento financeiro vencendo ou vencido

**Ações:** marcar como lida, marcar todas como lidas, ir para a origem

**Nota de consequência:** como não há disparo externo na v1, o cliente não é avisado pelo sistema. O time gera o link de aprovação e envia manualmente. Prever o campo de mensagem padrão pronta para copiar junto com o link.

---

## 8. CONFIGURAÇÕES

### 8.1 Organização
- Nome, CNPJ, logo, endereço, dados fiscais
- Cores e identidade (preparação para white-label futuro)
- Fuso horário, moeda
- Dados que saem nas propostas e relatórios

### 8.2 Usuários e permissões
- Lista de usuários com perfil
- Criação e edição de perfis de acesso
- Matriz: perfil x módulo x nível (sem acesso, ver, editar, administrar)
- Convites pendentes

### 8.3 Templates
**A tela mais subestimada do sistema.** É aqui que moram os modelos, e é por isso que nada no sistema tem nomenclatura fixa: tudo que parece "padrão" na operação é só um modelo salvo aqui, editável a qualquer momento.

- **Templates de projeto:** lista (identidade visual, naming, site, apresentação). Detalhe: nome, tipo, etapas (ordem, nome, duração estimada em dias, exige aprovação do cliente sim/não), demandas padrão por etapa (título, descrição, cargo responsável padrão, prazo relativo ao início da etapa)
- **Templates de planejamento:** lista (planejamento mensal orgânico, campanha paga, mensal com tráfego). Detalhe: nome, demandas padrão do mês, quantidade sugerida de publicações, partes de aprovação do cliente
- **Modelos de demanda** (usados nos dois acima): nome livre (Conteúdo, Redação, Design, Tráfego, Revisão, o que for), critério de conclusão, cargo responsável padrão, prazo relativo em dias, ordem, checklist padrão
- **Modelos de checklist:** listas de itens salvas para reuso dentro de uma demanda

Nada disso é obrigatório. Toda demanda e todo checklist pode ser criado do zero, renomeado ou excluído dentro de um projeto específico sem tocar no template. O template é atalho, não regra.

- Ações: criar, duplicar, editar, arquivar

### 8.4 Catálogo de serviços
- Lista: nome do serviço, descrição padrão de escopo, tipo (recorrência ou projeto), valor de referência, custo estimado, template de projeto associado
- Usado para montar propostas em segundos

### 8.5 Integrações
- Google Drive (pasta raiz por cliente)
- Google Calendar
- WhatsApp ou email para notificações
- Webhooks (futuro)

---

## 9. PERFIL DA PESSOA

Aberto pelo avatar no header. Duas abas.

**Conta:** foto (ou iniciais com a cor fixa da pessoa), nome, **nome de usuário com @** para menção em comentários, como quer ser chamada, email, telefone. Cargo aparece bloqueado, com a nota de que é definido pela gestão. Preferências: fuso, formato de data, tela inicial, visualização padrão, tema e idioma. Notificações por categoria. Segurança: senha, verificação em duas etapas e sessões ativas.

**Minha atividade:** demandas abertas, atrasadas, capacidade semanal, data de entrada, e a lista clicável do que é seu.

---

## 10. RELATÓRIO MENSAL

**Lista:** cliente, mês, entregas, pontualidade, rodadas de ajuste e situação.

**Relatório aberto**, em quatro camadas:

1. **Entrega do mês**, puxada do planejamento: previstas, aprovadas, publicadas, pontualidade e **rodadas de ajuste por peça**. Essa última é o número que ninguém mede e que explica quase todo atraso.
2. **Desempenho**: alcance, interações, seguidores, cliques. Digitado na v1, automático quando entrar a integração da Meta.
3. **Leitura estratégica**, escrita pela equipe. **O envio é bloqueado sem ela**: relatório sem leitura é planilha, e planilha o cliente não lê.
4. **Destaques e próximos passos**, listas editáveis que viram pauta da reunião seguinte.

**Fluxo de envio da v1, decidido:** o relatório **passa por revisão antes de sair**. Rascunho, depois "Em revisão" com alguém do perfil Administrador notificado, que aprova para envio ou devolve para ajuste. Só depois o botão de enviar fica ativo.

**O envio é manual.** O sistema marca como enviado, carimba data e hora e guarda o link; avisar o cliente é feito pelo canal de sempre. Envio automático por email fica para a v2.

**Ações:** mandar para revisão, aprovar para envio, devolver para ajuste, enviar ao cliente, exportar PDF, gerar link público.

---

## 11. ASSISTENTES DE CRIAÇÃO

Quatro passos cada, desenhados para mostrar as conexões e não só coletar campos.

**Novo projeto:** cliente → contratos dele → metodologia → prévia das demandas que vão nascer → dados → resumo do que será criado.

**Novo planejamento:** só lista clientes com contrato de recorrência, porque planejamento mensal nasce de um fee. Passo de responsáveis mostra a área vinda do template e quem o sistema sugeriu.

**A conexão que precisa ficar visível:** o volume de publicações vem do **escopo do contrato**, não do template. O template sugere, o contrato manda.

**O que a criação resolve sozinha:** área vira pessoa; etapa espelho recebe um item por publicação e etapa manual recebe o checklist fixo; **prazo relativo vira data absoluta**.

---

# PARTE 3: LINK DE APROVAÇÃO DO CLIENTE

Sem login na v1. Um link por planejamento e contato, permanente durante o mês. Funciona em celular e em desktop, com layouts diferentes. O cliente nunca vê valores, custos, demandas internas ou outros clientes.

### P.1 Identificação (primeira vez no link)

Logo, nome do planejamento e quantos itens aguardam resposta. Link para um contato específico pede só confirmação; link genérico pede nome e email. Token com validade padrão de 30 dias, revogável, e cada acesso registra data, hora, IP e dispositivo.

---

### P.2 Lista do mês

**Duas visualizações, alternáveis:**
- **Lista**: dois blocos, "Aguardando você" e "Fechados". Card com miniatura, nome (o tema), data, plataformas e o estado calculado.
- **Calendário**: o mês inteiro, cada publicação como ficha no seu dia, com o estado escrito. Aprovadas preenchidas, em ajuste tracejadas. No celular as fichas encolhem e escondem o texto de estado.

**Cabeçalho:** progresso (X de Y fechados) e prazo de retorno.

**Por que o calendário do lado do cliente:** ele enxerga buraco e acúmulo na distribuição do mês, que hoje é uma conversa que só acontece quando a Hoop levanta.

---

### P.3 Detalhe do item

**Ordem da tela, de cima para baixo:**
1. Nome (o tema), data, formato e plataformas
2. **Painel de aprovação**: as quatro partes lado a lado, cada uma com o seletor de status e, quando houver, "Aprovar tudo" com a contagem do que falta
3. Tema
4. Copy (com a formatação preservada)
5. Legenda
6. Criativo, no preview realista
7. Histórico e campo de mensagem

**Regra de ouro da tela:** o status aparece em um lugar só. O painel é onde se decide, o histórico é onde se lê o que aconteceu, e os blocos de conteúdo são só conteúdo. Uma versão anterior repetia o status no painel, no cabeçalho de cada bloco e no seletor, e ficou ilegível.

**Preview realista:** o criativo aparece dentro de um mockup do post, com avatar, arroba, área de mídia, ícones e a legenda embaixo cortada no "... mais". Carrossel navega com setas, pontos e swipe. Isso muda a conversa: o cliente deixa de avaliar um arquivo e passa a avaliar um post, e vê sozinho onde a legenda corta.

**Parte ainda em rascunho** aparece como "em produção", sem seletor. Item incompleto pode ser aprovado no que já existe: quando o resto chegar, volta para revisão.

---

### P.4 Versão web

Mesmo conteúdo, layout de duas colunas: textos à esquerda (tema, copy, legenda) e preview à direita. O painel de aprovação ocupa a largura toda no topo e o histórico no rodapé. A lista do mês vira grade de dois ou três cards por linha.

---

# PARTE 4: DICIONÁRIO DE DADOS

Entidades e seus campos essenciais. Isso é o que vira o banco.

**Regra multi-tenant (decisão 5):** toda tabela carrega `organizacao_id`, inclusive as que parecem "filhas" (demanda, entregável, lançamento). Parece redundante, mas é o que permite isolamento por linha no banco sem precisar percorrer relações em toda consulta. Nenhuma query do sistema roda sem filtro de organização. Adicionar isso depois significa migrar tudo.

### Entregavel (campos acrescentados)
organico (booleano), impulsionado (booleano), verba (decimal, só quando impulsionado)

Os dois sinalizadores são independentes e pelo menos um precisa estar ligado. `verba` soma no escopo do mês e gera lançamento de mídia.

### Lancamento (campos acrescentados na análise da planilha real)
Receita: `valor_bruto`, `taxa_cobranca`, `aliquota`, `imposto` (calculado), `valor_liquido` (calculado), `forma_pagamento`, `canal_cobranca`, `nota_fiscal_id`
Despesa: `natureza` (operacional | retirada_socio), `forma_pagamento`

**A regra que muda todo o financeiro:** receita bruta não é receita. Da planilha de 2026: em setembro, R$ 48.201 de bruto viram R$ 36.886 depois de R$ 3.002 de taxa e R$ 4.463 de imposto. Um sistema que soma só o bruto erra o resultado do mês em quinze por cento.

**Retirada de sócio não é custo operacional.** Sai do resultado, não do cálculo de quanto custa operar. Misturada, ela esconde se a operação se paga.

### LancamentoCusto (repasse)
id, lancamento_id, fornecedor, valor, descricao

Fornecedor contratado **para entregar aquela receita específica**: o tráfego da Cambaí, o dev do site. É o vínculo que faltava entre despesa e receita, e sem ele não existe margem real por cliente.

### NotaFiscal
id, numero, conta_id, cnpj, descricao_servico, valor, data_emissao, situacao, lancamento_id

A emissão continua no portal da prefeitura. O sistema registra para **conferir contra a receita do período** e avisar quando houver receita lançada sem nota. Dados fixos da emissão (regime, lista de serviço, local de prestação) ficam em Organizacao.

### Aliquota (por competência)
mes, aliquota, fonte (contabilidade | calculada | informada)

**A alíquota efetiva do Simples muda todo mês**, porque depende da receita bruta dos 12 meses anteriores. Três camadas resolvem, da mais simples à mais automática:

A contabilidade informa e alguém digita, dois minutos por mês. O sistema calcula pelo RBT12 e sugere, para conferência. E o lançamento individual pode ter exceção, que é o caso de cliente isento e de projeto com retenção diferente.

**A regra que sustenta as três:** a alíquota **congela no lançamento**. Setembro guarda a alíquota de setembro para sempre. Sem isso, mês fechado muda de valor sozinho e nenhum relatório bate duas vezes.

**As faixas do Anexo 3 no protótipo são referência e precisam ser conferidas com a contabilidade** antes de virarem cálculo oficial. O sistema sugere, a pessoa decide.

**Consequência útil:** a guia do Simples do mês é sempre a apuração do mês anterior, então o sistema pode lançá-la sozinho.

### SaldoMensal
mes, saldo_anterior, entradas, saidas, saldo_disponivel

Caixa, em paralelo à competência. São perguntas diferentes: quanto foi faturado e quanto está na conta.

### Usuario (campos acrescentados)
custo_mensal, vinculo (CLT | PJ | Sócio | Freelancer)

Com capacidade e custo, sai o **custo por hora**, que fecha o cálculo de rentabilidade e alimenta o orçamento por hora.

### Servico (campos acrescentados)
horas_por_area (objeto), valor_hora

Precificação como a planilha de orçamento já faz: horas por área vezes valor hora dá o valor base. O valor de referência continua editável por cima.

### Conta (campo acrescentado)
meses_de_casa (calculado a partir de `desde`) e LTV (recorrência × meses)

### Organizacao (cadastro completo)
id, nome, cnpj, logo_url, endereco, telefone, email, cores, fuso_horario, criado_em

### Usuario
id, organizacao_id, nome, apelido, email, senha_hash, foto_url, cargo, area, telefone, whatsapp, data_nascimento, data_contratacao, tipo_vinculo, capacidade_semanal_horas, perfil_permissao, status (ativo | ferias | afastado | inativo), ultimo_acesso

`capacidade_semanal_horas` é por pessoa, não fixa em 40: PJ de meio período tem 24. Sem isso, quem trabalha meio período aparece sempre folgado e recebe trabalho demais.

### Conta (lead e cliente na mesma tabela)
id, organizacao_id, tipo (lead | cliente), status, nome_fantasia, razao_social, cnpj, segmento, porte, logo_url, site, telefone, email, endereco, cidade, estado, redes_sociais (json), origem, responsavel_id, cliente_desde, estagio_funil, valor_estimado, motivo_perda, observacoes, fluxo_aprovacao_padrao (etapa_unica | duas_etapas), drive_folder_url, criado_em

### Contato
id, conta_id, nome, cargo, email, telefone, whatsapp, data_nascimento, principal (bool), aprovador (bool), acesso_portal (bool), senha_hash, ultimo_acesso

### Interacao
id, conta_id, contato_id, usuario_id, tipo, data, resumo, proximo_passo, data_followup

### Servico (catálogo)
id, nome, descricao_escopo, tipo (recorrencia | projeto), valor_referencia, custo_estimado, template_projeto_id

### Proposta
id, numero, versao, conta_id, contato_id, responsavel_id, tipo, data_emissao, validade_dias, descricao_escopo, valor_total, forma_pagamento, parcelas, vigencia_meses, data_inicio_prevista, status, motivo_recusa, pdf_url, criado_em

A proposta vigente é sempre a última versão. `versao` incrementa a cada rodada de negociação, e o que mudou fica em PropostaEvento.

### PropostaEvento
id, proposta_id, tipo (criacao | envio | alteracao | nova_versao | decisao), texto, versao, autor_id, criado_em

### PropostaItem
id, proposta_id, servico_id, descricao, quantidade, valor_unitario, valor_total

### PropostaCusto
id, proposta_id, tipo (fixo | variavel), descricao, valor

### Contrato
id, numero, conta_id, proposta_id, tipo, escopo, valor_total, valor_mensal, data_inicio, data_fim, renovacao_automatica, aviso_previo_dias, dia_vencimento, parcelas, forma_pagamento, indice_reajuste, data_reajuste, status, documento_url, criado_em

### ContratoHistorico
id, contrato_id, tipo_evento (reajuste | aditivo | suspensao | reativacao | renovacao | encerramento), valor_anterior, valor_novo, percentual, indice_aplicado, motivo, vigencia_a_partir_de, registrado_por, criado_em

### TemplateProjeto
id, organizacao_id, nome, tipo, descricao, ativo

### TemplateEtapa
id, template_projeto_id, ordem, nome, duracao_dias, exige_aprovacao_cliente

### TemplateDemanda
id, template_etapa_id, titulo, descricao, cargo_responsavel_padrao, prazo_relativo_dias, checklist_padrao (json)

### TemplatePlanejamento
id, organizacao_id, nome, descricao, quantidade_publicacoes_sugerida, fluxo_aprovacao_padrao, ativo

### TemplateDemandaPlanejamento
id, template_planejamento_id, nome, criterio_conclusao, cargo_responsavel_padrao, prazo_relativo_dias, ordem

### Projeto
id, conta_id, contrato_id, template_projeto_id, nome, tipo, descricao, briefing, responsavel_id, data_inicio, prazo_final, status, etapa_atual_id, percentual_concluido, drive_folder_url, criado_em

### Etapa
id, projeto_id, ordem, nome, status, data_inicio, data_fim_prevista, data_fim_real, exige_aprovacao_cliente

### Demanda
id, organizacao_id, projeto_id, planejamento_id, titulo, descricao, tipo, prioridade, ordem, data_inicio, prazo, tempo_estimado_horas, status (backlog | afazer | execucao | cliente | concluido), origem (template | avulsa), concluida_em, criado_por, criado_em

### SubDemanda
id, demanda_id, nome, criterio_conclusao (manual | legenda_aprovada | criativo_aprovado | tema_aprovado | publicado), prazo, ordem, concluida_em

Uma demanda tem uma ou mais sub-demandas. Com uma só, a interface não mostra o conceito e ela se comporta como checklist simples. `criterio_conclusao` define a natureza: manual é editável pela equipe, os demais espelham a aprovação da publicação e são travados.

### SubDemandaResponsavel
id, sub_demanda_id, usuario_id

### DemandaResponsavel
id, demanda_id, usuario_id

Uma demanda tem um ou mais responsáveis, com mínimo de um.

### ChecklistItem
id, sub_demanda_id, texto, responsavel_id, prazo, concluido, ordem, entregavel_id (quando o item representa uma publicação)

O item pendura na sub-demanda, não na demanda. Quando a sub-demanda tem critério automático e o item aponta para uma publicação, `concluido` é calculado e não editável.

Cada item tem responsável e prazo próprios. É o que permite uma demanda de pesquisa ter a parte de concorrentes com uma pessoa e a de referências visuais com outra, sem virar três demandas.

### DemandaAnexo
id, demanda_id, tipo (arquivo | link), nome, arquivo_id (quando for arquivo), url (quando for link), anexado_por, criado_em

Arquivo entra na biblioteca do cliente, na pasta do projeto. Link é referência externa e não ocupa armazenamento. Anexo é material de apoio interno: em publicações, o cliente não vê nada daqui.

### DemandaComentario
id, demanda_id, autor_id, texto, criado_em

### DemandaRegistro
id, demanda_id, autor_id, autor_tipo (usuario | contato), texto, criado_em

O log de ações. Registra mudança de status (com a origem: modal ou arraste no kanban), mudança de prazo, renomear, entrada e saída de responsável, item concluído ou reaberto, item criado ou removido, checklist criado ou removido, anexo adicionado ou removido, e fechamento automático por checklist completo.

Comentário e registro são entidades separadas e aparecem em blocos separados, tanto nas telas internas quanto no link do cliente. Ambos do mais novo para o mais velho. O log nasce recolhido.

**Carimbo:** `criado_em` em UTC no servidor, convertido para o fuso da organização na exibição. Confiar no relógio do navegador faz o histórico deixar de servir como prova.

### Comentario
id, entidade_tipo (demanda | projeto | entregavel | conta), entidade_id, autor_tipo (usuario | contato), autor_id, texto, mencoes (json), visivel_cliente (bool), criado_em

### Pasta
id, organizacao_id, conta_id, pasta_pai_id, nome, tipo (sistema | personalizada), criado_em

### Arquivo
id, organizacao_id, conta_id, pasta_id, nome, storage_path, storage_bucket, mime_type, tamanho_bytes, largura, altura, duracao_segundos, thumbnail_path, preview_path (versão web otimizada, imagem ou vídeo), status_processamento (pendente | processando | pronto | erro), versao, arquivo_origem_id (para versionamento), url_externa (quando for link do Drive em vez de arquivo armazenado), enviado_por, criado_em, arquivado_em

### ArquivoVinculo
id, arquivo_id, entidade_tipo (demanda | projeto | entregavel | contrato | comentario | conta), entidade_id, criado_em

### Planejamento (fee mensal)
id, organizacao_id, conta_id, contrato_id, template_planejamento_id, mes_referencia, tipo, status, quantidade_contratada, data_envio_cliente, prazo_retorno, responsavel_id, criado_em

### Entregavel (a publicação, independente de plataforma)
id, organizacao_id, planejamento_id, projeto_id, etapa_id, tipo (publicacao | arquivo | apresentacao), origem (planejada | extra), data_publicacao_prevista, formato_familia, veiculacao (organico | patrocinado), tema, versao_tema, copy_html, versao_copy, legenda, versao_legenda, hashtags, cta, link, pilar_conteudo, objetivo, responsavel_conteudo_id, responsavel_criativo_id, duplicado_de_id, publicado_em, cancelado_em, motivo_cancelamento, planejamento_origem_id, criado_em

Não existe `titulo_interno`: o nome da publicação é o tema, com queda para o início da copy e depois para "Publicação de DD/MM". Não existe `fluxo_aprovacao` nem `liberado_ao_cliente`: o que esconde do cliente é o status `rascunho` das partes.

### EntregavelParte
id, entregavel_id, tipo (tema | copy | legenda | criativo), status (rascunho | aprovacao | ajuste | aprovado), marcado_por_tipo (usuario | contato), marcado_por_id, marcado_em, versao_aprovada

Regras: quatro partes por publicação, independentes, sem ordem obrigatória. `versao_aprovada` guarda a versão da parte no momento do aprovado (para copy, legenda e tema é o número de versão; para criativo, a assinatura do conjunto de peças). Divergência entre a versão atual e a aprovada é o que marca "mudou depois de aprovado".

### EntregavelEvento
id, entregavel_id, tipo (status | mensagem), texto, autor_tipo (usuario | contato), autor_id, criado_em

A linha do tempo da publicação. Mudanças de status e mensagens no mesmo fio, em ordem cronológica. Substitui a antiga RodadaAprovacao e a lista de comentários.

### EntregavelDestino
id, entregavel_id, plataforma, publicado_em, ordem

Regras: a publicação tem pelo menos um destino, e todos usam o mesmo texto e as mesmas peças. O destino não guarda formato, texto nem aprovação: a aprovação é da publicação, o formato é a família, e o nome nativo é derivado de família + plataforma. Divergência de conteúdo se resolve duplicando a publicação, com `duplicado_de_id` guardando a origem.

### EntregavelPeca
id, entregavel_id, ordem, papel (slide | video | capa), tipo (imagem | video), rotulo, arquivo_id, thumbnail_path, gerada_automaticamente (capa extraída do vídeo)

Um conjunto por publicação, não por plataforma. É a família do formato que garante que isso funcione: Reels, Shorts e vídeo do TikTok são o mesmo arquivo.

### LinkAprovacao
id, organizacao_id, token, planejamento_id, contato_id (opcional), exige_identificacao, expira_em, revogado_em, criado_por, criado_em

Um link por planejamento e contato, vivo o mês inteiro. Não existe link por etapa nem por item.

### AcessoLink
id, link_aprovacao_id, contato_id, nome_informado, email_informado, ip, user_agent, acessado_em

### TemplatePartes (na configuração do cliente)
Define quais das quatro partes valem para aquele cliente. Tema é a única opcional na v1.

### DespesaRecorrente
id, organizacao_id, descricao, categoria, valor, periodicidade (mensal | trimestral | anual), dia_vencimento, data_inicio, data_fim, status (ativa | pausada), gera_automatico

Cada recorrente ativa gera o lançamento do período sozinha. Pausar interrompe a geração sem apagar o histórico. É o que permite calcular custo fixo e ponto de equilíbrio.

### Lancamento
id, organizacao_id, tipo (receita | despesa), categoria, descricao, valor, conta_id, contrato_id, projeto_id, despesa_recorrente_id, fornecedor, data_vencimento, data_pagamento, forma_pagamento, status, anexo_url, criado_por, criado_em

### Atividade (log)
id, organizacao_id, usuario_id, entidade_tipo, entidade_id, acao, descricao, criado_em

### Notificacao
id, destinatario_tipo, destinatario_id, tipo, titulo, mensagem, link, lida, criado_em

---

# PARTE 5: STATUS E ENUMERAÇÕES

**Estágio do funil:** novo, contato_feito, reuniao_marcada, proposta_enviada, em_negociacao, ganho, perdido

**Status da proposta:** rascunho, enviada, em_analise, aprovada, recusada, expirada

**Status do contrato:** ativo, encerrado, suspenso, em_renovacao

**Status do projeto:** nao_iniciado, em_andamento, aguardando_cliente, pausado, concluido, cancelado

**Status da etapa:** nao_iniciada, em_andamento, aguardando_aprovacao, aprovada, concluida

**Status da demanda:** backlog, afazer, execucao, cliente, concluido (calculado a partir das sub-demandas quando houver mais de uma)

**Critério de conclusão da sub-demanda:** manual, tema_aprovado, legenda_aprovada, criativo_aprovado, publicado

**Nome da demanda:** texto livre. Não existe lista fixa.

**Tipo de anexo:** arquivo, link

**Origem do entregável:** planejada, extra

**Plataformas:** instagram, facebook, linkedin, tiktok, youtube

**Veiculação:** organico, patrocinado

**Família de formato:** imagem, carrossel, story, video-vertical, video-horizontal, artigo

**Formato do entregável:** feed, carrossel, stories, reels, anuncio

**Papel da peça:** slide, video, capa, variacao

**Status interno do entregável:** rascunho, em_criacao, revisao_interna, pronto_para_envio, cancelada

**Partes da publicação:** tema, copy, legenda, criativo

**Status da parte:** rascunho, aprovacao, ajuste, aprovado

**Tipo de evento:** status, mensagem

**Status do planejamento:** em_producao, enviado_aprovacao, em_ajustes, aprovado, publicado

**Status do lançamento:** previsto, faturado, pago, atrasado, cancelado

---

# PARTE 6: MATRIZ DE PERMISSÕES

A matriz completa, ação por ação, está na tela Configurações > Usuários e permissões do protótipo, e é a fonte da verdade. São 36 ações em 10 áreas, contra 5 perfis, com quatro níveis: **tudo**, **o que é seu**, **só ver** e **não**.

**Quatro regras que a matriz assume:**

O nível "o que é seu" filtra pelos registros em que a pessoa é responsável, e não é uma tela diferente: é a mesma tela com menos linhas.

**Quem não pode ver, não vê no menu nem por link direto.** Permissão escondida na interface e aberta no servidor é o erro clássico.

**Custo e margem só aparecem para Administrador e Financeiro.** Quem executa vê valor de contrato quando precisa, nunca a margem.

**A organização nunca fica sem administrador.** O sistema recusa rebaixar o último.

**Três decisões que a matriz fecha e que valem destacar:**

Operador **não muda prazo nem responsável**, nem os seus: pede ao gestor. Sem isso, prazo vira combinado informal e o relatório de pontualidade perde o sentido.

Gestor **não mexe em contrato, dados bancários nem reajuste**. Toca cliente e produção.

Financeiro **não entra na produção**, e vê relatório só para conferir entrega contra faturamento.

---


Perfis sugeridos: Administrador, Gestor, Operador, Financeiro, Cliente

| Módulo | Administrador | Gestor | Operador | Financeiro | Cliente |
|---|---|---|---|---|---|
| Dashboard | completo | completo | parcial (só produção) | parcial (só financeiro) | sem acesso |
| CRM | completo | completo | ver | sem acesso | sem acesso |
| Clientes | completo | completo | ver | ver | sem acesso |
| Contratos | completo | ver | sem acesso | editar | sem acesso |
| Demandas | completo | completo | editar as suas | sem acesso | sem acesso |
| Planejamentos | completo | completo | editar | sem acesso | ver e aprovar os seus |
| Calendário | completo | completo | ver | ver financeiro | sem acesso |
| Financeiro | completo | ver | sem acesso | completo | sem acesso |
| Time | completo | ver | ver | sem acesso | sem acesso |
| Configurações | completo | sem acesso | sem acesso | sem acesso | sem acesso |

---

# PARTE 7: CONSEQUÊNCIAS DAS DECISÕES

O que cada decisão fechada obriga o sistema a ter.

### 1. Plataforma armazena os arquivos

**Nota estratégica:** esta é a decisão mais cara do conjunto. Armazenar arquivo não é só guardar. Envolve upload de arquivo grande, geração de miniatura, pré-visualização no navegador, custo de armazenamento recorrente, backup, e uma tela de biblioteca que o time realmente use. Uma agência que produz Reels gera volume rápido.

**Recomendação (aplicada no documento):** modelo híbrido. A plataforma armazena tudo que precisa ser exibido dentro dela ou dentro de um link de aprovação (criativos, peças, entregáveis finais, documentos). O material bruto de produção continua no Drive e entra como link externo. Isso te dá a experiência boa de aprovação sem transformar a plataforma num serviço de armazenamento.

**Obriga a ter:**
- Tela 3.4 Biblioteca de arquivos
- Entidades Pasta, Arquivo, ArquivoVinculo
- Versionamento de arquivo (arquivo_origem_id)
- Lixeira com retenção
- Limite de tamanho e formatos definidos
- Indicador de espaço usado (importante quando virar SaaS, vira item de plano)

### 2. Lead e cliente na mesma tabela

**Obriga a ter:** campo `tipo` na Conta, filtros de listagem separados por tipo, e uma conversão que só muda o tipo e preenche `cliente_desde`. Nenhum dado é recriado.

### 3. Só estimativa, sem timesheet

**Obriga a ter:** `tempo_estimado_horas` na demanda e `capacidade_semanal_horas` no usuário.
**Ganho:** planejamento de carga do time.
**Perda aceita:** rentabilidade por projeto fica baseada em custo alocado da proposta, não em horas reais. Se um dia você quiser saber onde o projeto sangrou, não vai ter o dado.

### 4. Aprovação por link público

**Obriga a ter:**
- Entidades LinkAprovacao e AcessoLink
- Token aleatório longo, validade, revogação
- Tela P.1 de identificação: sem isso, você tem aprovação anônima e perde exatamente o registro que motivou o projeto
- Preferência por link por contato (você já sabe quem é, o cliente só confirma)
- Nenhuma informação sensível acessível pelo token além do que ele aprova

### 5. Multi-tenant desde já

**Obriga a ter:** `organizacao_id` em todas as tabelas, isolamento por linha no banco, e todo acesso passando pela organização do usuário logado. Cuidado principal: o link público de aprovação também precisa resolver a organização pelo token, nunca por parâmetro de URL.

### 6. Notificações só na plataforma

**Obriga a ter:** tela 7.3 e ícone de sino.
**Consequência prática:** o cliente não é avisado de nada automaticamente. O envio do link continua manual, no WhatsApp. Prever mensagem padrão pronta para copiar junto com o link.

### 7. Aprovação de conteúdo e criativo separadas

**Obriga a ter:**
- Duas trilhas de status independentes no entregável
- Campo de fluxo no cliente (padrão) e no planejamento (exceção)
- Regra de bloqueio: criativo só é enviável com conteúdo aprovado
- Regra de reabertura: alterar conteúdo aprovado invalida a aprovação do criativo
- Link de aprovação carrega a etapa
- O portal muda o que mostra conforme a etapa

---

# PARTE 8: CONSEQUÊNCIAS DA SEGUNDA RODADA

### 8. Sem limite de armazenamento

**Nota estratégica:** "sem limite" é uma decisão de produto, não técnica. O custo é seu e cresce todo mês, para sempre, porque arquivo de agência nunca é apagado. O impacto não é grande enquanto for só a Hoop. Passa a ser relevante no dia em que virar SaaS vendido para outras agências, porque aí você absorve o custo de armazenamento de clientes que pagam mensalidade fixa. Sugestão: não impor limite agora, mas medir desde o primeiro dia (o indicador de espaço por cliente já está previsto na 3.4). Sem medição, você descobre o problema pela fatura.

**Obriga a ter:** upload em partes, retomada de upload interrompido, e nada de carregar arquivo inteiro na memória do servidor.

### 9. Vídeo nativo

**Obriga a ter:**
- Processamento em segundo plano (fila), porque converter vídeo trava qualquer requisição
- Versão web padronizada (MP4/H.264) além do original
- Frame de capa automático
- Campos de status de processamento no arquivo, com estado visível na interface
- Player no link de aprovação que funcione bem no celular, que é onde o cliente vai abrir

### 10. Aprovação parcial

**Obriga a ter:** decisão por item, status do planejamento calculado e não digitado, link reutilizável até o fechamento do mês, e produção liberada item a item.

### 11. Prazo de retorno só sinaliza internamente

**Obriga a ter:** contagem de dias aguardando retorno visível no planejamento, na lista de planejamentos e no dashboard, mais notificação interna no estouro. Nenhuma comunicação automática ao cliente.

### 12. Histórico de reajuste

**Obriga a ter:** entidade ContratoHistorico registrando todo evento de valor. O contrato guarda o valor vigente, o histórico guarda a trajetória. Reajuste recalcula apenas lançamentos futuros ainda não faturados, nunca o que já passou.

### 13. Só arquivamento

**Obriga a ter:** campo `arquivado_em` em conta, contrato, projeto, demanda, entregável, arquivo, usuário e planejamento. Toda listagem filtra arquivados por padrão. Nenhuma tela tem botão excluir. Consequência a aceitar: registro errado criado por engano fica lá para sempre, arquivado. Vale prever um filtro "ver arquivados" para não virar assombração.

### 14. Sem onboarding automático

**Obriga a ter:** criação de cliente cria só o cliente. Pastas e projetos são criados quando o time quiser, com um clique para aplicar modelo de estrutura de pastas e outro para criar projeto a partir de template.

---

# PARTE 9: CONSEQUÊNCIAS DAS RODADAS DE PROTÓTIPO

O protótipo navegável derrubou várias decisões anteriores. O que sobreviveu e o que caiu:

### O que caiu, e por quê

**Etapas de aprovação (conteúdo depois criativo).** Criava bloqueio, ordem obrigatória e dois links. Na prática o cliente não pensa em etapas, e o time precisava de flexibilidade. Substituído por quatro status independentes.

**Frente de demanda, e depois Etapa de projeto.** A frente nasceu para dar prazos diferentes a pessoas diferentes dentro da mesma demanda. Quando as demandas do planejamento passaram a ser criadas uma por natureza de trabalho (Redação, Arte, Tráfego), a frente virou uma demanda e a camada sumiu. A etapa de projeto caiu pelo mesmo motivo: briefing, pesquisa e conceito são demandas, não rótulos acima delas. Sobrou uma camada só: o projeto é uma lista de demandas com status.

**Copy por peça.** Um campo de texto por slide inchava a tela e obrigava a rolar muito. Virou um campo só, em texto rico, onde cabe "Peça 1", "Peça 2" e o roteiro inteiro.

**Comentário por peça com etiqueta de origem.** Botões espalhados e uma etiqueta por comentário. Virou um fio de conversa só, onde o cliente escreve onde quer o ajuste.

**Painel lateral de edição.** Tirava foco na hora de escrever e espremia o campo mais importante. Virou modal de duas colunas com navegação entre publicações.

**Título interno.** Dois campos de identificação para a mesma coisa. O tema é o nome.

**Flag de "liberado ao cliente".** Redundante com o status rascunho.

### O que a plataforma de referência ensinou

Análise de um concorrente (Pode Postar) trouxe quatro coisas que entraram:
- **Preview realista do post** em vez de caixa cinza: muda a conversa de "arte" para "post"
- **Chat por conteúdo, nos dois sentidos**: é o que finalmente tira a conversa do WhatsApp, porque antes o cliente comentava na plataforma e a Hoop respondia no zap
- **Tema aprovado antes da produção**: economia de retrabalho, e é onde a estratégia da Hoop aparece
- **Status como seletor, mudável pelos dois lados**: reflete a realidade de um ok que chega por fora

Duas coisas foram descartadas de propósito: **cliente editando os campos direto** (texto é o produto da Hoop, e cliente reescrevendo legenda faz a marca assinar o texto dele) e **dois modos nomeados de fluxo** (com status independente por parte, aprovar tudo de uma vez é só um botão).

### Regra de tela que saiu disso

**Status em um lugar só.** Uma versão do protótipo mostrava o status no painel, no cabeçalho de cada bloco e no seletor. A tela ficou ilegível. A regra virou: painel para decidir, histórico para ler, blocos de conteúdo só com conteúdo.

### Sub-demanda: a frente, reescrita (decisões 51 a 53)

A frente foi criada para dar prazos diferentes a pessoas diferentes dentro da mesma demanda, e caiu porque era invisível na tela e o conceito vazava onde não devia. A sub-demanda é a mesma ideia com duas correções:

- **É explícita.** Aparece como bloco numerado dentro da demanda, com nome, responsáveis, prazo, critério e checklist próprio.
- **Só existe quando existe mais de uma.** Demanda com um checklist não mostra o conceito em lugar nenhum.

**O que ela resolveu que nada mais resolvia:** o planejamento do mês tem quatro naturezas de trabalho com prazos diferentes (briefing em 25/08, conteúdo em 05/09, design em 12/09, publicação em 30/09) e precisa ser **um cartão só**, porque é um mês de um cliente. Sem a sub-demanda, ou viram quatro cartões soltos ou vira um cartão sem controle por etapa.

**Efeito colateral bom:** o mesmo mecanismo serve a projeto. "Manual da marca" passou a ter Diagramação com o Gonzaga, Revisão com a Julia e Fechamento com o Gonzaga, dentro de um cartão só.

### Consistência entre telas (decisões 45 a 50)

**Um layout de cartão só.** Demanda, planejamento, proposta e lead usam a mesma estrutura: título, subtítulo, barra de progresso, selos e responsável, sempre na mesma ordem. Antes eram layouts parecidos e diferentes, o que obriga o olho a reaprender a cada tela.

**Kanban como padrão** onde há fluxo: projetos, planejamentos, propostas e leads. Lista sempre disponível, porque tabela escaneia melhor quando o volume cresce.

**Proposta com versão.** Negociação muda escopo, valor e parcela várias vezes. Cada rodada gera versão e registro, nada é sobrescrito. Sem isso ninguém sabe qual é a proposta vigente e o cliente lembra do número antigo.

**Lead sem proposta.** Exigir valor e orçamento no cadastro enche o funil de proposta fantasma e estraga a taxa de conversão.

**Tudo leva ao objeto.** Cartão de calendário abre a demanda ou o planejamento, demanda na tela da pessoa abre a demanda, proposta no lead abre a proposta, contrato no cliente abre o contrato. A regra: se o sistema mostra a referência a alguma coisa, clicar tem que levar até ela.

**Meu trabalho é derivado, não uma lista própria.** A tela lê os projetos e os planejamentos e filtra por responsável. Uma versão do protótipo tinha uma lista paralela de demandas, e isso é exatamente o tipo de duplicação que faz a plataforma mentir depois de dois meses de uso.

---

# PARTE 10: LINGUAGEM VISUAL

O design system da Hoop (Figma) fornece a base. O que está aqui é como ele se aplica a um sistema de gestão, que é uso diferente do que o DS foi desenhado para cobrir.

## 10.1 Tokens

**Cor:** paleta do DS. Gray, Primary (#7A5AF8), Secondary (#2E90FA), Tertiary (#EE46BC), Success (#0F9918), Warning (#FF8800), Error (#E54545).

**Três correções feitas na conversão**, que valem ser aplicadas de volta no Figma:
- A rampa de cinza está invertida na ponta clara: Gray 50 (#ADADAD) é mais escuro que Gray 100 (#9E9E9E)
- Falta a faixa clara inteira. A rampa pula de #ADADAD para o branco, e é ali que vivem fundo de página, superfície de card, linha e borda. Foram derivados quatro tons: #D6D6D6, #E6E6E6, #F2F2F2, #F8F8F8
- Success 50 está rosa (#FEEDFA, que é o Tertiary 50), e Success 700 e 600 são idênticos

**Tipografia:** Poppins. Bricolage Grotesque fica para material de marca, não aparece no sistema.

**Espaçamento:** grid suave de 4px. **Forma:** raio de 8px, 6px em controles pequenos, pílula em selo e avatar. **Ícones:** Phosphor Regular, 20px, herdando a cor do texto.

## 10.2 A regra de cor de estado

Seis tons, cada um com um significado único no sistema inteiro.

| Tom | Significado | Onde aparece |
|---|---|---|
| Cinza | Parado por escolha, sem urgência | Backlog, a fazer, rascunho, previsto |
| Roxo | Acontecendo agora, é com você | Fazendo, em negociação, item selecionado |
| Azul | Esperando terceiro, não é sua vez | Com o cliente, proposta enviada, faturado |
| Amarelo | Atenção, ainda dá tempo | Vence em até 2 dias, ajuste pedido, contrato vencendo |
| Vermelho | Estourou, precisa de ação | Atrasado, vencido, margem baixa, carga acima da capacidade |
| Verde | Fechado, pode esquecer | Concluído, aprovado, pago, publicado |

**Duas regras que sustentam:** cor nunca vem sozinha, sempre acompanha texto ou ícone; e rosa fica fora dos estados, reservado para gráfico e categoria.

**Azul é a adição menos óbvia e a mais útil.** "Com o cliente" não é problema seu nem tarefa sua. Misturar isso com cinza faz a pessoa achar que está devendo quando está esperando.

**Prazo é calculado, não digitado:** venceu, vermelho; vence em até 2 dias, amarelo; mais que isso, neutro; concluído, verde.

A regra está documentada dentro do próprio sistema, em Configurações > Organização, para quem entrar no time conseguir ler o sistema sem treinamento.

## 10.3 A regra de cor de número

**Número só ganha cor quando existe um limite para comparar.** Valor solto fica neutro.

| Onde | Limite declarado |
|---|---|
| Ponto de equilíbrio | MRR contra custo fixo |
| Projeção | Total do mês contra custo fixo |
| Margem | Meta de 50%, alerta abaixo de 40% |
| Pontualidade | Meta de 90% |
| Rodadas de ajuste | Meta de até 1,5 por peça |
| Carga do time | Capacidade individual, não 40h fixas |

A meta aparece escrita ao lado do número. Sem ela, a cor vira opinião.

## 10.4 Cor no calendário

**A cor diz o estado, o ícone diz a categoria.** Sem isso, duas linguagens de cor competiriam na mesma tela: uma para camada (publicação, prazo, financeiro) e outra para estado.

Cada ficha tem faixa colorida à esquerda, fundo suave, ícone da categoria e um texto auxiliar que repete o estado em palavra. O dia de hoje tem contorno. Embaixo da grade, uma legenda com as cinco amostras.

## 10.5 Hierarquia tipográfica

Sete papéis nomeados, para não haver decisão de tamanho na hora de construir.

| Papel | Tamanho | Onde |
|---|---|---|
| Título da tela | 20px semibold | header e modais |
| Número | 26px semibold | cartão de métrica |
| Seção | 15px semibold | título de grupo |
| Item | 14px | cartão, linha de tabela, aba |
| Corpo | 14px | texto de leitura |
| Meta | 12px | subtítulo, data, apoio |
| Micro | 11px caixa alta 600 | rótulo de coluna, selo, label de campo |

## 10.6 Largura e tema

**O conteúdo ocupa a tela**, com respiro de 28px. Kanban, tabela e grade se esticam. A única exceção é texto corrido, limitado a 72 caracteres por linha: briefing, descrição e leitura estratégica de relatório.

**Tema claro por padrão**, com alternador. O escuro funciona, mas usa a rampa clara invertida. Se dark mode entrar de verdade, precisa de rampa própria no Figma: o roxo #7A5AF8 sobre #080808 fica com contraste no limite.

## 10.7 Avatar

Toda pessoa aparece com avatar ao lado do nome: cartão, checklist, chip de responsável, tabela, calendário do time, notificação, histórico e comentário. Sem foto, iniciais com **cor fixa por pessoa**, a mesma em todo lugar. Em cartão com vários responsáveis, os avatares empilham sobrepostos.

Cliente tem cor própria (rosa), o que distingue time de cliente no fio de conversa sem precisar de rótulo.

---

## 10.8 Componentes que o DS não cobre

Seis peças foram criadas sobre os tokens existentes, porque o DS foi desenhado para site e não para sistema de gestão: **tabela de dados, cartão de kanban, navegação lateral, selo de status semântico, estado vazio e o editor de checklist**. Vale levá-las de volta para o Figma como extensão.

---

# PARTE 11: PADRÕES DE INTERAÇÃO

Decisão de sistema, não de tela. Vale para tudo que for construído daqui em diante.

### Página, modal ou inline

**Página** para objeto que se abre, examina e revisita: cliente, contrato, projeto, proposta, planejamento, demanda. Tem URL própria, abre em nova aba, o link pode ser mandado para alguém do time, aguarda abas internas. Regra prática: mais de uma seção ou mais de uns 12 campos, é página.

**Modal** para tarefa curta com começo e fim claros: editar publicação, criar demanda, gerar link de aprovação, lançar despesa, aplicar reajuste, confirmar arquivamento. Uma coisa só, sem abas internas, e nunca um modal aberto de dentro de outro.

**Inline** para o que não merece interrupção: marcar item de checklist, trocar responsável, arrastar card no kanban, mudar data no calendário.

**O teste:** se você precisaria mandar o link daquilo para alguém, é página. Se é "faz isso agora e volta para o que estava fazendo", é modal. Se é um clique só, é inline.

### Regras do modal
- Largura confortável (referência: 820px) e duas colunas quando houver mídia e texto juntos
- Navegação entre irmãos sem fechar: setas no cabeçalho, contador de posição, ação "salvar e ir para a próxima", setas do teclado quando o foco não estiver em campo de texto
- Esc fecha, clique fora fecha, foco entra no campo principal ao abrir
- Nada de scroll dentro de scroll

### Painel lateral
Descartado como padrão de edição. Foi testado no protótipo e tira o foco justamente na hora de escrever, além de espremer o campo de texto mais importante da tela. Fica reservado para leitura passageira (ver histórico, ver comentários) onde o contexto atrás importa.

---

# PARTE 12: VOCABULÁRIO ÚNICO E CONTRATOS

Os dois protótipos nasceram separados e criaram apelidos diferentes para as mesmas coisas. No código isso vira bug silencioso, porque cada lado calcula com a regra que conhece. Esta parte fecha um termo por conceito e escreve os contratos entre eles. **Vale sobre qualquer nome usado nos protótipos.**

## 12.1 Um termo por conceito

| Termo oficial | Apelidos a aposentar | O que é |
|---|---|---|
| **Demanda** | tarefa, card, atividade | A unidade de trabalho. Pendura em projeto ou em planejamento |
| **Sub-demanda** | frente, frente do mês, etapa | Um checklist nomeado dentro da demanda, com responsáveis, prazo e critério |
| **Item de checklist** | subtarefa, item | A linha marcável dentro da sub-demanda |
| **Publicação** | post, card, entregável | O que vai ao ar em uma data. Uma publicação, uma data |
| **Parte** | campo de aprovação, etapa de aprovação | Tema, copy, legenda e criativo. É o que o cliente aprova |
| **Peça** | arte, slide, formato | O arquivo dentro do criativo. Uma publicação tem N peças |
| **Destino** | plataforma, rede | Onde a publicação sai, com o formato daquela plataforma |

**A confusão mais cara é "frente".** No editor de planejamento ela nomeia o agrupamento de trabalho do mês (Conteúdo, Design, Tráfego), e isso é exatamente uma **sub-demanda** da demanda do planejamento. Mantê-las com nomes diferentes faria o desenvolvimento criar duas tabelas para a mesma coisa.

## 12.2 Duas máquinas de estado, propósitos diferentes

Elas **não** se fundem, e isso é decisão, não acaso.

**Estado da demanda** (interno, quem trabalha): `backlog`, `afazer`, `execucao`, `cliente`, `concluido`.

**Estado da parte** (externo, o que o cliente responde): `rascunho`, `aprovacao`, `ajuste`, `aprovado`.

Fundir as duas obrigaria a inventar "em aprovação" para uma demanda de identidade visual, que não tem aprovação por link, ou "backlog" para uma legenda, que não faz sentido. São eixos diferentes: um é de quem está com a bola, o outro é de o que o cliente já respondeu.

## 12.3 Contrato de espelhamento

A sub-demanda tem uma `natureza` declarada, e ela decide quem manda:

**`espelho`** — os itens são gerados pelas publicações do mês e **não podem ser marcados à mão**. Cada item fecha quando a parte correspondente atinge o estado do `criterio` da sub-demanda. Conteúdo espelha a parte `legenda`; Design espelha `criativo`; Briefing espelha `tema`.

**`interno`** — checklist livre, marcado por quem trabalha, sem relação com aprovação.

**As regras derivadas, que valem nos dois lados:**

A parte é a fonte da verdade. O item de checklist é reflexo, nunca o contrário.

Quando toda sub-demanda de uma demanda fecha, a demanda vai para `concluido` sozinha, e o log registra a causa ("pelo checklist completo").

Quando toda publicação do mês tem todas as partes em `aprovado`, o planejamento vai para `aprovado` sozinho.

Publicação cancelada sai da conta dos dois lados: não conta como item pendente nem como entrega do mês.

Peça alterada depois da aprovação marca a parte como **desatualizada**, que não é um quinto estado: é um sinalizador sobre o estado `aprovado`, e reabre o item espelhado.

## 12.4 Uma fórmula por métrica

| Métrica | Fórmula | Onde aparece |
|---|---|---|
| Progresso da demanda | itens fechados ÷ itens totais, somando as sub-demandas | cartão, lista, kanban |
| Progresso do planejamento | soma dos itens fechados ÷ soma dos itens totais das demandas | cartão e lista de planejamentos |
| Completude da publicação | partes com conteúdo ÷ partes exigidas pelo contrato | grade do editor |
| Aprovação da publicação | partes em `aprovado` e não desatualizadas ÷ partes exigidas | link do cliente, relatório |
| Entrega do mês | publicações publicadas ÷ publicações contratadas | relatório, dashboard |
| Pontualidade | entregues no prazo ÷ entregues | relatório |
| Rodadas de ajuste | vezes que uma parte voltou para `ajuste` ÷ publicações | relatório |

**Regra que fecha:** "partes exigidas" vem do contrato do cliente, não do template. O template sugere, o contrato manda.

## 12.5 Formato dos registros

Comentário e registro de ação são **entidades separadas**, com o mesmo formato nos dois lados: autor, tipo de autor (usuário ou contato), texto e `criado_em`.

`criado_em` é sempre UTC no servidor, exibido no fuso da organização, no formato `DD/MM/AAAA · HH:MM`. Nada de "hoje" ou "há 2 dias" armazenado: isso é formatação de exibição.

Toda data guardada é data completa com ano. `04/09` só existe na tela.

## 12.6 Padrões de interação do sistema todo

Valem em qualquer tela, e por isso não estão repetidos tela a tela.

**Salvamento automático.** Modal de edição salva a cada alteração e mostra "salvo agora" no rodapé. Não existe par Salvar/Cancelar: existe Concluir, que fecha. Formulário de criação é a exceção, porque ali Cancelar realmente descarta o que nunca existiu.

**Desfazer no lugar de confirmar.** Exclusão acontece na hora e oferece Desfazer por 8 segundos, também por Cmd+Z. Caixa de confirmação sobrou só onde a ação é irreversível e cara.

**Edição inline.** Em lista, status, responsável e prazo são editáveis na própria linha, sem abrir nada. Toda edição inline entra no log com a origem declarada ("direto na lista", "arrastando no kanban", "em lote").

**Ação em lote.** Listas e kanbans permitem selecionar vários e mudar status, responsável ou prazo de uma vez. A barra de lote fica presa ao topo enquanto houver seleção.

**Paleta de comandos.** Cmd+K busca e executa: navegar, criar, abrir cliente, projeto, demanda, proposta ou planejamento. Atalhos: `/` busca, `N` cria na tela atual, `Cmd+Enter` salva, `?` lista os atalhos.

**Estado vazio ensina.** Toda lista vazia diz o que aquilo é, o que fazer e traz o botão para fazer.

**Filtro e ordenação.** Cada listagem tem filtros rápidos por cliente, responsável e situação, mais período onde faz sentido (mês, mês passado, últimos 3 meses, ano, intervalo). Filtro aplicado vira chip removível. Toda tabela ordena por coluna. **O filtro é lembrado por tela, nunca globalmente.**

## 12.7 Entrada de dados

**Máscara local** em valor, telefone, CEP, CPF, CNPJ, percentual, horas, agência, conta, mês/ano e @usuario. Estado e fuso horário são listas, nunca campo livre.

**CNPJ no formato alfanumérico**, vigente desde 2026: doze posições alfanuméricas mais dois dígitos verificadores numéricos, com cálculo pelo valor ASCII do caractere. Máscara só de dígitos está errada.

**Consulta externa preenche, não decide.** CNPJ traz razão social, fantasia, CNAE, porte, endereço, telefone e **situação cadastral**; CEP traz logradouro, bairro, cidade e UF. Três regras: o campo continua editável, falha nunca bloqueia o salvamento, e o resultado fica em cache por chave.

**Chave Pix se identifica sozinha** entre CPF, CNPJ, email, telefone e aleatória.

## 12.8 Veiculação e verba

Uma publicação pode ser **orgânica e impulsionada ao mesmo tempo**, e na prática é o caso comum: nasce orgânica e ganha verba depois. São dois sinalizadores independentes, não um campo de escolha única. Pelo menos um precisa estar ligado.

Publicação impulsionada tem **verba própria**. A soma do mês aparece no escopo do planejamento e vira lançamento de mídia no financeiro.

## 12.9 O que o protótipo faz e o produto não deve copiar

O planejamento roda isolado do shell por conveniência de protótipo, com estado próprio. **No produto eles compartilham o mesmo estado e os mesmos componentes**: a demanda do planejamento é a mesma entidade `Demanda`, e o editor é uma tela do app, não um aplicativo à parte.

Da mesma forma: dados de exemplo, consulta simulada de CNPJ e CEP, e a ausência de servidor são artifícios do protótipo. O comportamento é especificação; a arquitetura interna não é.

---

# PARTE 13: DO DOCUMENTO AO CÓDIGO

As telas estão desenhadas e navegáveis em dois protótipos. O que falta é decidir como construir e em que ordem.

## 13.1 O que existe hoje

| Artefato | Estado |
|---|---|
| Documento de estrutura (este) | 68 decisões, 12 partes |
| Diagramas | ER, navegação, máquinas de estado |
| Protótipo do app | 15 telas navegáveis, com o editor de planejamento embutido e a operação real de 2026 dentro |
| Protótipo de planejamento | Publicado à parte também, para testar o link de aprovação no celular |

**Os dados são reais.** 13 clientes, 262 lançamentos de janeiro a setembro de 2026, 16 despesas recorrentes, 11 notas fiscais e 9 guias. Setembro bate com a planilha em receita bruta, taxas, imposto, fornecedores, saldo da operação e custos totais.

**Os dois protótipos conversam.** O shell abre o editor por dentro e recebe de volta o resumo do mês, que executa o contrato de espelhamento da parte 12.3: marca o checklist da demanda e fecha o planejamento quando tudo é aprovado.

Os protótipos são **especificação executável**, não base de código. Eles resolvem a pergunta "como isso se comporta". Não resolvem dado real, permissão, upload nem multi-tenant.

## 13.2 A ordem de construção

**Fase 0, uma semana:** decisão de stack, projeto vazio de pé, autenticação, `organizacao_id` em tudo desde a primeira migração, e os tokens do design system no código.

**Fase 1, a fatia que prova o sistema:** cliente → contrato → planejamento do mês → demanda com sub-demandas → publicação com as quatro partes → link de aprovação.

É a fatia certa porque é **vertical**: atravessa cadastro, produção, aprovação e a tela externa. Se ela funcionar com um cliente real por um mês, o resto é repetição de padrão conhecido. Se quebrar, quebra cedo.

**O teste de sucesso da fase 1:** um mês de conteúdo de um cliente real inteiro dentro da plataforma, sem WhatsApp no meio da aprovação.

**Fase 2:** projetos com template, Meu trabalho, calendário, arquivos.

**Fase 3:** comercial (leads, propostas, contratos) e financeiro.

**Fase 4:** relatórios, dashboard, time.

**O que fica fora da v1, já decidido:** dark mode com rampa própria, integração com Meta, WhatsApp, notificação por email, timesheet, white-label ligado.

## 13.3 O erro a evitar

As tentativas anteriores morreram por construir em largura: um pouco de CRM, um pouco de financeiro, um pouco de demanda, nada inteiro. Sistema pela metade em cinco módulos não serve para nada; um módulo inteiro já substitui uma planilha.

## 13.4 Migração dos dados

A operação de hoje vive em planilha e no Trello. A análise da planilha de 2026 definiu o que entra:

**Financeiro retroativo entra, e vale.** É tabular, sem relação complexa, e sem ele o gráfico de seis meses nasce vazio e o ponto de equilíbrio não existe.

**Clientes e contratos entram com curadoria.** Ativos e os que podem voltar. Cliente que sumiu há dois anos polui a lista para sempre.

**Produção do Trello não entra.** Demanda concluída de 2024 não serve dentro do sistema, e demanda em aberto são poucas, recriadas em uma tarde. O Drive fica onde está, coerente com a decisão 43.

**Duas decisões pendentes antes de importar:** o que fazer com lançamento retroativo já pago (entra como histórico, fora dos alertas) e se o financeiro antigo entra por competência ou por caixa, já que a planilha mistura os dois.

**Três inconsistências que a importação vai denunciar**, achadas na análise: um CNPJ com 13 dígitos, nomes de cliente divergentes entre abas e um mesmo CNPJ aparecendo com dois nomes diferentes.

**Achado de segurança:** a planilha guarda senha de portal em célula aberta. Isso sai de lá independente do sistema.

## 13.5 Telas de borda

### Entrada no sistema

Cinco estados numa tela só: **login**, **recuperar senha**, **link enviado**, **convite recebido** e **primeiro acesso**.

**A regra que importa no recuperar senha:** a resposta é igual existindo a conta ou não ("se existir uma conta com esse email, o link chega"). Dizer "email não cadastrado" entrega quem é cliente de quem, e isso vaza informação comercial da Hoop.

O link de redefinição vale 1 hora e é de uso único.

**Convite** mostra quem convidou, para qual organização e com qual cargo, antes de pedir qualquer coisa. **Primeiro acesso** pede só três campos: como quer ser chamado, nome de usuário e senha, com indicador de força. O resto do perfil espera.

### Detalhe do arquivo

Pré-visualização, vínculo, quem enviou e **lista de versões, da mais nova para a mais velha**, cada uma com autor, data, motivo e download próprio.

**Substituir arquivo cria versão nova, nunca sobrescreve**, e restaurar uma versão antiga gera outra versão por cima em vez de apagar. Cliente pedir "manda o de antes" é rotina, e sem versionamento isso vira busca no WhatsApp.

Arquivar tira da lista e mantém acessível pelo histórico, coerente com a decisão 12 (nada é excluído).

### Erro e permissão

Três estados, cada um com saída:

**Sem acesso** diz qual é o perfil de quem tentou e oferece pedir acesso a quem administra, em vez de só barrar.

**Não encontrado** lembra que nada é excluído no sistema, então o item provavelmente existe, e leva à busca.

**Link expirado**, na tela do cliente, explica que o link vale enquanto o mês está aberto e oferece pedir um novo. Do lado da Hoop, gera link novo e avisa o contato.

