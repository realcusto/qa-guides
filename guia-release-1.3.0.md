# Guia de Teste para QA — Release 1.3.0

Este guia cobre **as novidades e correções que vão para produção na versão
1.3.0**. Tudo aqui é testável **somente pela tela**, sem conhecimento técnico.
Cada teste tem **Objetivo**, **Passo a passo** e **Resultado esperado**.

> Versão focada em **Real Controle**: um novo relatório de **Necessidade de
> Reposição** (estoque por dias de autonomia), **Limite de crédito** do cliente,
> **preferências comerciais** por cliente (lista de preço / vendedor / roteiro),
> **vendedor restrito aos próprios clientes**, e um novo **Roteiro de entrega com
> mapa** na Expedição. Também traz melhorias no **Real Análise** (saldo real das
> contas, categorias de "posição / fora do caixa", sincronizar por período), no
> **Real Plano** (janela de contratação/desligamento na folha, import em lote) e
> ajustes **Fiscais** (Código de Benefício `cBenef`, diálogo de "regras
> faltando", operações/regras padrão).

## Como usar este guia

- Faça login no ambiente de testes indicado pelo time.
- No topo (ou na lateral) da tela existe o seletor de **"Módulos"**, por onde se
  troca entre **Real Preço**, **Real Plano**, **Real Análise**, **Real Controle**
  e **Consultoria**.
- Marque cada cenário como ✅ (passou), ❌ (falhou) ou ⚠️ (passou com ressalva) e
  anote o que viu de diferente (de preferência com print).
- Se algum menu não aparecer, avise o time — pode ser trava de permissão ou uma
  feature ainda não liberada para o seu usuário.

### Pré-requisitos gerais

> **Confirme com o time que o ambiente está com a versão 1.3.0 já publicada.**

- Um usuário **administrador** e um usuário com acesso a **Real Controle**,
  **Real Análise** e **Real Plano**. Algumas telas só aparecem para
  administradores.
- Massa de dados à mão: **clientes/fornecedores com endereço preenchido**,
  **produtos** e **estoque com movimentações de saída/venda** (o giro só aparece
  se houve consumo no período), **vendas em aberto**, **títulos a receber** e
  **plano de contas**.
- Para o **Roteiro de entrega / mapa**: **internet liberada** no ambiente (o
  botão "Sugerir roteiro" consulta serviços de mapa **externos** — OpenStreetMap/
  Nominatim e OSRM), clientes com **endereço completo**, e o **perfil fiscal da
  empresa** (usado como depósito de partida) com **endereço/CEP** preenchidos.
- Para **vendedor restrito**: um usuário **vendedor** com alguns clientes
  vinculados a ele, e um **celular** com o app para a parte do **App Mobile**.
- Para o **saldo real** do Real Análise: pelo menos uma **conexão de ERP**
  (idealmente **Omie**) de teste, já sincronizada — é o Omie que informa o
  "Saldo em Contas".

---

## 1. Real Controle — Necessidade de Reposição (novo)

> Onde fica: **Real Controle → Estoque → Necessidade de Reposição**.

Novo relatório que mede o estoque por **autonomia (dias de cobertura)** em vez de
só mínimo/máximo fixos. Ele calcula o **giro** (consumo por dia no período),
quantos **dias de autonomia** cada item tem e **quanto sugerir comprar/fabricar**.

### 1.1 — Abrir o relatório e ler as colunas

**Objetivo:** confirmar que o relatório carrega e traz as colunas novas.

**Passo a passo:**

1. Abra **Necessidade de Reposição**.
2. Confira o período no topo (por padrão os **últimos 30 dias**) e observe as
   colunas: **Giro** (consumo/dia), **Autonomia** (dias), **Alvo** (dias
   desejados), **Prazo** (lead time), **Lote**, **Sugestão** (quanto pedir) e
   **Status**.

**Resultado esperado:** cada item mostra o **giro** = consumo do período ÷ dias
do período; a **autonomia** = estoque atual ÷ giro; e uma **sugestão** de compra/
fabricação. Itens **sem consumo** no período aparecem como **"sem giro"** e
autonomia **∞/—**.

### 1.2 — Status e ordenação por urgência

**Objetivo:** confirmar as cores/rótulos de status e a ordenação.

**Passo a passo:**

1. Observe a coluna **Status** dos itens.
2. Repare na ordem das linhas.

**Resultado esperado:** o status é **Urgente** (autonomia ≤ prazo de entrega),
**Atenção** (entre o prazo e o alvo), **OK** (autonomia ≥ alvo) ou **Sem giro**.
A lista vem **ordenada por urgência** (menor autonomia primeiro); os itens **sem
giro** ficam no **fim**.

### 1.3 — Tipo de reposição (Comprar × Fabricar)

**Objetivo:** confirmar que o sistema sugere comprar ou fabricar conforme o item.

**Passo a passo:**

1. Compare uma **matéria-prima** com um **produto acabado / semiacabado**.

**Resultado esperado:** matéria-prima aparece como **COMPRAR** e produto
acabado/semiacabado como **FABRICAR**.

### 1.4 — Sugestão arredondada ao lote

**Objetivo:** confirmar o arredondamento da sugestão pelo lote.

**Passo a passo:**

1. Num item com **Lote** maior que 1 (ex.: 10), veja o valor da **Sugestão**.

**Resultado esperado:** a sugestão é **arredondada para cima** até o próximo
múltiplo do lote (ex.: precisa de 277 com lote 10 → sugere **280**). Com **lote
1**, a sugestão sai sem arredondar.

### 1.5 — Editar Alvo / Prazo / Lote por item (e aplicar a todos)

**Objetivo:** confirmar a edição inline e as simulações globais.

**Passo a passo:**

1. Clique numa célula de **Alvo**, **Prazo** ou **Lote** de um item, altere o
   valor e salve.
2. No topo, use uma das simulações **"igual para todos"** (Alvo/Prazo/Lote) e veja
   o efeito na tabela.

**Resultado esperado:** a edição por item recalcula a **Sugestão** e o **Status**
daquela linha e **persiste**. As simulações globais aplicam o mesmo valor a
**todos** os itens (recalculando na hora), para você comparar cenários.

### 1.6 — Filtros, colunas e exportação

**Objetivo:** conferir filtros, mostrar/ocultar colunas e export.

**Passo a passo:**

1. Ligue o filtro **"só o que precisa"** (mostra apenas itens com sugestão > 0).
2. Filtre por **categoria** (matéria-prima / semiacabado / produto acabado) e use
   a **busca por texto**.
3. Mostre/oculte colunas e **ordene** clicando nos cabeçalhos.
4. **Exporte** em **Excel** e **PDF**.

**Resultado esperado:** o "só o que precisa" **esconde** os itens já abastecidos;
os filtros e a busca funcionam; a escolha de colunas/ordem **persiste** ao
recarregar (F5). Os totais e a exportação consideram **todos** os itens
filtrados, não só a página visível.

---

## 2. Real Controle — Limite de crédito do cliente (novo)

> Onde fica: **Real Controle → Cadastros → Cliente/Fornecedor** (grupo **"Limite
> de crédito"**) e a tela de **venda**.

### 2.1 — Configurar o limite e o modo

**Objetivo:** confirmar os novos campos de crédito no cadastro do cliente.

**Passo a passo:**

1. Abra um **cliente** e edite.
2. No grupo **"Limite de crédito"**, informe um **valor de limite** e escolha o
   **modo**: **Avisar** (só alerta) ou **Bloquear** (trava as ações).
3. Salve. Depois teste também um cliente **sem limite** (campo vazio).

**Resultado esperado:** os campos salvam. Cliente **sem limite** definido não é
afetado por nenhuma trava/aviso.

### 2.2 — Aviso ao montar o pedido de venda

**Objetivo:** confirmar o banner de crédito na tela da venda.

**Passo a passo:**

1. Crie uma **venda** para um cliente com limite baixo, escolhendo uma condição
   de pagamento **a prazo** que estoure o limite (considerando o que ele já deve
   em aberto).
2. Observe o banner que aparece.

**Resultado esperado:** aparece um **banner âmbar** (modo **Avisar**) ou
**vermelho** (modo **Bloquear**) informando que o crédito foi excedido. O banner
**não impede** salvar a venda (ele só alerta ao montar o pedido).

### 2.3 — Bloqueio nas ações pós-venda (modo Bloquear)

**Objetivo:** confirmar que o modo **Bloquear** trava as ações seguintes.

**Passo a passo:**

1. Com um cliente no modo **Bloquear** e uma venda que **estoura** o limite,
   tente: **Emitir NF-e/NFC-e**, **Enviar para produção** e **Lançar as contas**
   (converter em financeiro).
2. Repita com um cliente no modo **Avisar**.

**Resultado esperado:** no modo **Bloquear**, cada uma dessas ações é **recusada**
com uma mensagem clara em português citando a ação (ex.: "...para emitir a nota
fiscal do..."). No modo **Avisar**, as ações **passam** normalmente (só houve o
aviso).

---

## 3. Real Controle — Preferências comerciais do cliente (novo)

> Onde fica: **Real Controle → Cadastros → Cliente** (grupo **"Vendas e Frete"**).

### 3.1 — Preferências que pré-preenchem a venda

**Objetivo:** confirmar os novos seletores de preferência do cliente.

**Passo a passo:**

1. Abra um **cliente** e, no grupo **"Vendas e Frete"**, preencha **Categoria de
   preço preferida**, **Vendedor preferido** e **Roteiro de entrega preferido**.
   Salve.
2. Crie uma **nova venda** para esse cliente.

**Resultado esperado:** os três campos salvam no cadastro. Ao iniciar a venda para
esse cliente, as preferências **pré-preenchem** o pedido (lista de preço,
vendedor e roteiro), podendo ainda ser trocadas manualmente.

---

## 4. Real Controle — Vendedor restrito aos próprios clientes (novo)

> Onde fica: **Real Controle → Cadastros → Vendedores/Representantes** e o app.

### 4.1 — Ativar a restrição

**Objetivo:** confirmar o novo switch no cadastro do vendedor.

**Passo a passo:**

1. Abra um **vendedor/representante** e ligue o switch **"Só pode vender para seus
   próprios clientes"**. Salve.
2. Garanta que existam clientes com esse vendedor como **vendedor preferido** e
   outros **sem** vínculo com ele.

**Resultado esperado:** o switch salva.

### 4.2 — Efeito no seletor de cliente e na criação da venda

**Objetivo:** confirmar que o vendedor restrito só enxerga/atende seus clientes.

**Passo a passo:**

1. Logado como o **vendedor restrito**, abra o **seletor de cliente** numa venda.
2. Tente criar uma venda para um cliente que **não** é dele.

**Resultado esperado:** o seletor lista **apenas** os clientes cujo **vendedor
preferido** é ele. Criar venda para cliente de outro vendedor é **recusado**. Um
**administrador** (ou usuário não vinculado) continua vendo **todos** os clientes.

---

## 5. Real Controle — Roteiro de entrega + mapa (novo)

> Onde fica: **Real Controle → Expedição → (uma carga)** — painel **"Roteiro de
> entrega"**.

### 5.1 — Definir a sequência de entrega arrastando as paradas

**Objetivo:** confirmar a ordenação das paradas e a ordem de carga derivada.

**Passo a passo:**

1. Abra uma **carga** com itens de **vários clientes**.
2. No painel **"Roteiro de entrega"**, **arraste** as paradas (clientes) para
   definir a **ordem de entrega**.
3. Observe a **ordem de carga** mostrada por parada.

**Resultado esperado:** a **ordem de carga** é o **inverso** da ordem de entrega
(**LIFO**: quem é entregue por **último** é carregado **primeiro**, no fundo do
caminhão; a primeira entrega fica junto à porta). A sequência é salva.

### 5.2 — Sugerir roteiro (mapa e otimização automática)

**Objetivo:** confirmar a sugestão automática de rota e o mapa.

**Passo a passo:**

1. Com pelo menos **2 clientes com endereço**, clique em **"Sugerir roteiro"**.
2. Aguarde o processamento e observe o **mapa**.

**Resultado esperado:** o sistema **geocodifica** os endereços e sugere uma
**ordem de entrega otimizada**. O **mapa** mostra **pinos numerados** na ordem de
entrega, um **pino de origem/depósito** diferente e a **linha da rota**. Paradas
**sem localização** aparecem numa lista **"sem localização"**, separadas, e vão
para o **fim**. (Exige internet — usa mapas externos.)

### 5.3 — Coluna "Ordem de carga" no relatório de expedição

**Objetivo:** confirmar a coluna e a nota explicativa no relatório.

**Passo a passo:**

1. Abra o **relatório de Expedição** e localize a coluna **"Ordem de carga"**.
2. Exporte em **PDF** e **Excel**.

**Resultado esperado:** a coluna **"Ordem de carga"** (antes chamada só "Ordem")
vem **visível por padrão**. No PDF/Excel aparece uma **nota explicando o LIFO**
("quem é entregue primeiro é carregado por último, junto à porta").

---

## 6. Real Controle — Fiscal (cBenef, regras faltando, padrões)

> Onde fica: **Real Controle → Fiscal → Regras Fiscais / Operações Fiscais** e a
> emissão de NF-e.

### 6.1 — Código de Benefício Fiscal (cBenef) na regra

**Objetivo:** confirmar o novo campo `cBenef` na regra fiscal.

**Passo a passo:**

1. Abra/crie uma **Regra Fiscal** e localize o campo **cBenef** (Código de
   Benefício Fiscal), na linha dos campos de **ICMS-ST**.
2. Preencha um código e salve.

**Resultado esperado:** o campo salva e, na emissão de uma NF-e que usa essa
regra, o código de benefício é enviado no item da nota.

### 6.2 — Diálogo "regras faltando" ao emitir

**Objetivo:** confirmar o novo fluxo quando falta regra fiscal.

**Passo a passo:**

1. Tente **emitir uma NF-e** de uma venda cujo **produto × cliente × operação**
   **não** tem regra fiscal cadastrada.

**Resultado esperado:** em vez de um erro simples, abre um **diálogo listando as
combinações sem regra**, cada uma com um link **"Criar regra"** que leva ao
formulário de nova regra **já pré-preenchido** com as condições (produto/UF/
operação etc.). Outros bloqueios não-de-regra também aparecem na lista.

### 6.3 — Restaurar operações/regras padrão

**Objetivo:** confirmar os atalhos de restauração dos padrões.

**Passo a passo:**

1. Em **Operações Fiscais**, use o menu **"Restaurar operações padrão"**.
2. Em **Regras Fiscais**, use o menu **"Restaurar regras padrão"**.

**Resultado esperado:** as operações padrão (agora incluindo **Venda de produção
própria**, **Devolução de compra** e **Compra para industrialização**) e as
regras padrão são criadas **sem duplicar** as existentes. As **regras** nascem
**inativas** (você ativa depois de revisar as alíquotas).

> **Emissão fiscal (homologação):** esta versão traz correções internas na
> emissão (FCP em bloco aninhado, DIFAL/partilha, vICMSSubstituto em CST 60 /
> CSOSN 500). Se possível, **emita algumas notas em homologação** e confirme que
> notas que antes eram **rejeitadas pela SEFAZ/PlugNotas** passam agora.

---

## 7. Real Análise — saldo real, "posição / fora do caixa", sincronizar período

> Onde fica: **Módulos → Real Análise**.

### 7.1 — Categorias de "Posição / fora do caixa"

**Objetivo:** confirmar a nova marcação que tira categorias do Fluxo de Caixa mas
mantém na DRE.

**Passo a passo:**

1. Em **Tratamento → Categorias**, use a coluna/opção **"Posição / fora do
   caixa"** e marque uma categoria de posição (ex.: **Estoque Inicial/Final**).
2. Abra o **Fluxo de Caixa** e a **DRE**.

**Resultado esperado:** a categoria marcada **some do Fluxo de Caixa** mas
**continua na DRE**. Contas que só carregam esse tipo de lançamento (apuração/
transitórias) também são tratadas como **não-caixa**.

### 7.2 — Edição rápida de categorias na DRE (igual ao Fluxo)

**Objetivo:** confirmar a edição inline de categorias também na DRE.

**Passo a passo:**

1. Na **DRE**, selecione **uma única conexão de ERP**.
2. No menu (⋮) de uma linha de categoria, mude **Tipo** (Receita/Gasto/Outro),
   **Mover para seção** ou marque **Posição de estoque**.

**Resultado esperado:** a edição funciona igual à do Fluxo de Caixa; ao salvar, a
**DRE e o Fluxo atualizam** juntos. Com **mais de uma** conexão selecionada, fica
**somente leitura**.

### 7.3 — Saldo real das contas (âncora do realizado)

**Objetivo:** confirmar que o saldo real do ERP ancora o Fluxo realizado.

**Passo a passo:**

1. Numa conexão **Omie** sincronizada (que informa **"Saldo em Contas"**), abra o
   **Fluxo de Caixa realizado**.

**Resultado esperado:** o **saldo inicial/realizado** passa a bater com o **saldo
real** informado pelo ERP na data dele (em vez de ser reconstruído somando todo o
histórico). O **drill-down** por categoria também bate com a célula (categorias do
Omie deixam de cair em "Sem categoria").

### 7.4 — Tiny não fica mais zerado

**Objetivo:** confirmar o realizado onde não há livro-caixa.

**Passo a passo:**

1. Numa conexão **Tiny** (sem livro-caixa), abra o **Fluxo de Caixa** realizado.

**Resultado esperado:** o realizado usa os **lançamentos liquidados** quando não
há livro-caixa — o Tiny **não fica mais zerado**.

### 7.5 — Sincronizar por período

**Objetivo:** confirmar a sincronização manual por intervalo de datas.

**Passo a passo:**

1. Em **Integrações**, clique em **"Sincronizar período"**.
2. Use um atalho (**último mês / últimos 3 meses / este ano**) ou informe **De/
   Até** e confirme.

**Resultado esperado:** a sincronização varre **apenas** os meses do intervalo
escolhido. Intervalo invertido/ inválido não sincroniza nada.

---

## 8. Real Plano — folha com janela, import em lote, export

> Onde fica: **Módulos → Real Plano → (um plano)**.

### 8.1 — Janela de contratação/desligamento na folha

**Objetivo:** confirmar que o custo da pessoa só entra dentro da janela.

**Passo a passo:**

1. Na aba **Folha**, cadastre/edite uma pessoa e preencha **"Contratação"** e
   **"Desligamento"** (o mês dentro do plano; **"Até o fim"** = sem desligamento).
2. Abra o **Fluxo de Caixa** e a **DRE** do plano.

**Resultado esperado:** o custo dessa pessoa só é lançado **dentro da janela**;
meses fora dela ficam **zerados**. O **13º/férias** são **prorrateados** pelos
meses trabalhados. Aparece um **badge** com a janela ("Mês → Mês" ou "→ fim") ao
lado do nome quando a janela difere do plano inteiro.

### 8.2 — Import em lote (mais rápido)

**Objetivo:** confirmar o import de premissas num único envio.

**Passo a passo:**

1. Nas abas **Despesas fixas**, **Folha** ou **Capex/Investimentos**, use o
   **importar** (de premissas cadastradas ou de outro plano).

**Resultado esperado:** o import acontece em **um único envio** (não um a um), com
o aviso **"N itens importados"**. O import passa a enxergar **os cadastros de toda
a organização**, não só os do usuário logado.

### 8.3 — Export do plano na linha do cabeçalho

**Objetivo:** confirmar o menu Exportar reposicionado e os gráficos no PDF.

**Passo a passo:**

1. Abra uma aba de relatório (**DRE**, **Fluxo**, **Ponto de equilíbrio**, **NCG**
   ou **Balanço**) e use o menu **Exportar**.
2. Gere o **PDF** e confira os **gráficos**.

**Resultado esperado:** o menu **Exportar** aparece na **linha do cabeçalho fixo**
do plano (não mais numa div solta). No PDF, o **gráfico** mantém a **proporção**
(não fica esticado).

---

## 9. App Mobile — vendedor restrito

> Onde fica: **App Mobile** (usuário vendedor).

### 9.1 — Vendedor restrito só vê seus clientes no app

**Objetivo:** confirmar que a restrição também vale no celular.

**Passo a passo:**

1. Com um vendedor marcado como **restrito** (teste 4.1), faça login no **app** e
   sincronize.
2. Abra a lista/seleção de **clientes**.

**Resultado esperado:** o app sincroniza e mostra **apenas** os clientes cujo
vendedor preferido é ele — coerente com o comportamento do desktop.

---

## 10. Diversos (aparência e pequenos ajustes)

### 10.1 — Contraste das notificações (toasts)

**Objetivo:** confirmar a legibilidade do texto das notificações.

**Passo a passo:**

1. Faça uma ação que gere uma notificação **com descrição** (texto secundário).

**Resultado esperado:** o **texto de descrição** do toast fica **legível** (com
bom contraste), tanto no tema claro quanto no escuro.

### 10.2 — Aba "Nota fiscal" do produto reorganizada

**Objetivo:** conferir o novo alinhamento dos campos fiscais do produto.

**Passo a passo:**

1. Abra um **produto** e vá na aba **"Nota fiscal"**.

**Resultado esperado:** **Origem**, **NCM** e **CEST** aparecem na **mesma linha**
(grid de 3 colunas), sem a linha separada que a Origem ocupava antes.
