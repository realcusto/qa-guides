# Guia de Teste para QA — Revisão do PR #519

Este guia cobre **apenas as correções feitas durante a revisão do PR #519**. As
funcionalidades que já existiam antes (vindas da `dev-plano`) **já foram testadas**
e não precisam ser testadas de novo aqui.

Tudo aqui é testável **somente pela tela**, sem conhecimento técnico. Cada teste
tem **Objetivo**, **Passo a passo** e **Resultado esperado**.

## Como usar este guia

- Faça login no ambiente de testes indicado pelo time.
- No topo da tela existe o botão **"Módulos"**, por onde se troca entre
  **Real Preço**, **Real Plano**, **Real Análise**, **Real Controle** e
  **Consultoria**.
- Marque cada cenário como ✅ (passou), ❌ (falhou) ou ⚠️ (passou com ressalva) e
  anote o que viu de diferente (de preferência com print).
- Se algum menu não aparecer, avise o time — pode ser trava de permissão.

### Pré-requisitos gerais

- Um usuário **administrador** (para a área de **Novidades** e para configurar
  status). Algumas telas só aparecem para administradores.
- Um usuário **vendedor** e um **celular** com o aplicativo, para a parte do
  **App Mobile**.
- Credenciais de teste da **Omie** (peça ao time) para a parte de integração.
- Confirme com o time que o ambiente está com **esta versão já publicada**.

---

## 1. Novidades (avisos do sistema)

> Onde fica: ícone/área de **Novidades** do sistema. Criar e editar novidades é
> uma ação de **administrador**.

### 1.1 — O link da novidade só aceita endereços de internet válidos

**Objetivo:** garantir que só seja possível salvar links que comecem com
`http://` ou `https://` (proteção contra links perigosos).

**Passo a passo:**

1. Como administrador, crie ou edite uma **novidade**.
2. No campo de **link**, digite um endereço **sem** `http`, por exemplo:
   `www.google.com` ou `pagina-de-teste`.
3. Tente **salvar**.
4. Depois, troque o link por um endereço completo, ex.: `https://www.google.com`.
5. Salve novamente.

**Resultado esperado:**

- No passo 3, o sistema **recusa** e mostra uma mensagem pedindo um link que
  comece com `http://` ou `https://`.
- No passo 5, com o endereço completo, a novidade **salva normalmente**.
- Deixar o link **em branco** também é permitido (não é obrigatório).

### 1.2 — As imagens das novidades continuam aparecendo

**Objetivo:** confirmar que as imagens das novidades continuam sendo exibidas
normalmente.

**Passo a passo:**

1. Abra a lista/feed de **Novidades** que tenham imagens.

**Resultado esperado:** as imagens aparecem normalmente, como antes.

### 1.3 — Enviar imagem na novidade (e recusar arquivo que não é imagem)

**Objetivo:** confirmar que o envio de imagens funciona e que arquivos que **não
são imagem** são recusados.

**Passo a passo:**

1. Como administrador, ao criar/editar uma novidade, **envie uma imagem normal**
   (PNG ou JPG). Confirme que ela aparece na novidade.
2. (Opcional) Tente enviar um arquivo que **não seja imagem** — por exemplo um
   **PDF** ou um documento de texto.

**Resultado esperado:**

- A imagem normal é enviada e aparece.
- O arquivo que não é imagem é **recusado** com uma mensagem avisando que o
  formato/conteúdo não é uma imagem válida.

### 1.4 — Imagem muito grande é recusada (limite de 5 MB)

**Objetivo:** confirmar que não dá para enviar uma imagem acima de **5 MB**.

**Passo a passo:**

1. Como administrador, ao criar/editar uma novidade, tente enviar uma **imagem
   grande, acima de 5 MB** (uma foto em alta resolução, por exemplo).
2. Em seguida, envie uma **imagem normal, abaixo de 5 MB**.

**Resultado esperado:**

- A imagem **acima de 5 MB** é **recusada**, com aviso de que excede o tamanho
  máximo de 5 MB. O sistema não trava nem demora muito para responder.
- A imagem **abaixo de 5 MB** é enviada normalmente.

---

## 2. Real Preço — Orçamentos

> Módulo: **Módulos → Real Preço → Orçamentos**.

### 2.1 — A prévia do DRE mostra sempre o valor mais recente

**Objetivo:** ao alterar valores rapidamente, a prévia de resultado (DRE) não pode
"travar" num valor antigo.

**Passo a passo:**

1. Abra um **orçamento** existente (ou crie um novo com alguns itens).
2. No **preço de venda** (ou no desconto), **mude o valor várias vezes seguidas,
   rapidamente** (ex.: digite 1000, apague, digite 2000, apague, digite 3000).
3. Pare e aguarde 1–2 segundos.

**Resultado esperado:** a prévia do **DRE/resultado** reflete o **último** valor
digitado (no exemplo, 3000). Ela **não** fica mostrando um valor intermediário
antigo.

### 2.2 — Configuração de status do orçamento (salvar, reordenar, sem duplicar)

**Objetivo:** garantir que dá para editar/reordenar os status e que eles não
aparecem duplicados.

**Passo a passo:**

1. Abra a tela de **configuração de status** dos orçamentos.
2. Na **primeira vez** que abrir (numa empresa nova), observe a lista de status
   padrão.
3. **Renomeie** um status e troque a **cor**; clique em **salvar**.
4. **Feche e reabra** a tela de configuração.
5. **Arraste** um status para outra posição para reordenar.
6. Feche e reabra novamente.

**Resultado esperado:**

- Os status padrão aparecem **uma única vez** (sem repetições/duplicatas).
- O novo **nome e cor** continuam salvos após reabrir.
- A **nova ordem** (arrastar) continua salva após reabrir.

### 2.3 — A opção "Espremer em 1 página (PDF)" foi removida

**Objetivo:** confirmar que essa opção (que não fazia nada) sumiu da configuração
da proposta.

**Passo a passo:**

1. Dentro de um orçamento, abra **Configurar** (configuração da proposta em PDF).
2. Procure por uma opção chamada **"Espremer em 1 página (PDF)"**.

**Resultado esperado:** essa opção **não existe mais**. As demais opções de
configuração continuam funcionando normalmente.

### 2.4 — A proposta continua igual visualmente

**Objetivo:** confirmar que a aparência da proposta não mudou.

**Passo a passo:**

1. Gere/visualize a **proposta** de um orçamento.

**Resultado esperado:** o layout da proposta está **igual ao de antes** (cores,
espaçamentos, totais, cabeçalho de marca etc.).

---

## 3. App Mobile (vendedor)

> No celular, com o usuário **vendedor** logado no aplicativo.

### 3.1 — Trocar a lista de preços limpa o carrinho (com aviso)

**Objetivo:** ao trocar a lista de preços com itens no carrinho, o app deve
**avisar** e limpar o carrinho (para não misturar preços de listas diferentes).

**Passo a passo:**

1. Inicie um **novo pedido**.
2. Adicione **alguns produtos** ao carrinho.
3. **Troque a lista de preços** (categoria de preço) para outra.

**Resultado esperado:**

- Aparece uma **confirmação** avisando que o carrinho será esvaziado.
- Ao **confirmar**, o carrinho fica **vazio** e a nova lista passa a valer.
- Ao **cancelar**, o carrinho **continua igual** e a lista não muda.

### 3.2 — Sincronizar pedidos não duplica (toque duplo)

**Objetivo:** apertar "sincronizar" mais de uma vez não pode enviar o mesmo pedido
duas vezes.

**Passo a passo:**

1. Tenha **um ou mais pedidos pendentes** para enviar.
2. Na tela de **sincronizar**, toque no botão **"Sincronizar"** e, logo em
   seguida, **toque de novo** rapidamente.

**Resultado esperado:** cada pedido é enviado **uma única vez**. Não aparecem
pedidos/vendas duplicados (confira em **Meus pedidos** e/ou nas vendas do
**Real Controle**).

### 3.3 — Enviar pedido sem internet e reenviar não duplica

**Objetivo:** se a internet cair no meio do envio, reenviar não pode criar o pedido
duas vezes.

**Passo a passo:**

1. Crie um **pedido** no app.
2. Ative o **modo avião** (sem internet).
3. Tente **sincronizar** (vai falhar/ficar pendente).
4. **Desative** o modo avião.
5. **Sincronize** novamente.

**Resultado esperado:** o pedido aparece **uma única vez** no sistema, mesmo tendo
tentado enviar mais de uma vez. Nada é duplicado.

### 3.4 — As telas do app continuam iguais visualmente

**Objetivo:** confirmar que o visual das telas do app não mudou.

**Passo a passo:**

1. Navegue pelas telas: **início, catálogo, clientes, detalhe do cliente, novo
   pedido, meus pedidos, detalhe do pedido e comissões**.

**Resultado esperado:** todas as telas estão **iguais ao de antes** (textos,
botões, espaçamentos e cores no lugar).

---

## 4. Real Controle — Status de Expedição e de Produção

> Módulo: **Módulos → Real Controle**, nas áreas de **Expedição** e **Produção**.

### 4.1 — Configuração de status de Expedição (salvar, reordenar, sem duplicar)

**Objetivo:** mesmo teste do orçamento (item 2.2), agora na **Expedição**.

**Passo a passo:**

1. Abra a **configuração de status** de **Expedição**.
2. Na primeira vez, confira os status padrão (devem aparecer **sem duplicatas**).
3. **Renomeie** um status, troque a cor e **salve**.
4. **Feche e reabra**; confirme que salvou.
5. **Arraste** para reordenar, feche e reabra.

**Resultado esperado:** sem duplicatas; nome/cor e a nova ordem continuam salvos
após reabrir.

### 4.2 — Configuração de status de Produção

**Objetivo:** mesmo teste, agora na **Produção**.

**Passo a passo:** repita os passos do item 4.1 na configuração de status de
**Produção**.

**Resultado esperado:** igual ao 4.1 (sem duplicatas; alterações e ordem salvas).

---

## 5. Real Análise — Integração com a Omie

> Módulo: **Módulos → Real Análise → Integrações**. Precisa de credenciais de
> teste da Omie (peça ao time).

### 5.1 — Credenciais erradas mostram erro rápido (não trava)

**Objetivo:** ao informar uma chave da Omie **errada**, o sistema deve avisar
**rapidamente**, sem ficar muito tempo "tentando".

**Passo a passo:**

1. Em **Integrações**, configure a **Omie** com um **App Key/App Secret inválidos**
   (ex.: digite qualquer coisa errada).
2. Clique em **Testar** (ou **Conectar/Sincronizar**).

**Resultado esperado:** aparece uma mensagem de **erro de credencial** em poucos
segundos. O sistema **não** fica travado tentando por muito tempo.

> Observação: com credenciais **corretas**, a conexão e a sincronização devem
> continuar funcionando normalmente, como antes.

### 5.2 — Após a publicação, sincronizar para repovoar o Real Análise

**Objetivo:** confirmar que, depois desta versão ir ao ar, os dados financeiros do
Real Análise (**Fluxo de Caixa** e **DRE**) voltam a aparecer após uma
sincronização feita **pela tela**.

> Importante: esta versão faz uma limpeza interna dos lançamentos financeiros do
> Real Análise. Por isso eles ficam **temporariamente vazios** até a **primeira
> sincronização** — isso é **esperado** e se resolve com o passo a passo abaixo.
> Essa ação **precisa ser feita** pelo menos uma vez após a publicação.

**Passo a passo:**

1. Vá em **Módulos → Real Análise → Integrações**.
2. Antes de sincronizar, abra **Fluxo de Caixa** e **DRE** e observe que podem
   estar **sem dados** (vazios). Isso é normal neste momento.
3. Volte em **Integrações** e clique em **"Sincronizar"** na conexão
   (Omie/Bling/etc.).
4. Aguarde a barra **"Sincronizando…"** terminar — pode levar **alguns minutos**,
   conforme o tamanho do histórico.
5. Abra novamente **Fluxo de Caixa** e **DRE**.

**Resultado esperado:**

- Depois que a sincronização termina, **Fluxo de Caixa** e **DRE** voltam a
  mostrar os lançamentos normalmente.
- Os valores **não aparecem duplicados** (cada lançamento uma única vez).

---

## 6. O que NÃO precisa de teste manual (apenas para transparência)

Estas correções são internas e **não mudam o que o usuário vê** — não há o que
testar pela tela:

- Ajustes internos de **segurança de dados entre empresas** (uma empresa não
  enxerga dados de outra) e de **cadastro/período de teste** de novas contas.
- Ajustes de **código e desempenho** (organização interna, contadores, etc.).
- **Precisão dos números** (valores monetários) foi **mantida como estava** — não
  deve haver nenhuma diferença em contas/totais.
- Pequenas correções de **formatação de código** (sem efeito visual).

Se, ao testar, você notar **qualquer** diferença em valores, totais ou em dados
aparecendo trocados entre empresas, **avise o time imediatamente**.

---

## Resumo para marcação

| Item | Cenário | Status |
|------|---------|--------|
| 1.1 | Link só aceita http/https | |
| 1.2 | Imagens das novidades aparecem | |
| 1.3 | Envio de imagem / recusa de não-imagem | |
| 1.4 | Imagem acima de 5 MB é recusada | |
| 2.1 | Prévia do DRE mostra valor mais recente | |
| 2.2 | Status do orçamento (salvar/reordenar/sem duplicar) | |
| 2.3 | Opção "Espremer em 1 página" removida | |
| 2.4 | Proposta igual visualmente | |
| 3.1 | Trocar lista de preços limpa o carrinho (com aviso) | |
| 3.2 | Sincronizar não duplica (toque duplo) | |
| 3.3 | Enviar sem internet e reenviar não duplica | |
| 3.4 | Telas do app iguais visualmente | |
| 4.1 | Status de Expedição (salvar/reordenar/sem duplicar) | |
| 4.2 | Status de Produção (salvar/reordenar/sem duplicar) | |
| 5.1 | Omie: credencial errada avisa rápido | |
| 5.2 | Real Análise: sincronizar repovoa Fluxo de Caixa/DRE | |
