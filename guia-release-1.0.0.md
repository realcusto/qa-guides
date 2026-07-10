# Guia de Teste para QA — Release 1.0.0

Este guia foi escrito para o time de QA testar as novidades **somente pela tela**,
sem precisar de conhecimento técnico. Cada teste tem **Objetivo**, **Passo a
passo** e **Resultado esperado**.

## Como usar este guia

- Faça login no ambiente de testes indicado pelo time.
- No topo da tela existe o botão **"Módulos"**. É por ele que você troca entre
  **Real Preço**, **Real Plano**, **Real Análise**, **Real Controle** e
  **Consultoria**.
- Marque cada cenário como ✅ (passou), ❌ (falhou) ou ⚠️ (passou com ressalva) e
  anote o que viu de diferente do esperado (de preferência com print).
- Se algum menu não aparecer, avise o time — pode ser uma trava de permissão ou
  uma feature flag desligada.

### Pré-requisitos gerais

- Um usuário com acesso aos módulos **Real Preço** e **Real Controle**.
- Massa de dados de teste: ao menos alguns **clientes/fornecedores**, **produtos**,
  **vendas** e **contas a pagar/receber** já cadastrados.

---

## 1. Real Análise (integração com ERP)

> Módulo: **Módulos → Real Análise**.
> Pré-requisito: ter credenciais de teste de pelo menos um ERP (Bling, Tiny, Omie
> ou Nomus). Peça ao time.

### 1.1 — Acessar o módulo

**Objetivo:** confirmar que o módulo aparece e abre.
**Passo a passo:**

1. Clique em **Módulos** no topo.
2. Selecione **Real Análise**.

**Resultado esperado:** abre a tela de **Visão geral** do Real Análise, com o menu
lateral mostrando: Visão geral, Integrações, Tratamento, Fluxo de Caixa e D.R.E.

### 1.2 — Conectar um ERP

**Objetivo:** conectar um ERP de teste.
**Passo a passo:**

1. No menu, abra **Integrações**.
2. Escolha o ERP (ex.: Bling) e clique em **conectar**.
3. Faça o login/autorização na tela do ERP (quando for OAuth) ou informe a chave de
   API (Omie/Nomus).
4. Volte para a tela de Integrações.

**Resultado esperado:** o ERP aparece como **conectado**. O botão **Testar** indica
sucesso. É possível **Sincronizar** e **Desconectar**.

### 1.3 — Sincronizar e ver os dados

**Objetivo:** trazer os lançamentos do ERP.
**Passo a passo:**

1. Em **Integrações**, clique em **Sincronizar**.
2. Aguarde a conclusão.

**Resultado esperado:** a sincronização termina sem erro e passam a existir dados
para os relatórios (Fluxo de Caixa / D.R.E.).

### 1.4 — Tratamento (categorias De-Para)

**Objetivo:** mapear categorias do ERP para as seções do relatório.
**Passo a passo:**

1. Abra **Tratamento**.
2. Para uma categoria vinda do ERP, defina a seção macro e o **Tipo**
   (Receita × Gasto).
3. Saia da tela e volte.

**Resultado esperado:** o mapeamento e o tipo continuam salvos ao voltar. O tipo
escolhido reflete em Receita ou Gasto nos relatórios.

### 1.5 — Fluxo de Caixa

**Objetivo:** validar o relatório de Fluxo de Caixa.
**Passo a passo:**

1. Abra **Fluxo de Caixa**.
2. Troque a **granularidade** (dia / semana / mês / trimestre / ano).
3. **Clique em um período** (uma coluna) para ver o detalhamento (drill-down).
4. Role a tabela para os lados.
5. Use **Exportar** (Excel e PDF).

**Resultado esperado:** os valores mudam conforme a granularidade; o clique abre o
detalhe do período; a **coluna Total** fica fixa à direita ao rolar; o arquivo
exportado abre corretamente.

### 1.6 — D.R.E.

**Objetivo:** validar a D.R.E.
**Passo a passo:**

1. Abra **D.R.E.**.
2. Confira os **KPIs** no topo.
3. Troque a granularidade e as casas decimais (nas Configurações).

**Resultado esperado:** os números batem com o Fluxo de Caixa para o mesmo período;
agrupamento automático e DFC por atividade exibidos corretamente.

---

## 2. Boletos bancários

> Módulo: **Real Controle → Financeiro → Boletos**.
> Pré-requisito: dados de cedente/conta e um **certificado digital** de teste.
> ⚠️ Boletos está atrás de uma **feature flag de desenvolvimento** e de uma
> permissão. Confirme com o time se o ambiente está com a flag **ligada** e se o
> usuário tem acesso; caso contrário, o menu **Boletos** não aparece.

### 2.1 — Configurar a empresa emissora (wizard)

**Objetivo:** cadastrar cedente, conta, convênio e certificado.
**Passo a passo:**

1. Acesse **Boletos**.
2. Inicie o **assistente de cadastro** da empresa emissora.
3. Preencha cedente, conta bancária e convênio.
4. Faça o **upload do certificado digital**.

**Resultado esperado:** a empresa fica configurada; o certificado aparece na lista
**por convênio** na linha da conta.

### 2.2 — Aviso de certificado vencido

**Objetivo:** confirmar o alerta visual.
**Passo a passo:**

1. Use (ou peça ao time) um certificado já **vencido**.
2. Olhe a lista de certificados/contas.

**Resultado esperado:** aparece um **aviso visual de vencido** no certificado.

### 2.3 — Emissão bloqueada sem certificado

**Objetivo:** confirmar a trava de segurança.
**Passo a passo:**

1. Em uma conta **sem certificado válido**, tente emitir um boleto.

**Resultado esperado:** a emissão fica **bloqueada** e há mensagem orientando a
configurar o certificado.

### 2.4 — Emitir, consultar e baixar PDF

**Objetivo:** emitir um boleto e acompanhar.
**Passo a passo:**

1. Com certificado válido, emita um boleto (a partir de uma venda).
2. Veja a lista de boletos e o **status**.
3. Clique para **baixar o PDF**.

**Resultado esperado:** o boleto é criado, o status atualiza e o PDF abre.

### 2.5 — Cancelar boleto não registrado

**Objetivo:** validar o cancelamento.
**Passo a passo:**

1. Em um boleto **ainda não registrado**, use a ação de **cancelar**.

**Resultado esperado:** o boleto é cancelado e o status reflete o cancelamento.

---

## 3. NFC-e

> ⚠️ A NFC-e está atrás de uma **feature flag de desenvolvimento**. Confirme com o
> time se o ambiente está com a flag **ligada**; caso contrário, não haverá tela
> de NFC-e para testar.

### 3.1 — Configurar empresa/série

**Objetivo:** validar a configuração de NFC-e.
**Passo a passo:**

1. Acesse a configuração de NF e abra a parte de **NFC-e**.
2. Cadastre a empresa e a(s) **série(s)** pelo assistente (wizard).

**Resultado esperado:** a empresa/série fica salva, exibida em cards de
configuração.

### 3.2 — Emitir e gerenciar a NFC-e

**Objetivo:** percorrer o fluxo de emissão.
**Passo a passo:**

1. A partir de uma venda, **prepare** e **confirme** a NFC-e.
2. Consulte o **status**.
3. **Baixe o PDF e o XML**.
4. Teste **cancelar** a NFC-e.

**Resultado esperado:** cada etapa conclui sem erro; PDF/XML baixam; status reflete
emissão e cancelamento.

---

## 4. Quadro de Produção v2

> Módulo: **Real Controle → Produção → Quadro de Produção**.

### 4.1 — Quadro e status

**Objetivo:** validar o quadro novo.
**Passo a passo:**

1. Acesse **Quadro de Produção**.
2. Crie uma ordem de produção.
3. Use as **setas ↑↓** para **reordenar os status** das colunas.

**Resultado esperado:** a ordem é criada; os status reordenam e a nova ordem
permanece.

### 4.2 — Relatório de separação (PDF e Excel)

**Objetivo:** gerar o relatório de separação.
**Passo a passo:**

1. Abra o **Relatório de Separação** (picking list).
2. Ajuste as colunas/configurações disponíveis.
3. **Exporte em PDF** e depois **em Excel**.

**Resultado esperado:** os dois arquivos abrem corretamente; no Excel o cabeçalho
sai em cinza.

### 4.3 — Modos de produção do semi-acabado

> Módulo: **Real Preço → Produtos → Produto semi-acabado**.

**Objetivo:** validar os modos Fantasma / Estoque / Híbrido.
**Passo a passo:**

1. Edite um **produto semi-acabado** e escolha o **modo de produção**.
2. Salve e crie uma ordem de produção que use esse semi-acabado.
3. No modo **Híbrido**, deixe parte do item em estoque.

**Resultado esperado:** o modo é salvo; no modo Híbrido o sistema **consome o que
há em estoque e produz o restante** (split).

### 4.4 — Inativar e excluir (Real Preço)

> Esta validação acontece em **Real Preço → Materiais** (`/materials`) e **Real
> Preço → Produtos → Produto semi-acabado** (`/semi-products`).

**Objetivo:** validar soft-delete (Inativar) e exclusão definitiva.
**Passo a passo:**

1. Em um material/semi-acabado, use **Inativar**; depois troque o filtro de
   status para **Inativos** e confirme que ele aparece lá.
2. **Reative** o item e confirme que ele volta para **Ativos**.
3. Em outro item **sem uso**, use a **exclusão definitiva**.
4. Tente a **exclusão definitiva** de um item que já tem movimentação de estoque.

**Resultado esperado:** o inativado some das listas ativas mas mantém histórico e
pode ser reativado; a exclusão definitiva remove o item e limpa o estoque
relacionado; quando há movimentação que não pode ser apagada, a exclusão é
**bloqueada** com orientação para inativar.

---

## 5. Quadro de Expedição v2

> Módulo: **Real Controle → Expedição → Quadro de Expedição**.

### 5.1 — Gerar expedição a partir de vendas

**Objetivo:** criar ordens de expedição.
**Passo a passo:**

1. Acesse **Quadro de Expedição**.
2. Veja as **vendas pendentes** e **gere** ordens a partir delas.

**Resultado esperado:** as ordens aparecem no quadro a partir das vendas
selecionadas.

### 5.2 — Ações em lote e status

**Objetivo:** validar as ações do quadro.
**Passo a passo:**

1. Selecione várias ordens e teste **concluir**, **reverter** e **duplicar**.
2. **Reordene os status** das colunas.

**Resultado esperado:** cada ação afeta as ordens selecionadas; a ordem dos status
permanece após reordenar.

### 5.3 — Relatório de romaneio

**Objetivo:** gerar o romaneio.
**Passo a passo:**

1. Abra o **Relatório** de expedição.

**Resultado esperado:** o romaneio é exibido/exportado corretamente.

---

## 6. Relatório de Vendas e Visão Geral

> Módulo: **Real Controle → Vendas → Relatório de Vendas**.

### 6.1 — Relatório dimensão × métrica

**Objetivo:** montar um relatório.
**Passo a passo:**

1. Acesse **Relatório de Vendas**.
2. Escolha uma **dimensão** (ex.: cliente, produto, vendedor) e uma **métrica**
   (ex.: faturamento, quantidade).

**Resultado esperado:** o relatório recalcula conforme a combinação escolhida.

### 6.2 — Visão Geral (insights)

**Objetivo:** validar o painel de insights.
**Passo a passo:**

1. Abra a tela de **Visão Geral** de vendas.

**Resultado esperado:** o painel destaca automaticamente os principais insights das
vendas, sem áreas quebradas/vazias.

### 6.3 — Badges e barra de ações nas vendas

**Objetivo:** conferir os ajustes visuais.
**Passo a passo:**

1. Em **Pedidos de venda**, observe os **badges de NF**.
2. Selecione vendas e veja a **barra de ações em massa**.

**Resultado esperado:** badges de NF claros; barra de ações mais enxuta.

---

## 7. Financeiro — baixa e antecipação

> Módulo: **Real Controle → Financeiro → Contas a pagar / Contas a receber**.

### 7.1 — Baixar 1 título

**Objetivo:** validar o novo modo de baixa.
**Passo a passo:**

1. Em **Contas a pagar**, abra o menu **"..."** de um título e clique **Baixar**.
2. Preencha a **Data de pagamento**.
3. Observe os 3 botões: **Baixar**, **Atualizar** e **Cancelar**.
4. Clique **Baixar**.

**Resultado esperado:** o título fica baixado/pago com a data informada.

### 7.2 — Estorno de baixa

**Objetivo:** reverter um pagamento baixado.
**Passo a passo:**

1. Em um título já baixado, use **estornar a baixa**.

**Resultado esperado:** o título volta ao estado de em aberto.

### 7.3 — Propagar valor na recorrência

**Objetivo:** atualizar a mesma recorrência.
**Passo a passo:**

1. Edite o valor de um título que faz parte de uma recorrência.
2. Confirme a opção de **propagar o novo valor** às demais contas da recorrência.

**Resultado esperado:** as parcelas futuras da recorrência assumem o novo valor.

### 7.4 — Antecipação (contas a receber)

**Objetivo:** validar o fluxo de antecipação.
**Passo a passo:**

1. Em **Contas a receber**, antecipe um título.
2. Dê um desfecho: **Recebido** ou **Não recebido (perda)**.
3. Teste também **estornar a antecipação**.
4. Repita selecionando **vários títulos** (ações **em lote**).

**Resultado esperado:** os estados mudam corretamente (Antecipado → Recebido / Não
recebido); o estorno volta o título; as ações em lote afetam todos os selecionados.

### 7.5 — Colunas e visualizações rápidas

**Objetivo:** validar a tabela.
**Passo a passo:**

1. Observe a coluna que mostra **Cliente** (em receber) ou **Fornecedor** (em
   pagar) e as colunas de **Ocorrência** e **Parcela**.
2. Troque entre as **visualizações rápidas** (colunas/status/período).
3. Selecione vários títulos e use a **edição em massa** (veja o contador no
   seletor).

**Resultado esperado:** as colunas mudam conforme o tipo; as visualizações aplicam
o conjunto certo de colunas/filtros; a edição em massa altera todos os
selecionados.

---

## 8. Fluxo de Caixa e D.R.E. (Real Controle)

> Módulo: **Real Controle → Financeiro**.

### 8.1 — Fluxo de Caixa

**Objetivo:** validar granularidade, gráfico e coluna fixa.
**Passo a passo:**

1. Abra o **Fluxo de Caixa**.
2. Troque a granularidade, incluindo **Trimestre** e **Ano**.
3. Escolha o período pelo **calendário**.
4. Role a tabela para os lados e observe a **coluna Total fixa**.
5. Veja o **gráfico abaixo da tabela**.

**Resultado esperado:** os valores recalculam; a coluna Total fica fixa com sombra;
o gráfico mostra realizado e previsão.

### 8.2 — D.R.E.

**Objetivo:** validar granularidade e detalhamento.
**Passo a passo:**

1. Abra a **D.R.E.**.
2. Troque a granularidade e os **toggles**.
3. **Detalhe as categorias por conta** do plano.
4. Ative **ocultar zerados** na granularidade mais profunda.
5. **Exporte**.

**Resultado esperado:** o detalhamento abre as contas netas; "ocultar zerados"
some com linhas zeradas só no nível profundo; a exportação abre corretamente.

---

## 9. Edição rápida (inline) nas tabelas

> Módulo: **Real Preço** (ex.: **Materiais**, **Produto semi-acabado**,
> **Preços**).

### 9.1 — Editar células como planilha

**Objetivo:** validar a edição rápida.
**Passo a passo:**

1. Abra **Materiais** (ou Preços).
2. Ative o **modo de edição rápida**.
3. Edite uma célula e use as **setas do teclado** para mover; comece a digitar e
   veja que **substitui** o conteúdo.
4. Edite várias células (note a **marcação de alteração**).
5. Clique no **único botão de salvar**.

**Resultado esperado:** a navegação por teclado funciona; o salvamento grava tudo
de uma vez e sai do modo de edição automaticamente.

### 9.2 — Busca avançada

**Objetivo:** validar a busca avançada.
**Passo a passo:**

1. Abra a **busca avançada** de uma tabela.

**Resultado esperado:** já abre com a **1ª coluna pesquisável selecionada** e uma
condição padrão; existe a opção **"selecionar todos do dia"**.

---

## 10. D.R.E. do Kit

> Módulo: **Real Preço → Kits → abrir um kit**.

**Objetivo:** validar o D.R.E. do kit.
**Passo a passo:**

1. Abra um **kit** com produtos e impostos configurados.
2. Veja o **D.R.E.** do kit.

**Resultado esperado:** os valores batem com o motor de precificação dos produtos;
o **imposto sobre a venda** e o **imposto sobre o resultado** aparecem separados.

---

## 11. Estorno de recebimento de compra

> Módulo: **Real Controle → Compras → Pedidos de Compra**.

### 11.1 — Estornar um recebimento

**Objetivo:** desfazer o recebimento de uma compra.
**Passo a passo:**

1. Em um pedido de compra **já recebido** (cujo estoque **não foi consumido**), use
   o **estorno de recebimento**.

**Resultado esperado:** o recebimento é desfeito; estoque e financeiro voltam ao
estado anterior.

### 11.2 — Bloqueio quando o estoque já foi consumido

**Objetivo:** validar a proteção.
**Passo a passo:**

1. Tente estornar um recebimento cujo estoque **já foi consumido** em produção/
   venda.

**Resultado esperado:** o estorno é **bloqueado** com mensagem explicativa.

---

## 12. Seletor de módulos

> Topo da tela, botão **Módulos**.

**Objetivo:** validar a exibição dos módulos sem acesso.
**Passo a passo:**

1. Com um usuário que **não tem acesso a todos os módulos**, abra **Módulos**.

**Resultado esperado:** aparecem **os 5 módulos**; os sem acesso ficam **opacos com
cadeado** (não clicáveis); os com acesso navegam normalmente.

---

## 13. Plano Financeiro — fonte do realizado

> Módulo: **Real Plano → Plano Financeiro** (`/planning/financial-plan`).

**Objetivo:** validar a troca da fonte do "realizado" no plano.
**Passo a passo:**

1. Abra um plano financeiro.
2. Localize o seletor de **fonte do realizado** e escolha **Manual**.
3. Troque para **Real Controle (interno)** e observe o realizado.
4. Troque para **ERP** (exige uma integração conectada no Real Análise).

**Resultado esperado:** ao trocar a fonte, o **realizado do plano é recalculado /
re-mapeado** conforme a origem escolhida (planilha manual, dados internos do Real
Controle, ou ERP via Real Análise), sem perder o planejado.

---

## 14. Busca no plano de contas e cadastro rápido de parceiro

> Módulo: **Real Controle → Financeiro → Contas a pagar / Contas a receber /
> Extrato**; também no **Pedido de vendas**.

**Objetivo:** validar a pesquisa de contas e o cadastro rápido de
cliente/fornecedor nos formulários financeiros.
**Passo a passo:**

1. Abra o cadastro de uma **conta a pagar** (ou a receber).
2. No campo de **conta (plano de contas)**, comece a **digitar**: a lista deve
   filtrar em tempo real e mostrar o **caminho completo** de cada conta.
3. Apague a pesquisa: a lista deve voltar ao **modo árvore**.
4. No campo **Cliente/Fornecedor**, use o botão **"+"** para **cadastrar um
   parceiro na hora** (só o nome) e confirme que ele já fica selecionado.
5. Repita o teste do botão **"+"** no **Pedido de vendas**.

**Resultado esperado:** a busca filtra as contas com o caminho completo e volta à
árvore ao limpar; o cadastro rápido cria o parceiro (tipo Empresa) sem sair da tela
e o seleciona no campo.

---

## Itens sem teste de QA

- **Design system** (página `/admin/design-system`): é referência visual interna;
  validação resume-se a conferir que as telas continuam com aparência consistente.
- **Infraestrutura** (bastion, auto-pause de DEV, divisão do schema): não há nada a
  testar pela interface.
