# Sistema All-in-One · Hoop Branding

Plataforma de gestão da Hoop: CRM, propostas, contratos, produção, planejamento de conteúdo com aprovação do cliente, financeiro, relatórios e arquivos.

Este repositório guarda a **especificação** e os **protótipos navegáveis**. Ainda não há código de produção.

## O que tem aqui

```
docs/
  estrutura-telas.md    especificação completa: 114 decisões, telas, dicionário de dados,
                        permissões, linguagem visual, vocabulário e ordem de construção
  diagramas.md          modelo de dados (ER), navegação e máquinas de estado, em Mermaid
prototipo/
  index.html            o sistema inteiro, navegável, com o editor de planejamento embutido
  planejamento.html     o editor de planejamento e o link de aprovação, isolados
```

Os protótipos são arquivos únicos, sem dependência e sem servidor. Abra no navegador.

## Como ler, na ordem

1. **`docs/estrutura-telas.md`**, começando pelo sumário e pela tabela de decisões. Ela responde quase toda pergunta de "por que assim".
2. **`prototipo/index.html`**, navegando pelas telas com os dados reais de 2026 dentro.
3. **`docs/diagramas.md`**, para entender o modelo antes de modelar o banco.
4. **Parte 13 da especificação**, que define a ordem de construção.

## Decisões que mudam a arquitetura

Estas não são preferências, são restrições. Estão detalhadas na especificação.

- **Multi-tenant desde a primeira migração.** `organizacao_id` em toda tabela. Adicionar depois é reescrita.
- **O sistema não interpreta legislação tributária.** A alíquota vem da guia apurada pela contabilidade, por divisão, e congela no lançamento.
- **Receita bruta não é receita.** Taxa, imposto e repasse saem dela; o resultado usa o líquido.
- **Espelhamento tem sentido único.** A parte da publicação alimenta o item de checklist. O item nunca escreve de volta.
- **Nada é excluído.** Arquivamento em vez de exclusão, e versão nova em vez de sobrescrita.
- **Permissão por ação, não por tela.** 36 ações contra 5 perfis, e o que não pode ver não aparece nem por link direto.

## O que o protótipo faz e o produto não deve copiar

O editor de planejamento roda isolado dentro do shell por conveniência de protótipo. **No produto é uma tela do mesmo app, com o mesmo estado e os mesmos componentes.** Dados de exemplo, consulta simulada de CNPJ e CEP e a ausência de servidor também são artifício. O comportamento é especificação; a arquitetura interna não é.

## Estado atual

- Especificação na versão 3.8, com 114 decisões fechadas
- Diagramas na versão 3.3, incluindo o domínio financeiro
- 32 telas desenhadas e navegáveis
- Protótipo alimentado com a operação real de 2026, conferido contra a planilha de fluxo financeiro

## Próximos passos

1. Decidir a stack
2. Fase 0: projeto de pé, autenticação, `organizacao_id`, tokens do design system
3. Fase 1, a fatia que prova o sistema: cliente → contrato → planejamento do mês → demanda → publicação → link de aprovação

O teste de sucesso da fase 1 está escrito na especificação: um mês inteiro de um cliente real dentro da plataforma, sem WhatsApp no meio da aprovação.
