# Guia de Teste para QA — Release 1.1.0 (PR #522)

Este guia cobre **as novidades e correções que vão para produção na versão
1.1.0**. Tudo aqui é testável **somente pela tela**, sem conhecimento técnico.
Cada teste tem **Objetivo**, **Passo a passo** e **Resultado esperado**.

> As correções pontuais já listadas no guia da revisão do PR #519 continuam
> valendo. Este guia é o conjunto completo da versão que será publicada.

## Como usar este guia

- Faça login no ambiente de testes indicado pelo time.
- No topo da tela existe o botão **"Módulos"**, por onde se troca entre
  **Real Preço**, **Real Plano**, **Real Análise**, **Real Controle** e
  **Consultoria**.
- Marque cada cenário como ✅ (passou), ❌ (falhou) ou ⚠️ (passou com ressalva)
  e anote o que viu de diferente (de preferência com print).
- Se algum menu não aparecer, avise o time — pode ser trava de permissão.

### Pré-requisitos gerais

- Um usuário **administrador** (para criar **Novidades** e configurar status).
  Algumas telas só aparecem para administradores.
- Um usuário **vendedor** vinculado e um **celular** com o aplicativo, para a
  parte do **App Mobile**.
- Credenciais de teste da **Omie** (peça ao time) para a parte de integração.
- Se possível, peça ao time uma conta de teste com **período de avaliação
  prestes a expirar / já expirado** (para a seção de "Acesso").
- Confirme com o time que o ambiente está com **esta versão já publicada** e
  que as integrações do Real Análise **já foram sincronizadas** (veja item 9.6).

---

## 1. Novidades (changelog do sistema)

> Onde fica: ícone de **lâmpada** no topo / área de **Novidades**. Criar e
> editar novidades é ação de **administrador**.

### 1.1 — A tela de Novidades virou um "mural" de avisos

**Objetivo:** confirmar o novo formato em lista de cards (changelog).

**Passo a passo:**

1. Abra a área de **Novidades**.
2. Observe a lista de cards, do mais recente para o mais antigo.

**Resultado esperado:** aparece uma lista de cards com data, etiqueta do módulo,
título e resumo. **Não existe mais** a aba de "Comunidade de Ideias" nem botão
de "Sugerir ideia".

### 1.2 — Contador de novidades não lidas e "marcar como lido"

**Objetivo:** o número de avisos não lidos deve aparecer e poder ser zerado.

**Passo a passo:**

1. Observe a **bolinha vermelha com número** no ícone de lâmpada (avisos novos
   desde sua última visita).
2. Entre em **Novidades** e clique em **"Marcar como lido"**.
3. **Recarregue** a página (F5).

**Resultado esperado:** ao marcar como lido, a bolinha **zera**. Depois de
recarregar, ela **continua zerada** (não volta a contar os mesmos avisos).

### 1.3 — Abrir um aviso e ver imagens, vídeo e link

**Objetivo:** confirmar o detalhe do aviso (modal) com mídia.

**Passo a passo:**

1. Clique em um card que tenha imagem/vídeo/link.
2. No detalhe, clique em uma **imagem** para ampliar (galeria).
3. Com a imagem ampliada: use as **setas** para trocar de foto e feche clicando
   no **X** ou no **fundo escuro** (ou tecla Esc).
4. Se houver **vídeo**, dê play; se houver **link**, clique nele.

**Resultado esperado:** o detalhe abre sem sair da página. As imagens ampliam e
trocam pelas setas; X/fundo/Esc fecham. O vídeo (YouTube/Vimeo) toca dentro da
tela. O link abre em **nova aba**.

### 1.4 — Criar / editar / apagar uma novidade (administrador)

**Objetivo:** confirmar a publicação de avisos pelo administrador.

**Passo a passo:**

1. Como **administrador**, clique em **"Nova Atualização"** (na mesma linha do
   título).
2. Preencha **título**, escolha o **módulo**, escreva **descrição** e
   **conteúdo**.
3. Adicione uma ou mais **imagens**, um **link** e/ou um **vídeo**.
4. **Publique**. Depois, no card criado, use o menu (•••) para **Editar** e
   para **Apagar**.

**Resultado esperado:** a novidade é publicada e aparece no topo do mural.
Editar reabre o formulário já preenchido e salva as mudanças. Apagar remove o
card. O botão "Nova Atualização" e o menu (•••) **só aparecem para
administradores**.

### 1.5 — Envio de imagem: recusar arquivo que não é imagem e acima de 5 MB

**Objetivo:** confirmar que só imagens válidas e até 5 MB são aceitas.

**Passo a passo:**

1. Ao criar/editar uma novidade, envie uma **imagem normal** (PNG/JPG) e
   confirme que ela aparece.
2. Tente enviar um arquivo que **não é imagem** (ex.: um **PDF**) — se preciso,
   renomeie-o para `.jpg` para tentar enganar o sistema.
3. Tente enviar uma **imagem acima de 5 MB**.

**Resultado esperado:** a imagem normal é aceita. O arquivo que não é imagem é
**recusado** com aviso de formato inválido (mesmo renomeado). A imagem acima de
5 MB é **recusada** com aviso de tamanho máximo de 5 MB. O sistema não trava.

### 1.6 — O link da novidade só aceita endereços de internet válidos

**Objetivo:** garantir que só salve links começando com `http://` ou `https://`.

**Passo a passo:**

1. Ao criar/editar, no campo de **link**, digite algo **sem** `http` (ex.:
   `www.google.com`) e tente salvar.
2. Depois troque por um endereço completo (ex.: `https://www.google.com`).

**Resultado esperado:** o primeiro é **recusado** pedindo um link com
`http://`/`https://`; o completo **salva normalmente**. Link **em branco**
também é permitido.

---

## 2. Aparência geral do sistema (interface 15% menor)

### 2.1 — A interface ficou um pouco mais compacta

**Objetivo:** o sistema todo ficou ~15% menor; confirmar que **nada quebrou**.

**Passo a passo:**

1. Navegue por várias telas (orçamentos, relatórios, tabelas, cadastros).
2. Observe textos, botões, menus e tabelas.
3. Teste também no **celular/janela menor** e no **modo escuro**, se usar.

**Resultado esperado:** tudo aparece um pouco menor, porém **proporcional e
legível**. Nada fica **cortado**, sobreposto ou ilegível. Botões e menus
continuam clicáveis normalmente.

---

## 3. Real Preço — Orçamentos

> Módulo: **Módulos → Real Preço → Orçamentos**.

### 3.1 — Arrastar para reordenar os status do orçamento

**Objetivo:** dá para reordenar os status arrastando, e a ordem é salva.

**Passo a passo:**

1. Abra a **configuração de status** dos orçamentos.
2. **Arraste** um status (pela alça de "seis pontinhos") para outra posição.
3. **Feche e reabra** a tela (ou recarregue).

**Resultado esperado:** a nova ordem é mantida após reabrir. Os status padrão
aparecem **sem duplicatas**.

### 3.2 — O status do orçamento não volta para "Selecione" ao reabrir

**Objetivo:** o status escolhido deve continuar gravado.

**Passo a passo:**

1. Em um orçamento, escolha um **status** e salve.
2. **Recarregue** a página e reabra o orçamento.

**Resultado esperado:** o campo **status continua com o valor escolhido** (não
volta para "Selecione" nem fica em branco).

### 3.3 — A prévia do DRE mostra sempre o valor mais recente

**Objetivo:** ao alterar valores rapidamente, a prévia não pode "travar" num
valor antigo.

**Passo a passo:**

1. Abra um orçamento com itens.
2. No **preço de venda** (ou desconto), **mude o valor várias vezes seguidas,
   rapidamente** (ex.: 1000, 2000, 3000).
3. Pare e aguarde 1–2 segundos.

**Resultado esperado:** a prévia do **DRE** reflete o **último** valor (3000),
nunca um valor intermediário antigo.

### 3.4 — A opção "Espremer em 1 página (PDF)" foi removida

**Objetivo:** confirmar que essa opção (que não fazia nada) sumiu.

**Passo a passo:**

1. Em um orçamento, abra **Configurar** (proposta em PDF).
2. Procure por **"Espremer em 1 página (PDF)"**.

**Resultado esperado:** a opção **não existe mais**; as demais continuam
funcionando. O **layout da proposta** continua igual ao de antes.

---

## 4. App Mobile (vendedor)

> No celular, com o usuário **vendedor** logado no aplicativo.

### 4.1 — Tela de Início (resumo) e Catálogo

**Objetivo:** conferir a nova tela inicial com indicadores e o catálogo.

**Passo a passo:**

1. Faça login e **sincronize** os dados.
2. Abra **Início** e veja a saudação com o nome e os cards de resumo (vendas do
   mês, comissão a receber, pedidos em aberto).
3. Toque no card **"Comissão a receber"**.
4. Abra o **Catálogo**, busque por um produto e troque a **lista de preço** (se
   houver mais de uma).

**Resultado esperado:** Início mostra o nome e os números do vendedor; tocar em
"Comissão a receber" leva à tela de **Comissões**. O Catálogo lista produtos com
preço da lista escolhida e filtra pela busca.

### 4.2 — Detalhe do cliente (contato, mapa e histórico)

**Objetivo:** conferir a nova tela de detalhe do cliente.

**Passo a passo:**

1. Em **Clientes**, toque em um cliente.
2. Teste os botões **Ligar**, **WhatsApp**, **E-mail** e **Mapa** (os que
   tiverem dados).
3. Veja o **histórico de pedidos** e toque em "Novo pedido para este cliente".

**Resultado esperado:** os botões abrem telefone/WhatsApp/e-mail/mapa
corretamente. O histórico mostra os pedidos **daquele cliente**. "Novo pedido"
já abre com o cliente selecionado.

### 4.3 — Meus Pedidos: busca, filtro por etapa e detalhe com linha do tempo

**Objetivo:** conferir busca/filtro e o status detalhado do pedido.

**Passo a passo:**

1. Abra **Meus Pedidos**.
2. Use a **busca** (cliente ou número) e os **filtros por etapa**.
3. Toque em um pedido e veja a **linha do tempo** (criado → confirmado →
   produção → expedição → faturado).
4. Se o pedido tiver parcelas, veja a seção **Pagamento**.

**Resultado esperado:** a busca e os filtros mostram só os pedidos
correspondentes. A linha do tempo mostra a etapa atual com cores (verde =
concluído, azul = atual, cinza = a fazer). As parcelas aparecem com vencimento e
valor.

### 4.4 — Editar e cancelar rascunho (pedido ainda não enviado)

**Objetivo:** rascunhos não enviados podem ser editados ou cancelados.

**Passo a passo:**

1. Crie um pedido e **não sincronize** (fica em "Não enviados").
2. Toque em **Editar** — deve voltar ao carrinho com os itens.
3. Crie outro rascunho e toque em **Cancelar**.

**Resultado esperado:** "Editar" reabre o pedido no carrinho para alteração.
"Cancelar" pede **confirmação** e remove o rascunho.

### 4.5 — Trocar a lista de preços limpa o carrinho (com aviso)

**Objetivo:** ao trocar a lista com itens no carrinho, o app deve avisar e
limpar.

**Passo a passo:**

1. Inicie um **novo pedido**, adicione **alguns produtos**.
2. **Troque a lista de preços** para outra.

**Resultado esperado:** aparece uma **confirmação** avisando que o carrinho será
esvaziado. Ao **confirmar**, o carrinho fica vazio e a nova lista passa a valer.
Ao **cancelar**, o carrinho continua igual. (Com carrinho vazio, troca sem
perguntar.)

### 4.6 — Sincronizar não duplica (toque duplo e sem internet)

**Objetivo:** enviar pedidos não pode gerar duplicatas.

**Passo a passo:**

1. Tenha pedidos pendentes para enviar.
2. Toque em **"Sincronizar"** e, logo em seguida, **toque de novo** rápido.
3. Em outro teste: crie um pedido, ative o **modo avião**, tente sincronizar
   (falha), desative o modo avião e **sincronize de novo**.

**Resultado esperado:** cada pedido é enviado **uma única vez** (confira em
"Meus pedidos" e nas vendas do Real Controle). Nada é duplicado, mesmo com toque
duplo ou reenvio após queda de internet.

### 4.7 — O app reconhece o vendedor certo

**Objetivo:** confirmar o vínculo entre o login e o vendedor.

**Passo a passo:**

1. Entre com um usuário **vinculado a um vendedor** e sincronize.
2. Veja o nome do vendedor no Início e crie um pedido.

**Resultado esperado:** o app reconhece o vendedor e permite criar pedidos. Um
usuário **sem vínculo** vê um aviso e **não consegue** criar pedidos (peça ao
time para testar os dois casos).

---

## 5. Real Controle — Produção e Expedição

> Módulo: **Módulos → Real Controle**, nas áreas de **Produção** e
> **Expedição**.

### 5.1 — Quadro de Produção em Kanban (arrastar e soltar)

**Objetivo:** mover ordens entre colunas de status arrastando.

**Passo a passo:**

1. Abra o **Quadro de Produção** e alterne para a visão **"Quadro"**.
2. **Arraste** um cartão de ordem para outra coluna de status e solte.
3. **Recarregue** a página.

**Resultado esperado:** a ordem muda de coluna e **permanece** lá após
recarregar. Se o status "dá baixa", aparece um aviso (com opção **Desfazer**).
Cliques curtos/menus **não** disparam arraste por acidente (precisa mover um
pouco para começar a arrastar).

### 5.2 — Alternar entre Quadro e Tabela e configurar os cartões

**Objetivo:** o toggle de visão e a escolha de campos do cartão.

**Passo a passo:**

1. Use o botão para alternar entre **"Quadro"** e **"Tabela"**.
2. No botão **"Campos"**, marque/desmarque informações do cartão (cliente,
   itens, prazo, etc.).
3. **Recarregue** a página.

**Resultado esperado:** a visão e os campos escolhidos **continuam** após
recarregar. As duas visões mostram as mesmas ordens.

### 5.3 — Configuração de status (Produção e Expedição): salvar, reordenar, sem
duplicar

**Objetivo:** editar/reordenar status sem duplicar, nas duas áreas.

**Passo a passo:**

1. Abra a **configuração de status** de **Produção**.
2. Na primeira vez, confira os status padrão (**sem duplicatas**).
3. **Renomeie** um status, troque a **cor** e **salve**.
4. **Arraste/ordene** (setas ↑↓), feche e reabra.
5. Repita tudo na **Expedição**.

**Resultado esperado:** sem duplicatas; nome, cor e ordem **continuam salvos**
após reabrir, tanto em Produção quanto em Expedição.

---

## 6. Real Controle — Vendas

> Módulo: **Módulos → Real Controle → Vendas**.

### 6.1 — Agrupar (fundir) pedidos de venda

**Objetivo:** juntar 2+ orçamentos do **mesmo cliente** (ainda sem financeiro/
nota/produção) em um pedido só.

**Passo a passo:**

1. Selecione **2 ou mais** pedidos em **orçamento** do **mesmo cliente**.
2. Clique em **Ações → Agrupar pedidos**.
3. No modal, resolva as diferenças (data, vendedor, frete, etc.) escolhendo de
   qual pedido vem cada informação, e confirme.

**Resultado esperado:** nasce um **pedido novo** com os itens de todos
(sem duplicar). Os originais ficam **cancelados** com o selo **"Fundido em
#xxx"** e não podem mais ser editados. O sistema **não deixa** agrupar de
clientes diferentes nem pedidos que já têm financeiro/nota.

### 6.2 — Menu de Ações agrupado por categoria

**Objetivo:** as ações do menu ficaram organizadas por grupo.

**Passo a passo:**

1. Abra o menu (•••) de um pedido (e também o botão **"Ações"** com vários
   selecionados).

**Resultado esperado:** as ações aparecem agrupadas com títulos
(**Financeiro**, **Fiscal**, **Produção**, **Expedição**, **Documentos**),
facilitando achar cada uma. As ações que não se aplicam continuam escondidas/
desabilitadas como antes.

### 6.3 — Excluir uma venda já expedida devolve o estoque

**Objetivo:** ao apagar uma venda expedida, o estoque deve voltar.

**Passo a passo:**

1. Localize uma venda com status **expedido**.
2. Anote o **saldo de estoque** dos produtos.
3. **Apague** a venda e confirme.
4. Confira o estoque novamente.

**Resultado esperado:** a quantidade que tinha saído **volta** ao estoque. Não
há saldos negativos nem valores estranhos.

### 6.4 — Reordenar e redimensionar colunas das tabelas

**Objetivo:** as tabelas permitem mover, ordenar e esconder colunas.

**Passo a passo:**

1. Em uma tabela (ex.: Vendas), **arraste** o cabeçalho de uma coluna para outra
   posição.
2. No menu do cabeçalho, use **classificar**, **agrupar** e **ocultar coluna**.
3. **Recarregue** a página.

**Resultado esperado:** a nova ordem das colunas **permanece** após recarregar.
Classificar/agrupar/ocultar funcionam normalmente.

---

## 7. Real Controle — Força de Vendas (Representantes)

> Módulo: **Real Controle → Força de Vendas → Representantes**.

### 7.1 — Vincular um representante a um usuário de acesso

**Objetivo:** ligar o vendedor a um login para usar o app mobile.

**Passo a passo:**

1. Crie/edite um **representante**.
2. No campo **"Usuário de acesso"**, escolha um membro da organização.
3. Salve. Tente vincular o **mesmo** usuário a **outro** representante.

**Resultado esperado:** o representante passa a ter um login vinculado. Um
usuário **já vinculado** a outro representante aparece como **"já vinculado"** e
**não pode** ser escolhido de novo. Deixar **"Sem acesso"** também é permitido.

---

## 8. Plano de Contas — arrastar para reordenar categorias

> Onde fica: **Plano de Contas** (árvore de categorias).

### 8.1 — Reordenar categorias arrastando

**Objetivo:** reordenar categorias/subcategorias e manter a ordem.

**Passo a passo:**

1. Abra o **Plano de Contas**.
2. **Arraste** uma categoria (ou subcategoria) para outra posição no mesmo
   nível.
3. **Recarregue** a página.

**Resultado esperado:** a nova ordem **permanece** após recarregar.

---

## 9. Real Plano — Planejamento Financeiro

> Módulo: **Módulos → Real Plano**. Abra um plano para ver as abas (DRE, Fluxo
> de Caixa, Balanço, Ponto de Equilíbrio, NCG, Cenários, Orçado×Realizado).

### 9.1 — Exportar relatórios em PDF e Excel

**Objetivo:** confirmar a exportação dos relatórios.

**Passo a passo:**

1. Dentro de um plano, em qualquer aba, clique em **Exportar → PDF** e
   **Exportar → Excel**.
2. Na **lista de planos**, use **"Baixar tudo"** em um plano.
3. Abra os arquivos.

**Resultado esperado:** os arquivos abrem normalmente; o **PDF** traz capa,
indicadores, tabela e gráfico; o **Excel** traz uma aba por relatório. Os
**números nos arquivos batem com os números da tela**.

### 9.2 — Visual dos relatórios (igual ao Real Análise)

**Objetivo:** conferir a nova apresentação das tabelas e gráficos.

**Passo a passo:**

1. Abra as abas **DRE**, **Fluxo de Caixa**, **Balanço**, etc.
2. **Role** a página para baixo e para os lados.
3. Abra um plano com **nome bem longo**.

**Resultado esperado:** o cabeçalho fica **fixo** ao rolar; as colunas têm
largura própria com rolagem lateral; o **gráfico de fluxo de caixa** ocupa a
largura inteira; o nome longo do plano aparece **encurtado** (com o nome
completo ao passar o mouse), sem cobrir os menus.

### 9.3 — Os números das contas estão coerentes (auditoria contábil)

**Objetivo:** confirmar, de forma simples, que as contas "fecham".

**Passo a passo:**

1. Na aba **Balanço**, compare, mês a mês, o **total do Ativo** com o **total de
   Passivo + Patrimônio Líquido**.
2. Na aba **Ponto de Equilíbrio**, monte um cenário ruim (custos altos, receita
   baixa).
3. Confira os mesmos números na aba **Cenários** e na exportação.

**Resultado esperado:** Ativo = Passivo + Patrimônio Líquido (diferença de no
máximo centavos de arredondamento). Quando o negócio não cobre os custos, o
Ponto de Equilíbrio aparece como **"N/A (inviável)"** em vez de "R$ 0". Os
valores **não aparecem duplicados** e batem entre tela, Cenários e exportação.

> Se você notar **qualquer** diferença grande de valores, **avise o time** — são
> correções de cálculo sensíveis.

### 9.4 — Orçado × Realizado: fonte do realizado e "Não classificado"

**Objetivo:** escolher a fonte do realizado e ver categorias sem mapeamento.

**Passo a passo:**

1. Abra a aba **Orçado × Realizado**.
2. No **seletor de fonte do realizado**, alterne entre **"Manual (planilha)"** e
   **"Ao vivo"** (Real Controle e/ou conexões de ERP).
3. Role a tabela até o fim.

**Resultado esperado:** os valores do realizado mudam conforme a fonte
escolhida. Lançamentos sem categoria-padrão aparecem agrupados em **"Não
classificado"** (em vez de sumirem).

---

## 10. Real Análise — Integrações (Omie/Bling)

> Módulo: **Módulos → Real Análise → Integrações**. Precisa de credenciais de
> teste da Omie (peça ao time).

### 10.1 — Credenciais erradas mostram erro rápido (não trava)

**Objetivo:** chave da Omie **errada** deve avisar **rapidamente**.

**Passo a passo:**

1. Configure a **Omie** com **App Key/Secret inválidos**.
2. Clique em **Testar/Conectar**.

**Resultado esperado:** aparece **erro de credencial em poucos segundos**. O
sistema **não** fica travado tentando, e os dados existentes não somem.
(Com credenciais **corretas**, conectar e sincronizar funcionam normalmente.)

### 10.2 — Renomear e excluir uma conexão

**Objetivo:** confirmar renomear e excluir conexões.

**Passo a passo:**

1. No card de uma conexão, clique no **lápis** e troque o nome.
2. Em outra conexão de teste, clique em **Excluir** e confirme.

**Resultado esperado:** o nome é atualizado na hora. A exclusão pede
**confirmação** (avisando que remove a conexão e os dados sincronizados por ela)
e, ao confirmar, a conexão some.

### 10.3 — Tratamento de categorias usa o ERP conectado

**Objetivo:** as categorias mostradas devem ser do ERP realmente conectado.

**Passo a passo:**

1. Conecte um ERP (ex.: **Omie**) e sincronize.
2. Vá em **Real Análise → Tratamento de categorias → aba "Campos"**.

**Resultado esperado:** aparecem as **categorias do ERP conectado** (não fica
mais preso ao Bling). Se trocar o ERP conectado, a lista acompanha.

### 10.4 — Seletor de fonte no DRE e no Fluxo de Caixa

**Objetivo:** consolidar dados de mais de uma fonte.

**Passo a passo:**

1. Em **DRE** (e em **Fluxo de Caixa**), abra **Configurações (⚙️)**.
2. Em **"Fonte de dados"**, marque **Real Controle** e as conexões de ERP
   desejadas.

**Resultado esperado:** os relatórios recalculam somando as fontes escolhidas,
**sem duplicar** os valores. Sem nenhuma fonte marcada, volta para Real
Controle.

### 10.5 — Painéis de análise do DRE

**Objetivo:** conferir os quadros de Análise Vertical e Ponto de Equilíbrio.

**Passo a passo:**

1. Abra **Real Análise → DRE** e role abaixo da tabela.
2. Em **Configurações (⚙️) → Análises**, ligue/desligue os painéis.

**Resultado esperado:** aparecem os quadros de **Análise Vertical** (cada linha
em % da receita) e **Ponto de Equilíbrio**, alinhados com a tabela; ligar/
desligar mostra/esconde os quadros.

### 10.6 — ⚠️ APÓS A PUBLICAÇÃO: sincronizar para repovoar o Real Análise

**Objetivo:** confirmar que, depois da versão ir ao ar, **Fluxo de Caixa** e
**DRE** voltam a aparecer após uma sincronização **pela tela**.

> Importante: esta versão faz uma **limpeza interna** dos lançamentos
> financeiros do Real Análise. Por isso eles ficam **temporariamente vazios**
> até a **primeira sincronização**. Isso é **esperado** e essa ação **precisa
> ser feita** ao menos uma vez após a publicação.

**Passo a passo:**

1. Vá em **Real Análise → Integrações**.
2. Antes de sincronizar, abra **Fluxo de Caixa** e **DRE**: podem estar
   **vazios**. É normal neste momento.
3. Em cada conexão, clique em **"Sincronizar"** e aguarde terminar (pode levar
   **alguns minutos**).
4. Abra de novo **Fluxo de Caixa** e **DRE**.

**Resultado esperado:** depois da sincronização, **Fluxo de Caixa** e **DRE**
voltam a mostrar os lançamentos, **sem duplicação** (cada lançamento uma vez).

---

## 11. Acesso à conta (período de avaliação)

> Peça ao time contas de teste nos cenários abaixo.

### 11.1 — Conta com período expirado é bloqueada

**Objetivo:** uma conta expirada não deve acessar o sistema.

**Passo a passo:**

1. Faça login com uma conta de **período expirado**.

**Resultado esperado:** aparece uma **tela cheia** dizendo que o período de
acesso **expirou**, com botões para **falar pelo WhatsApp** e **Sair**. Nenhum
módulo fica acessível por trás. **Administradores não** são bloqueados.

### 11.2 — Aviso de expiração próxima (3 dias ou menos)

**Objetivo:** conta perto de expirar deve mostrar aviso, mas continuar usável.

**Passo a passo:**

1. Faça login com uma conta que **expira em 3 dias ou menos**.

**Resultado esperado:** aparece um **aviso no topo** ("seu período termina em X
dia(s)") com link de contato, **mas o sistema continua acessível** por baixo.
Contas convidadas por uma organização têm **acesso permanente** (sem aviso/
bloqueio).

---

## 12. O que NÃO precisa de teste manual (apenas para transparência)

Estas mudanças são internas e **não mudam o que o usuário vê** — não há o que
testar pela tela:

- **Medição de uso/engajamento** (registro invisível de quais módulos são mais
  acessados) e o painel de engajamento na Administração.
- Ajustes internos de **segurança de dados entre empresas** e de **cadastro/
  período de teste** de novas contas.
- Reversões internas de **custo/estoque** (estorno de compra restaura o custo
  anterior, baixas/estornos cost-neutral) — só importam pelos **saldos**, já
  cobertos nos itens 6.3 e do estoque.
- **Precisão dos números** (valores monetários) foi **mantida/corrigida** — se
  algo parecer diferente, é proposital (auditoria contábil), mas **avise o time**
  se a diferença for grande.
- Pequenas correções de **formatação de código** (sem efeito visual).

Se, ao testar, você notar **qualquer** diferença em valores, totais, ou dados
aparecendo trocados entre empresas, **avise o time imediatamente**.

---

## Resumo para marcação

| Item | Cenário | Status |
|------|---------|--------|
| 1.1 | Novidades viraram mural de cards (sem aba de ideias) | |
| 1.2 | Contador de não lidos + "marcar como lido" persiste | |
| 1.3 | Detalhe com imagem/galeria, vídeo e link | |
| 1.4 | Criar/editar/apagar novidade (admin) | |
| 1.5 | Recusa de não-imagem e de imagem > 5 MB | |
| 1.6 | Link só aceita http/https | |
| 2.1 | Interface 15% menor sem quebrar nada | |
| 3.1 | Reordenar status do orçamento (arrastar) | |
| 3.2 | Status do orçamento persiste ao reabrir | |
| 3.3 | Prévia do DRE mostra valor mais recente | |
| 3.4 | Opção "Espremer em 1 página" removida | |
| 4.1 | Mobile: Início (KPIs) e Catálogo | |
| 4.2 | Mobile: detalhe do cliente (contato/mapa/histórico) | |
| 4.3 | Mobile: Meus Pedidos (busca/filtro) + linha do tempo | |
| 4.4 | Mobile: editar e cancelar rascunho | |
| 4.5 | Mobile: trocar lista de preços limpa o carrinho | |
| 4.6 | Mobile: sincronizar não duplica (toque duplo/offline) | |
| 4.7 | Mobile: app reconhece o vendedor certo | |
| 5.1 | Quadro de Produção Kanban (arrastar e soltar) | |
| 5.2 | Alternar Quadro/Tabela + campos do cartão | |
| 5.3 | Status de Produção e Expedição (salvar/reordenar/sem duplicar) | |
| 6.1 | Agrupar (fundir) pedidos de venda | |
| 6.2 | Menu de Ações agrupado por categoria | |
| 6.3 | Excluir venda expedida devolve o estoque | |
| 6.4 | Reordenar/redimensionar colunas das tabelas | |
| 7.1 | Vincular representante a um usuário de acesso | |
| 8.1 | Plano de Contas: reordenar categorias (arrastar) | |
| 9.1 | Real Plano: exportar PDF/Excel + "Baixar tudo" | |
| 9.2 | Real Plano: visual dos relatórios (cabeçalho fixo, etc.) | |
| 9.3 | Real Plano: contas "fecham" (Balanço, PE inviável) | |
| 9.4 | Real Plano: fonte do realizado + "Não classificado" | |
| 10.1 | Omie: credencial errada avisa rápido | |
| 10.2 | Renomear e excluir conexão | |
| 10.3 | Tratamento de categorias usa o ERP conectado | |
| 10.4 | Seletor de fonte no DRE e Fluxo de Caixa | |
| 10.5 | Painéis de análise do DRE | |
| 10.6 | Após publicar: sincronizar repovoa Fluxo de Caixa/DRE | |
| 11.1 | Conta expirada é bloqueada | |
| 11.2 | Aviso de expiração próxima (≤ 3 dias) | |
