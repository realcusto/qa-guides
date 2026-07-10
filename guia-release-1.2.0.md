# Guia de Teste para QA — Release 1.2.0

Este guia cobre **as novidades e correções que vão para produção na versão
1.2.0**. Tudo aqui é testável **somente pela tela**, sem conhecimento técnico.
Cada teste tem **Objetivo**, **Passo a passo** e **Resultado esperado**.

> É uma versão grande, com bastante coisa nova na parte **Fiscal** do
> **Real Controle** (Operações Fiscais, Regras Fiscais, Nota Avulsa, CC-e,
> Inutilização, Notas Destinadas e SPED), além de **Expedição parcial**,
> melhorias no **Real Análise**, no **Real Plano**, no painel de **Admin** e uma
> nova preferência de **navegação** para todo o sistema.

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

> **Ambiente pré-populado — comece por aqui.** Já existe um org de testes
> **"QA — Release 1.2.0"** com a massa de dados destes testes pronta. **Você
> precisa ser membro para vê-lo:** o time de testes já foi adicionado; se você
> não tiver acesso, **peça ao desenvolvedor do app para te adicionar** (só ele
> adiciona membros). Depois de logar, confirme que o
> org ativo é **"QA — Release 1.2.0"** (troque pelo menu de organização, se
> preciso).
>
> **Boleto, conexão de ERP e emissão de NF-e/SPED** ainda precisam de credenciais
> reais mesmo no org de QA (esses cadastros vêm apenas como exemplo).

- Um usuário **administrador** (para o painel de Admin e para algumas
  configurações). Algumas telas só aparecem para administradores.
- Um usuário com acesso a **Real Controle** (fiscal/financeiro/estoque),
  **Real Análise** e **Real Plano**.
- Massa de dados: **já vem pronta no org de QA** (clientes/fornecedores,
  produtos, estoque, vendas em vários estados, contas a pagar/receber e plano de
  contas). Fora do org de QA, tenha esses cadastros à mão.
- Um usuário **vendedor** e um **celular** com o aplicativo, para a parte do
  **App Mobile** (Expedição).
- Para a parte fiscal "de verdade": credenciais de **homologação/sandbox** da
  PlugNotas e, se possível, um **certificado digital** de teste — peça ao time.
- Para Boleto: uma conta de **cedente** já configurada (PlugBoleto homologação).
- Para Real Análise: ao menos uma **conexão de ERP** (Bling, Tiny ou Omie) de
  teste, já sincronizada.
- Confirme com o time que o ambiente está com **esta versão já publicada**.

---

## 1. Navegação — menu superior OU menu lateral (novo)

> Onde fica: **Módulos → (qualquer módulo)** e **Configurações de usuário**.

### 1.1 — Escolher o layout de navegação

**Objetivo:** confirmar a nova preferência que troca o menu do topo por um menu
na lateral esquerda.

**Passo a passo:**

1. Abra o menu do usuário (canto superior) e vá em **Configurações de usuário**.
2. Procure a seção **"Navegação"** e escolha **"Lateral"**.
3. Volte para qualquer módulo (ex.: Real Controle).
4. Depois volte nas configurações e escolha **"Superior"** de novo.

**Resultado esperado:** com **Lateral**, os mesmos itens de menu que ficavam no
topo passam para uma **barra à esquerda** (com logo em cima e controles do
usuário embaixo). Com **Superior**, volta o menu no **topo**. A escolha
**persiste** ao recarregar a página (F5) e ao trocar de módulo.

### 1.2 — Textos renomeados

**Objetivo:** conferir pequenas renomeações.

**Passo a passo:**

1. Abra o menu do usuário e observe o item de configurações.
2. Vá para a tela inicial (home).

**Resultado esperado:** o antigo **"Configurações"** agora se chama
**"Configurações de usuário"**. Na home, o nome **"Real Custo"** aparece como
**"Real ERP"**.

---

## 2. Real Controle — Operações Fiscais (novo)

> Onde fica: **Real Controle → Fiscal → Operações Fiscais**.

### 2.1 — Criar as operações padrão

**Objetivo:** confirmar o botão que cria as operações fiscais padrão de uma vez.

**Passo a passo:**

1. Abra **Operações Fiscais** (lista provavelmente vazia numa conta nova).
2. Clique em **"Criar operações padrão"** (ou equivalente).

**Resultado esperado:** aparecem **6 operações** padrão: **Venda de mercadoria,
Devolução de venda, Remessa, Bonificação/Brinde, Transferência e
Entrada/Compra**. Clicar de novo **não duplica** (continua com 6).

### 2.2 — Criar/editar uma operação

**Objetivo:** testar o cadastro de uma operação fiscal.

**Passo a passo:**

1. Clique em **Nova** (ou edite uma existente).
2. Preencha **direção** (Saída/Entrada), **finalidade** (Normal/Complementar/
   Ajuste/Devolução), **natureza padrão**, **CFOP padrão** e os campos de
   **intermediador**.
3. Marque/desmarque **"Afeta o financeiro"** e **"Exige documento referenciado"**.
4. Salve.

**Resultado esperado:** a operação salva e aparece na lista. Uma operação de
**Devolução** deve ter **"Exige documento referenciado"** fazendo sentido (será
cobrada na emissão).

---

## 3. Real Controle — Regras Fiscais + Simulador (novo)

> Onde fica: **Real Controle → Fiscal → Regras Fiscais**.

### 3.1 — Cadastrar uma regra fiscal

**Objetivo:** confirmar o formulário completo de regra fiscal.

**Passo a passo:**

1. Clique em **Nova regra**.
2. No grupo **Dimensões de match**, preencha alguns filtros (ex.: **Operação**,
   **UF do emitente**, **NCM**, **Indicador de IE**, **Consumidor final** e a
   nova opção **"Destino com SUFRAMA (ZFM/ALC)?"**). Deixe outros em branco
   (curinga).
3. No grupo **Resultado**, informe **CST/CSOSN**, **CFOP** e **alíquotas**.
4. Nos **situacionais**, experimente **DIFAL**, **FCP**, **ST** e **desoneração
   de ICMS** (motivo 7 SUFRAMA / 9 Outros).
5. (Opcional) Preencha o bloco **Reforma Tributária (IBS/CBS/IS)**.
6. Defina **vigência** (data inicial/final) e salve.

**Resultado esperado:** a regra salva. Se você preencher **alíquota de
PIS/COFINS** num CST que **não** aceita (04–09), o sistema **recusa**; nos CSTs
tributados (01/02/03) a alíquota é **obrigatória**.

### 3.2 — Duplicar / regra idêntica nasce inativa

**Objetivo:** confirmar a proteção contra regras ambíguas.

**Passo a passo:**

1. Selecione uma regra **ativa** e use **"Duplicar"**.
2. Tente também criar/salvar uma regra **idêntica** a uma que já está **ativa**.

**Resultado esperado:** a **cópia** (e a regra idêntica) nasce **inativa**, para
não conflitar com a original. Só passa a valer quando você ativar
manualmente.

### 3.3 — Simulador de tributos

**Objetivo:** ver qual regra o sistema escolhe para uma combinação, sem emitir
nada.

**Passo a passo:**

1. Abra o **Simulador** (a partir de Regras Fiscais).
2. Escolha **cliente**, **produto**, **operação** e, no simulador rico, o **CNPJ
   emitente**.
3. Rode a simulação.

**Resultado esperado:** aparece a **regra escolhida**, o **resultado** (CST/CFOP/
alíquotas/situacionais) e, ao lado, o **fallback do produto** para comparar. Sem
informar o **CNPJ emitente**, situacionais que dependem da UF de origem (ex.:
DIFAL) **não casam** — confirme que o seletor de emitente muda o resultado.

---

## 4. Real Controle — Cadastros (parceiros, documentos, busca por CNPJ)

> Onde fica: **Real Controle → Cadastros** (Clientes/Fornecedores,
> Transportadoras, Motoristas, Veículos).

### 4.1 — Perfil fiscal do parceiro

**Objetivo:** conferir os novos campos fiscais do cliente/fornecedor.

**Passo a passo:**

1. Abra um **cliente/fornecedor** e edite.
2. Observe os campos novos: **Inscrição SUFRAMA**, **Indicador de IE**,
   **Consumidor final** e **Regime tributário**.
3. Troque entre **Pessoa Jurídica** e **Pessoa Física**.

**Resultado esperado:** os campos aparecem. Ao mudar para **Pessoa Física**, os
campos que só fazem sentido para **PJ** (regime/IE) são **limpos/escondidos**.

### 4.2 — Validação de documentos (CPF/CNPJ, placa, CNH)

**Objetivo:** confirmar que documentos inválidos são recusados.

**Passo a passo:**

1. Num cliente/fornecedor, digite um **CNPJ** ou **CPF** com dígito verificador
   **errado** e tente salvar.
2. Num **veículo**, digite uma **placa** inválida (nem padrão antigo nem
   Mercosul).
3. Num **motorista**, digite um **CPF** inválido e uma **categoria de CNH**
   inexistente.

**Resultado esperado:** em todos os casos o sistema **recusa** com mensagem
clara. Documentos **válidos** (dígito correto, placa `ABC1234` ou `ABC1D23`,
categoria de CNH válida) salvam normalmente.

### 4.3 — Busca automática por CNPJ

**Objetivo:** confirmar o preenchimento automático a partir do CNPJ.

**Passo a passo:**

1. Ao **criar** um cliente/fornecedor PJ, digite um **CNPJ válido** no campo.
2. Aguarde a busca.
3. Depois **edite** um parceiro já existente **sem** mexer no CNPJ.

**Resultado esperado:** na criação, os dados da empresa (razão social, endereço
etc.) são **preenchidos automaticamente**. Ao apenas abrir um cadastro existente
para editar, a busca **não** dispara sozinha (não sobrescreve o que já estava).
O mesmo vale para a busca de **CEP** (só busca quando você mexe no campo).

---

## 5. Real Controle — Produtos (campos fiscais)

> Onde fica: **Real Controle → Produtos** (ou onde o produto é cadastrado).

### 5.1 — CST/CFOP saíram do produto

**Objetivo:** confirmar que a tributação do produto agora vem da **Regra
Fiscal**.

**Passo a passo:**

1. Abra um **produto** e vá para a área fiscal.

**Resultado esperado:** os campos de **CST de venda (ICMS/PIS/COFINS/IPI)**,
**CFOP** e **enquadramento de IPI** **não aparecem mais** no cadastro do produto
(agora são definidos pelas Regras Fiscais).

### 5.2 — ST retido por unidade

**Objetivo:** conferir os campos de ST retido, que só aparecem quando aplicável.

**Passo a passo:**

1. Em um produto, marque **"tem substituição tributária"** (ST).
2. Observe os campos **Base ST retido**, **Valor ST retido** e **Alíquota ST
   retido** por unidade.
3. Desmarque a ST.

**Resultado esperado:** os 3 campos de **ST retido por unidade** aparecem
**apenas** quando a ST está marcada; ao desmarcar, somem.

---

## 6. Real Controle — Emissão de NF-e / NFC-e

> Onde fica: **Real Controle → Vendas → (venda) → Emitir NF-e/NFC-e**.

### 6.1 — Frete, seguro e outras despesas na venda

**Objetivo:** confirmar os novos campos de despesa na venda.

**Passo a passo:**

1. Abra/edite uma **venda** e informe **Frete**, **Seguro** e **Outras
   despesas**.
2. Escolha a **Operação Fiscal** da venda.
3. (Se for devolução) informe a **chave da NF-e referenciada**.

**Resultado esperado:** os campos salvam. Ao emitir, esses valores compõem os
totais da nota. Numa **devolução**, sem a **chave referenciada** a emissão é
**bloqueada**.

### 6.2 — Emissão usando as Regras Fiscais

**Objetivo:** confirmar que a tributação da nota vem do motor de regras.

**Passo a passo:**

1. Numa venda pronta, clique em **Emitir NF-e**.
2. Observe a tela de conferência/prévia antes de confirmar.

**Resultado esperado:** os tributos (CST/CFOP/alíquotas) vêm da **Regra Fiscal**
que casa com a combinação cliente×operação×produto. Se **faltar regra** ou os
dados estiverem incoerentes (ex.: Indicador de IE x Inscrição Estadual
incompatíveis), a emissão é **bloqueada com aviso** antes de enviar.

### 6.3 — Lista de pendências fiscais ("Corrigir →")

**Objetivo:** conferir a validação assistida.

**Passo a passo:**

1. Tente emitir uma nota com algum dado faltando (ex.: parceiro sem endereço).

**Resultado esperado:** aparece uma **lista de pendências** (bloqueios e avisos),
cada uma com um link **"Corrigir →"** que leva ao campo/tela do problema.
Bloqueios impedem a emissão; avisos, não.

### 6.4 — Logo na DANFE

**Objetivo:** confirmar o upload do logo da empresa.

**Passo a passo:**

1. Na configuração de NF, abra o modal de **logo** e **envie uma imagem**.
2. Emita/visualize uma DANFE.
3. Remova o logo.

**Resultado esperado:** é possível **enviar** e **remover** o logo; ele aparece
na DANFE quando presente.

### 6.5 — Erros da SEFAZ/PlugNotas legíveis

**Objetivo:** confirmar que erros de emissão vêm em texto legível.

**Passo a passo:**

1. Force um erro de emissão (ex.: dados fiscais inválidos em homologação).

**Resultado esperado:** a mensagem de erro aparece em **linhas de texto legível**
(não mais um "amontoado" de JSON técnico).

---

## 7. Real Controle — Nota Avulsa (novo)

> Onde fica: **Real Controle → Fiscal → Nota Avulsa**.

### 7.1 — Emitir uma nota avulsa

**Objetivo:** confirmar a emissão manual, sem vínculo com uma venda.

**Passo a passo:**

1. Abra **Nota Avulsa** e preencha destinatário, itens, tributos e pagamentos.
2. Clique em **Preparar** e depois **Confirmar** a emissão.
3. Acompanhe o **status** e, se precisar, **Cancelar**.

**Resultado esperado:** o fluxo **Preparar → Confirmar → Status → Cancelar**
funciona igual ao da venda, mas **sem venda vinculada**. Pendências fiscais são
mostradas antes de confirmar.

---

## 8. Real Controle — Carta de Correção (CC-e) (novo)

> Onde fica: **Real Controle → Fiscal → CC-e** (ou a partir de uma NF-e
> concluída).

### 8.1 — Solicitar uma CC-e

**Objetivo:** confirmar a Carta de Correção Eletrônica.

**Passo a passo:**

1. Selecione uma **NF-e já concluída/autorizada**.
2. Solicite uma **CC-e**, escrevendo a **correção** (mínimo ~15 caracteres).
3. Acompanhe o **status** e veja a **lista** de CC-e da nota.

**Resultado esperado:** com texto **muito curto** o sistema **recusa**; com texto
válido, a CC-e é registrada e aparece na lista com seu status.

---

## 9. Real Controle — Inutilização de numeração (novo)

> Onde fica: **Real Controle → Fiscal → Inutilização**.

### 9.1 — Inutilizar uma faixa de numeração

**Objetivo:** confirmar a inutilização manual de faixa (modelo 55/65).

**Passo a passo:**

1. Abra **Inutilização** e informe **CNPJ**, **numeração inicial e final** e a
   **justificativa**.
2. Tente com **final menor que a inicial**.
3. Corrija e solicite.

**Resultado esperado:** com **final < inicial** o sistema **recusa**; com faixa
válida, a inutilização é solicitada e aparece com status na **lista**.

---

## 10. Real Controle — Documentos Fiscais (novo)

> Onde fica: **Real Controle → Fiscal → Documentos Fiscais**.

### 10.1 — Relatório unificado + download em lote

**Objetivo:** confirmar a listagem única de notas (de venda + avulsas) e o
download em lote.

**Passo a passo:**

1. Abra **Documentos Fiscais** e veja a lista com NF-e de venda **e** notas
   avulsas juntas.
2. Selecione **várias** notas e use **Baixar em lote** (XML/PDF).

**Resultado esperado:** o relatório mostra os dois tipos juntos. O download em
lote separa as **baixáveis** das que têm **impedimento** e gera os arquivos com
nomes coerentes (sem colisão de nomes).

---

## 11. Real Controle — Notas Destinadas / DF-e (novo)

> Onde fica: **Real Controle → Fiscal → Notas Destinadas**.

### 11.1 — Ativar o DF-e e sincronizar

**Objetivo:** confirmar a busca das notas emitidas **contra** o seu CNPJ.

**Passo a passo:**

1. Abra **Notas Destinadas** e **ative o DF-e** (se ainda não ativo).
2. Informe o **período** e clique em **Sincronizar**.
3. Abra o **detalhe** de uma nota destinada (modal).

**Resultado esperado:** a sincronização traz as **notas destinadas** do período.
O detalhe mostra emitente e **itens** (quando importados), com indicação de
quais têm **crédito de ICMS elegível**. Baixar **XML/PDF** da destinada funciona.

---

## 12. Real Controle — SPED Fiscal (novo)

> Onde fica: **Real Controle → Fiscal → SPED**.

### 12.1 — Perfil fiscal da empresa

**Objetivo:** confirmar o cadastro do perfil usado no SPED.

**Passo a passo:**

1. Abra a página **SPED** e preencha o **perfil fiscal** (razão social,
   inscrição estadual, UF, CNAE, regime tributário, contador, e os campos de
   **código de receita** e **dia de vencimento** do ICMS).
2. Salve.

**Resultado esperado:** o perfil salva por **CNPJ**. Sem **código de receita /
vencimento**, a prévia (abaixo) acusa pendência.

### 12.2 — Prévia e geração do arquivo

**Objetivo:** confirmar a prévia de apuração e o download do `.txt`.

**Passo a passo:**

1. Selecione **CNPJ** e **competência** (mês/ano).
2. Clique em **Prévia** e veja o **resumo de apuração** e as **pendências**.
3. Clique em **Gerar** e baixe o arquivo `.txt`.
4. Veja o **histórico de gerações**.

**Resultado esperado:** a prévia mostra **débito − crédito de ICMS** e bloqueia se
houver **entradas manifestadas sem itens importados** ou perfil incompleto. A
geração baixa um **arquivo `.txt`** e registra uma linha no **histórico**.

---

## 13. Real Controle — Expedição parcial + conferência

> Onde fica: **Real Controle → Expedição** (Cargas / Romaneio).

### 13.1 — Expedir só parte de uma venda (status "Parcial")

**Objetivo:** confirmar a baixa parcial por item.

**Passo a passo:**

1. Gere uma carga a partir de vendas pendentes; note que a quantidade sugerida é
   o **saldo a expedir** ("X de Y"), não a quantidade total vendida.
2. Numa carga, **reduza** a quantidade de um item para **menos** que o saldo e
   despache.
3. Volte na venda e confira o **status de expedição**.

**Resultado esperado:** a venda fica com status **"Parcial"** (badge). O saldo
restante continua disponível para uma **próxima carga**. Tentar despachar **mais
que o saldo** é bloqueado (destaque **vermelho** no campo).

### 13.2 — Conferência de carregamento (gate)

**Objetivo:** confirmar a trava que exige conferir os itens antes de despachar.

**Passo a passo:**

1. No status que dispara estoque, ative a opção **"Exigir conferência"**.
2. Numa carga desse status, use o bloco de **conferência**: marque itens (ou
   **"Marcar todos"**) e observe o contador **X/Y**.
3. Tente **despachar** com itens ainda **não conferidos**.

**Resultado esperado:** com **"Exigir conferência"** ligado, o despacho é
**bloqueado** enquanto **nem todos** os itens estiverem conferidos. Com todos
marcados, despacha normalmente. Após o despacho, a conferência **congela**.

### 13.3 — Reordenar status por arrastar (drag-and-drop)

**Objetivo:** confirmar a nova reordenação de status.

**Passo a passo:**

1. Abra a tela de **status** da Expedição.
2. **Arraste** um status para outra posição (pela "alça" de arrastar).

**Resultado esperado:** as antigas **setas para cima/baixo** foram substituídas
por **arrastar e soltar**; a nova ordem é salva.

### 13.4 — Estorno da carga

**Objetivo:** confirmar que estornar devolve o saldo.

**Passo a passo:**

1. **Estorne** uma carga já despachada.
2. Volte na venda.

**Resultado esperado:** o estoque é **devolvido** e o **saldo a expedir** da
venda **volta a subir** (a venda pode voltar de "Parcial"/"Expedido" para o
estágio anterior). A nota de estorno cita **"Carga #número"**.

---

## 14. Real Controle — Financeiro (DRE, Fluxo, Plano de contas)

> Onde fica: **Real Controle → Financeiro / Plano de Contas / DRE / Fluxo de
> Caixa**.

### 14.1 — Categorias de sistema obrigatórias no Plano de Contas

**Objetivo:** confirmar o aviso de categorias de sistema não mapeadas.

**Passo a passo:**

1. Abra o **Plano de Contas** e observe o painel de **categorias de sistema**
   (comissão, juros, tarifas, antecipação, receita de vendas etc.).
2. Tente **excluir** ou **deixar sem vínculo** uma categoria de sistema
   necessária.

**Resultado esperado:** o sistema **avisa** quais categorias de sistema estão
**sem mapeamento** e **impede** deixar/remover uma que geraria lançamentos
automáticos **órfãos** (que sumiriam da DRE).

### 14.2 — DRE não infla mais com remessas

**Objetivo:** confirmar que operações "que não afetam o financeiro" saíram da
receita.

**Passo a passo:**

1. Registre/tenha vendas com operação **Remessa/Demonstração/Conserto**
   ("não afeta o financeiro").
2. Abra a **DRE** no período.

**Resultado esperado:** essas vendas **não inflam** a receita bruta na DRE. Os
mesmos filtros valem nos **relatórios de vendas**.

### 14.3 — Título parcialmente pago mantém o valor de competência

**Objetivo:** confirmar o tratamento de baixa parcial na DRE.

**Passo a passo:**

1. Faça uma **baixa parcial** de um título e depois consulte a **DRE**.

**Resultado esperado:** a receita reconhece o **valor original de competência**
(não o valor "reduzido" pela baixa parcial), sem subavaliar o título.

### 14.4 — Previsão de fluxo inclui vencidos

**Objetivo:** confirmar a previsão do fluxo de caixa.

**Passo a passo:**

1. Tenha títulos **vencidos antes** do período e abra a **previsão** do fluxo.

**Resultado esperado:** os **vencidos** anteriores ao período entram no **primeiro
período** da previsão (não somem mais).

### 14.5 — Comissão e Pedido de Compra

**Objetivo:** conferir ajustes de comissão e a obrigatoriedade de plano de contas.

**Passo a passo:**

1. Ao criar um **Pedido de Compra**, tente salvar **sem** informar o **nó do
   plano de contas**.
2. Gere/baixe uma **comissão** e observe o título gerado.

**Resultado esperado:** o Pedido de Compra **exige** o plano de contas. A comissão
gera **um título por comissão** (1:1); ao **baixar** a comissão, ela é marcada
como **paga**.

---

## 15. Real Controle — Boleto: baixa automática

> Onde fica: **Real Controle → Financeiro → Boletos** (e o título a receber
> vinculado).

### 15.1 — Liquidação sincroniza e baixa o título

**Objetivo:** confirmar que a liquidação do boleto baixa o recebível
automaticamente.

**Passo a passo:**

1. Tenha um **boleto** ligado a um **recebível em aberto**.
2. Provoque/aguarde a **liquidação** do boleto (peça ao time para simular pela
   PlugBoleto homologação, ou use o botão manual de **atualizar status** como
   alternativa).

**Resultado esperado:** quando o boleto passa a **LIQUIDADO**, o recebível
correspondente é **baixado automaticamente**. Se houver **ambiguidade** (nenhum
ou vários títulos casam), o boleto exibe uma mensagem de **"baixa manual
necessária"** para o financeiro revisar — sem baixar errado.

---

## 16. Real Análise

> Onde fica: **Módulos → Real Análise**.

### 16.1 — Saúde das conexões

**Objetivo:** confirmar o novo painel de saúde.

**Passo a passo:**

1. Na home do Real Análise, veja o painel **"Saúde das conexões"**.

**Resultado esperado:** para **cada conexão** aparece status/erro, sincronização
por entidade, volume e **pendências de categorização** (sem seção de Fluxo, sem
linha de DRE ou sem categoria). Substitui o card antigo que era fixo no Bling.

### 16.2 — Multi-tratamento por conexão

**Objetivo:** confirmar o seletor de conexão no Tratamento.

**Passo a passo:**

1. Abra **Tratamento** e use o **seletor de conexão**.
2. Ajuste um **De-Para** de categoria/campo em **uma** conexão.
3. Troque para outra conexão.

**Resultado esperado:** o Tratamento é **por conexão** — o mapeamento de uma
conexão **não vaza** para a outra.

### 16.3 — Seções e linhas personalizadas

**Objetivo:** confirmar os grupos personalizados de Fluxo/DRE.

**Passo a passo:**

1. Em **Tratamento → Categorias**, clique em **"Personalizar seções"**.
2. Crie uma **seção de Fluxo de Caixa** e/ou **linha de DRE** personalizada.
3. Aponte algumas categorias para o grupo criado.
4. Confira o **Fluxo de Caixa** e a **DRE**; depois **apague** o grupo.

**Resultado esperado:** as categorias passam a aparecer sob a **seção/linha
personalizada** (com nome próprio), **sem alterar os totais**. Ao **apagar** o
grupo, as categorias voltam para as **seções fixas**.

### 16.4 — Realizado híbrido (Tiny deixa de zerar)

**Objetivo:** confirmar o realizado onde não há livro-caixa.

**Passo a passo:**

1. Numa conexão **Tiny** (sem livro-caixa), abra o **Fluxo de Caixa** realizado.

**Resultado esperado:** o realizado agora usa os **lançamentos liquidados**
quando não há livro-caixa — o Tiny **não fica mais zerado**. O **drill-down** por
nó também reflete isso.

---

## 17. Real Plano

> Onde fica: **Módulos → Real Plano**.

### 17.1 — Navegação lateral do plano

**Objetivo:** confirmar a navegação por abas no sidebar (com layout lateral).

**Passo a passo:**

1. Com **navegação lateral** ligada (teste 1.1), abra um **plano**.

**Resultado esperado:** as abas/categorias do plano aparecem **no sidebar
esquerdo**. Com navegação **superior**, seguem na **barra de cima**.

### 17.2 — IRPJ/CSLL trimestral (Lucro Real)

**Objetivo:** conferir a apuração trimestral na DRE do plano.

**Passo a passo:**

1. Num plano com linhas em **Lucro Real**, abra a aba **DRE**.
2. Observe meses de **prejuízo** seguidos de **lucro** dentro do mesmo trimestre
   (jan-mar, abr-jun...).

**Resultado esperado:** a apuração é **por trimestre civil** com compensação
dentro do trimestre (mês de prejuízo pode **reverter** provisão). O adicional de
10% incide sobre o excedente proporcional (R$ 20 mil × mês do trimestre, R$ 60
mil no fechamento).

### 17.3 — Ponto de equilíbrio (Presumido vs Real)

**Objetivo:** confirmar o imposto sobre resultado no PE.

**Passo a passo:**

1. Abra a aba **Ponto de Equilíbrio** num plano **Lucro Presumido** e depois num
   **Lucro Real**.

**Resultado esperado:** no **Presumido**, o imposto variável (CSLL/IRPJ sobre
receita) **reduz a margem de contribuição** e **eleva** o PE. No **Real/Simples**
o PE **não** muda por esse efeito. As fórmulas de PE não citam mais CSLL/IRPJ
diretamente.

### 17.4 — Empréstimos e financeiro manual no fluxo

**Objetivo:** confirmar captação e lançamentos manuais no DFC.

**Passo a passo:**

1. Cadastre um **empréstimo contratado dentro do horizonte** do plano.
2. Adicione **receitas/despesas financeiras manuais**.
3. Abra o **fluxo de caixa** do plano.

**Resultado esperado:** o DFC mostra novas linhas: **Captação de Empréstimos** (no
mês da contratação) e **Receitas/Despesas Financeiras**. A dívida do empréstimo
**não** aparece antes do mês de contratação.

### 17.5 — Exportação para Excel

**Objetivo:** confirmar números "de verdade" na planilha.

**Passo a passo:**

1. Exporte um relatório do plano para **Excel**.
2. Abra e some algumas células.

**Resultado esperado:** os valores saem como **números** (dá para somar/formatar
no Excel), não como texto. O **nome do arquivo** usa a **data local**.

### 17.6 — Não perde o que foi digitado ao trocar de aba

**Objetivo:** confirmar o auto-save ao sair da aba.

**Passo a passo:**

1. Na aba **Receitas** (ou Premissas), digite valores e **troque de aba** logo em
   seguida.

**Resultado esperado:** o que você digitou **não se perde** ao trocar de aba.

---

## 18. Admin — Painel de Engajamento

> Onde fica: **Admin → Engajamento/Evolução** (somente administrador).

### 18.1 — As 4 sub-abas

**Objetivo:** confirmar o painel reescrito.

**Passo a passo:**

1. Abra o painel e navegue entre **Visão geral**, **Organizações**, **Usuários**
   e **Produto**.

**Resultado esperado:** cada aba carrega. **Visão geral** traz KPIs com variação
(delta), gráfico de pulso e os feeds **"Precisa de atenção"** e **"Ganhos da
semana"**. **Organizações** lista orgs com **health score**; **Produto** mostra
**adoção por feature**; **Usuários** inclui cohorts de **retenção** (heatmap).

### 18.2 — Filtros e contato

**Objetivo:** confirmar toggles e ações de contato.

**Passo a passo:**

1. Ligue/desligue os toggles **"Incluir internos"** e **"Incluir admin"**.
2. Clique em **"Contatar"** numa organização.

**Resultado esperado:** por padrão e-mails **@realcusto.com/@test.com**, banidos e
o módulo **Admin** ficam **de fora**; os toggles os trazem de volta. **"Contatar"**
abre o cliente de e-mail (mailto) com o destinatário certo.

---

## 19. App Mobile — Expedição parcial

> Onde fica: **App Mobile → Meus Pedidos** (usuário vendedor).

### 19.1 — Estágio "Expedido parcial"

**Objetivo:** confirmar o novo estágio no app.

**Passo a passo:**

1. Tenha um pedido **parcialmente expedido** (teste 13.1).
2. Abra **Meus Pedidos** no celular.

**Resultado esperado:** o pedido mostra o estágio **"Expedido parcial"** (com
ícone próprio), coerente com o status **"Parcial"** da web.

---

## 20. Diversos (aparência e pequenos ajustes)

### 20.1 — Notificações (toasts)

**Objetivo:** confirmar a posição das notificações.

**Passo a passo:**

1. Faça uma ação que dê **sucesso** e outra que dê **erro**.

**Resultado esperado:** o toast de **sucesso** aparece em **cima à direita**; o de
**erro**, **embaixo à direita**.

### 20.2 — Tema e selo do reCAPTCHA

**Objetivo:** conferir ajustes visuais e o selo do reCAPTCHA.

**Passo a passo:**

1. Percorra algumas telas no tema claro e escuro.
2. Abra as telas de **login / cadastro / esqueci a senha**.

**Resultado esperado:** o visual segue consistente (cartões e fundos). O **selo
flutuante** do reCAPTCHA some, mas o **texto de atribuição** ("protegido por
reCAPTCHA...") aparece nas telas de autenticação.

### 20.3 — Números com vírgula/ponto

**Objetivo:** confirmar a leitura correta de números digitados.

**Passo a passo:**

1. Em campos numéricos (ex.: quantidades, valores, importações), digite
   **`1,5`** e também **`1.5`**.

**Resultado esperado:** `1,5` é lido como **um e meio** (não vira `15`). O sistema
lida bem com os dois separadores.
