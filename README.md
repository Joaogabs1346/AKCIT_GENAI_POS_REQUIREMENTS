# Eventus SGE — Análise e Especificação de Requisitos com IA Generativa

## 1. Sobre o trabalho

Atividade de Engenharia de Requisitos apoiada por IA generativa (item 3.4 do enunciado), sobre o
**Eventus SGE** — sistema que substitui formulários on-line e planilhas na gestão de congressos,
workshops e eventos corporativos da empresa fictícia Eventus. O insumo é um único documento de
elicitação: 5 grupos de stakeholders, 14 falas de entrevista (P1–P5, O1–O5, F1–F3, L1) e 9 pontos
declaradamente sem definição (OB1–OB9) — 23 itens rastreáveis. Dele saíram 4 documentos de análise
e 8 artefatos de especificação, todos ancorados nesses 23 códigos de origem.

---

## 2. O problema em uma frase, e a tese da especificação

**A Eventus não tem um processo de inscrição — tem vários, e nunca escreveu qual vale para qual
tipo de evento.**

As nove indefinições da elicitação não são nove problemas independentes. Oito delas — prazo de
cancelamento (OB1), hipóteses de reembolso (OB2), funcionamento da fila (OB3), critério do
certificado (OB4), canais de notificação (OB5), instante da reserva da vaga (OB6), tratamento de
conflito de horário (OB7) e dados visíveis ao palestrante (OB8) — são a mesma pergunta feita oito
vezes: *qual política se aplica a este evento?* Um congresso pago e público, um workshop gratuito e
aberto e um encontro corporativo fechado e faturado exigem respostas diferentes para as oito, e
nenhuma resposta única é correta. Por isso a especificação introduz o **Perfil de Política do
Evento** (`PoliticaEvento`): oito parâmetros configurados por evento antes da abertura das
inscrições, herdados pelas atividades, congelados na inscrição no instante da confirmação e
irretroativos depois disso. A indefinição deixa de ser pendência de documento e vira parâmetro de
produto — é isso que permite construir antes da homologação, com o risco reduzido a reconfiguração
em vez de reescrita. A nona (OB9) é de outra natureza: não é política, é a ausência de qualquer
linha de base de segurança, desempenho, disponibilidade, acessibilidade e privacidade, tratada em
[requisitos não funcionais](analise/requisitos-nao-funcionais.md) como proposta a homologar.

---

## 3. Estrutura do repositório

```text
AKCIT_GENAI_POS_REQUIREMENTS/
├── README.md                              Este documento: escolha de artefatos, método e uso da IA
├── analise/
│   ├── requisitos-funcionais.md           34 RF em 11 módulos, com origem rastreada, MoSCoW, dependências e critério de verificação
│   ├── requisitos-nao-funcionais.md       24 RNF por categoria ISO/IEC 25010 e conformidade legal, 5 cenários de qualidade e inventário LGPD
│   ├── regras-de-negocio.md               30 RN tipadas (fato, restrição, cálculo, derivação, inferência), 15 invariantes e precedência entre regras
│   └── duvidas-e-lacunas.md               24 questões abertas (6 AMB · 5 INC · 13 LAC), agenda de homologação em 11 blocos e 10 riscos de requisitos
└── especificacao/
    ├── 01-mapa-de-historias.md            Jornada em 8 etapas e 41 atividades, com as 24 histórias fatiadas em MVP, R2 e R3
    ├── 02-historias-de-usuario.md         24 histórias com ficha de rastreabilidade e 80 cenários de aceitação em Gherkin
    ├── 03-casos-de-uso.md                 8 casos de uso expandidos, com o critério objetivo que promoveu cada um e os 12 fluxos não promovidos
    ├── 04-modelo-de-dominio.md            Glossário de 49 termos, 17 entidades, 7 decisões de modelagem e 32 regras de integridade
    ├── 05-ciclo-de-vida-da-inscricao.md   14 estados, 35 transições com guarda e efeito, temporizadores e matriz de transições proibidas
    ├── 06-tabelas-de-decisao.md           7 tabelas parametrizadas pelo Perfil de Política, 52 colunas de regra e 18 células pendentes de homologação
    ├── 07-prototipo-lo-fi.md              9 telas em baixa fidelidade cobrindo 17 das 24 histórias, com roteiro de validação
    └── 08-matriz-de-rastreabilidade.md    Rastreio nos dois sentidos, matriz de 40 operações por 5 papéis, indicadores de cobertura e 13 buracos declarados
```

Acesso direto aos documentos de análise: [requisitos funcionais](analise/requisitos-funcionais.md) ·
[requisitos não funcionais](analise/requisitos-nao-funcionais.md) ·
[regras de negócio](analise/regras-de-negocio.md) ·
[dúvidas e lacunas](analise/duvidas-e-lacunas.md).

Marcações usadas em todos os arquivos: `⚠️ DECISÃO PROPOSTA` para valor recomendado por este
trabalho e ainda não homologado; `❓` para pergunta sem recomendação fechada. Todo identificador
(`RF`, `RNF`, `RN`, `AMB`, `INC`, `LAC`, `HU`, `CA`, `UC`, `TD`, `E`, `CT`) vem de um registro
canônico único; nenhum artefato cria identificador novo.

---

## 4. Artefatos escolhidos e por quê

| Artefato | Pergunta que ele responde | Por que ele era necessário neste projeto |
|---|---|---|
| [01 — Mapa de histórias](especificacao/01-mapa-de-historias.md) | O que precisa existir na primeira abertura de inscrições com dados reais, e o que pode esperar? | A data do evento é externa e não negocia: sem fatiamento explícito, a lista de 34 RF ordenada por prioridade não diz onde é seguro cortar |
| [02 — Histórias de usuário](especificacao/02-historias-de-usuario.md) | Como se prova que o comportamento entregue é o combinado, sem tradução intermediária? | Cada política é um comportamento diferente para o mesmo fluxo; o cenário em Gherkin é o formato que vira teste e detecta regressão de política |
| [03 — Casos de uso](especificacao/03-casos-de-uso.md) | O que acontece quando duas pessoas disputam a última vaga, o prestador de pagamento some ou o prazo vence no meio da operação? | Nos 8 fluxos promovidos, o caminho feliz é a parte irrelevante: o valor está nas exceções de concorrência, dinheiro e relógio |
| [04 — Modelo de domínio](especificacao/04-modelo-de-dominio.md) | Onde cada dado mora, quem é o seu titular e qual estado o modelo nunca pode apresentar? | Vaga, reserva, posição na fila, pagamento, presença e consentimento são objetos distintos com ciclos de vida distintos; tratá-los como campos de uma tabela de inscrição inviabiliza a regra de vaga e a de privacidade |
| [05 — Ciclo de vida da inscrição](especificacao/05-ciclo-de-vida-da-inscricao.md) | Em qual transição a vaga é consumida, o dinheiro passa a ser devido e o certificado é liberado? | OB1, OB3, OB4 e OB6 são a mesma pergunta feita em pontos diferentes de uma linha do tempo que dura semanas; só a máquina de estados responde às quatro sem contradição |
| [06 — Tabelas de decisão](especificacao/06-tabelas-de-decisao.md) | Dadas todas as combinações de política possíveis, alguma ficou sem desfecho definido? | É onde a tese vira artefato executável: a tabela é a função e o Perfil de Política é o argumento; a lacuna aparece como coluna faltante, não como silêncio |
| [07 — Protótipo de baixa fidelidade](especificacao/07-prototipo-lo-fi.md) | O que o participante lê quando o sistema diz não? | Ninguém homologa `RN-22`; todo mundo tem opinião sobre a frase "volta 50 % se você desistir depois de 06/05". É o instrumento que leva a política a quem decide a política |
| [08 — Matriz de rastreabilidade](especificacao/08-matriz-de-rastreabilidade.md) | Alguma fala ficou sem requisito, algum requisito ficou sem origem, e o que ainda não tem prova? | Com 34 RF, 30 RN e 24 questões abertas espalhados por 12 arquivos, cobertura vira memória; a matriz troca memória por contagem |

**01 — Mapa de histórias.** Um sistema de eventos tem prazo externo: se o Congresso de maio abrir
inscrições sem o produto pronto, volta-se à planilha. O mapa organiza as 24 histórias em 8 etapas e
41 atividades da jornada e mostra a fatia mínima que já resolve a dor original — controle de vagas
sem sobrevenda, cobrança e comprovante — separando-a do que pode ir para R2 e R3. Ele também expõe
as 5 capacidades canônicas sem história dedicada (RF-01, RF-02, RF-10, RF-11, RF-28), buraco que só
fica visível quando a jornada inteira é desenhada em uma superfície só.

**02 — Histórias de usuário.** A variabilidade de política significa que o mesmo botão "Cancelar"
tem quatro comportamentos legítimos. Descrever isso em prosa produz interpretação; descrever em 80
cenários Gherkin produz teste. Cada história traz persona, release, requisitos, regras e pendências
associadas, e usa a mesma instância de referência (Congresso Eventus de Tecnologia 2026, Workshop de
Engenharia de Prompt, Encontro Corporativo Nexa) para que os números dos cenários sejam conferíveis
entre si.

**03 — Casos de uso.** Só virou caso de uso o fluxo que marcou três ou mais de sete gatilhos —
ramificação real, concorrência, dinheiro, prazo, mudança de estado, mais de um ator, exposição
regulada. Isso promoveu 8 fluxos e manteve 12 fora, cada um com o lugar onde ficou especificado.
O corte importa porque a concorrência por vaga e a assincronia do pagamento são exatamente o que
uma história linear esconde: em `UC-01`, a reserva pode vencer durante a própria execução do caso.

**04 — Modelo de domínio.** Este domínio tem três armadilhas de vocabulário que a elicitação já
manifesta: "evento" ora significa o congresso ora o workshop (AMB-06), "inscrito" ora é quem
solicitou ora é quem pagou (INC-01), e "vaga" ora é capacidade ora é disponibilidade (RN-20).
O glossário de 49 termos e as 17 entidades fixam o vocabulário; as 32 regras de integridade
declaram o que o modelo nunca pode apresentar — ocupação acima da capacidade, dois registros de
presença válidos para o mesmo par, campo de contato devolvido a palestrante sem consentimento.

**05 — Ciclo de vida da inscrição.** A inscrição vive semanas: é criada, reserva vaga, aguarda
liquidação, pode expirar, ser convidada pela fila, cancelada, restituída, presenciada e
certificada. Nesse intervalo, quatro atores de tempo agem sem ação humana. Os 14 estados e as 35
transições com guarda, efeito e notificação são o que impede que "expirou" e "cancelou" produzam
efeitos financeiros diferentes por acidente de implementação — e a matriz de transições proibidas
diz explicitamente o que o motor de estados deve recusar.

**06 — Tabelas de decisão.** Sete tabelas cobrem cancelamento, restituição, certificado,
sobreposição de horário, promoção da fila, desfecho da submissão e visibilidade de campos por
papel, somando 52 colunas de regra com desfecho definido. É o único formato em que se demonstra,
por contagem, que nenhuma combinação de política ficou sem resposta — e em que a pendência aparece
identificada: 18 células operam hoje com valor padrão marcado `⚠️`, cada uma com o responsável pela
homologação e o efeito de uma divergência.

**07 — Protótipo de baixa fidelidade.** As nove telas existem para validação, não para
implementação: não há paleta, tipografia nem componente. Elas cobrem 17 das 24 histórias e servem
para que Rafael Nunes conteste a frase da recusa de cancelamento e Dra. Helena Prado veja
exatamente quais campos a lista de inscritos devolve. Desenhar as telas também produziu regras que
o texto não pedira — a tela precisa decidir o que exibir quando o dado não existe.

**08 — Matriz de rastreabilidade.** Fecha o ciclo nos dois sentidos: 23 de 23 itens da elicitação
têm ao menos um requisito, e cada RF aponta origem, regra, história, caso de uso, tabela, estado e
caso de teste. A matriz de 40 operações por 5 papéis é onde a LGPD deixa de ser aviso e vira regra
de composição da resposta. E os indicadores declaram o elo fraco em vez de escondê-lo: 59 % dos RF
e 46 % dos RNF têm caso de teste, contra 85 % a 91 % de cobertura de análise.

### Por que estes oito, e não mais

O critério foi o mesmo aplicado à promoção de casos de uso: **um artefato só entrou se respondia a
uma pergunta que nenhum outro respondia.** As perguntas deste sistema são cinco — qual política se
aplica, quem ganha a vaga sob concorrência, em que ponto do ciclo de vida cada obrigação nasce,
quem pode ver o quê, e o que já está provado. O conjunto as cobre sem sobreposição: 01 e 02 tratam
de escopo e comportamento observável; 03 e 05 tratam do mesmo fluxo em recortes complementares —
casos de uso descrevem interação entre atores, a máquina de estados descreve o que muda sozinho com
o tempo; 04 e 06 tratam de estrutura e de regra, um sem o outro produz regra sobre vocabulário
ambíguo; 07 é o único artefato que um stakeholder não técnico consegue contestar; 08 é o único que
mede a própria cobertura. Retirar qualquer um deixa uma das cinco perguntas sem dono. Acrescentar
um nono significaria descrever duas vezes o mesmo comportamento — e dois documentos sobre o mesmo
fluxo divergem na primeira mudança.

---

## 5. Artefatos avaliados e descartados

| Artefato | Situação | Justificativa técnica |
|---|---|---|
| Diagrama BPMN formal dos processos | Descartado | BPMN é forte em raias, tarefas e mensagens entre organizações. Aqui os desvios que importam são disparados por temporizadores e por notificação assíncrona do prestador, e a decisão depende de política parametrizada: o mesmo diagrama precisaria de uma variante por perfil, ou de gateways condicionais que reproduzem, com menos precisão, o que TD-01 a TD-07 já expressam. Além disso, o comportamento sem ator humano é modelado com mais fidelidade em máquina de estados |
| Diagrama de sequência por fluxo | Descartado, com uma exceção | A granularidade de mensagem só é informativa onde a ordem temporal entre participantes é o conteúdo. Isso ocorre em um ponto: a corrida pela última vaga, mantida como tabela instante a instante na exceção `4e` de `UC-01`. Nos demais, o diagrama repetiria os passos numerados do caso de uso e passaria a divergir deles na primeira revisão |
| Diagrama de classes de implementação | Descartado | Exigiria decidir agora atributos técnicos, tipos, visibilidade e estratégia de persistência — decisões de projeto que dependem de escolhas ainda não feitas (prestador de pagamento, mecanismo de bloqueio para RNF-06, formato da trilha de RNF-16). O que a especificação precisa fixar é o vocabulário e as invariantes, e isso está no modelo de domínio, deliberadamente sem chave estrangeira, índice ou tabela de junção |
| Especificação completa no padrão IEEE 830 | Descartado | O padrão organiza requisitos por seção fixa e trata cada requisito como sentença isolada. Neste projeto, o conteúdo de maior valor é relacional — qual regra depende de qual parâmetro, qual estado habilita qual ação, qual fala originou qual requisito. Um documento monolítico no formato 830 diluiria isso em prosa e ainda concorreria com a matriz de rastreabilidade, que faz o mesmo trabalho de forma conferível |
| Protótipo de alta fidelidade | Descartado | Em sessão de homologação, tela acabada desloca a discussão para cor, ícone e posição de botão, e produz aceite prematuro por aparência de pronto. O que precisa ser contestado por Rafael Nunes e Cleide Barros é a regra escrita na tela, não o seu acabamento. Também seria descartável: as decisões de LAC-01 a LAC-08 podem mudar a tela inteira |
| Backlog em ferramenta (Jira, Trello, Azure Boards) | Descartado | Backlog é instrumento de execução, não de especificação, e fragmenta o conteúdo em cartões sem contexto — a regra deixa de ser legível fora da ferramenta. O sequenciamento que a entrega exige já está no mapa de histórias, em formato versionável junto com o restante do repositório; a importação para uma ferramenta é passo de planejamento, posterior à homologação |
| Documento de visão | Descartado | Visão, escopo, público-alvo e proposta de valor estão dados pela elicitação e não são objeto de análise nesta atividade. Um documento de visão aqui seria derivado de artefatos existentes e envelheceria sem que ninguém percebesse, porque nada depende dele |
| Modelo de casos de uso UML completo, com `include` e `extend` | Descartado, com uso parcial | O diagrama de casos de uso foi mantido para mostrar atores e fronteira, incluindo os atores não humanos (temporizadores, prestador de pagamento). O modelo completo com decomposição por `include`/`extend` foi rejeitado: a distinção entre os dois estereótipos é fonte conhecida de divergência de leitura, e a fatoração de passos comuns criaria um segundo lugar onde a mesma regra vive. Neste domínio, o passo compartilhado relevante é "consumir vaga de forma atômica", que é invariante de RN-07, não fragmento de caso de uso |
| Matriz RACI de stakeholders | Descartado e substituído | RACI descreve responsabilidade genérica de projeto e não responde à pergunta que este trabalho precisava responder: **quem homologa cada número**. Cada uma das 24 questões abertas traz decisor nomeado, e a agenda de homologação distribui as 24 em 11 blocos ordenados por precedência |
| Diagrama de atividades por requisito | Descartado | Redundante com casos de uso e máquina de estados nos fluxos que importam, e desproporcional nos que não importam. Manter três representações do mesmo fluxo multiplica o custo de manutenção sem acrescentar decisão |

---

## 6. Ferramenta de GenAI utilizada

| Item | Valor |
|---|---|
| Ferramenta | **Claude Code**, da Anthropic |
| Modelo | **Claude Opus 5** |
| Modo de execução | Terminal, com leitura e escrita direta dos arquivos do repositório |
| Organização | Orquestração multiagente com registro canônico compartilhado |

O método não foi conversar com um chat e colar o resultado. Foi organizado em quatro movimentos:

1. **Leituras independentes da elicitação por lentes diferentes.** O mesmo documento fonte foi lido
   em passagens separadas com objetivos distintos — comportamento pedido, restrição de negócio,
   contradição entre falas, omissão, exposição regulada. Passagens independentes encontram coisas
   diferentes: a leitura de contradição foi a que produziu INC-01 (comprovante imediato de P2 contra
   liberação condicionada de F3) e INC-04 (controle automático de vagas de O1 sem instante de
   reserva definido em OB6), que a leitura de "requisitos" não vê porque cada fala, isolada, é
   razoável.
2. **Consolidação em um registro canônico de identificadores.** Antes de qualquer artefato, um
   único registro fixou 34 RF, 24 RNF, 30 RN, 24 questões abertas, 24 HU, 8 UC, 7 TD, 14 estados e
   26 CT, com título, origem e prioridade de cada entrada. Nenhum autor de artefato podia criar
   identificador: faltando um conceito, cita-se o ID mais próximo. É o que impede deriva de
   numeração entre documentos escritos em momentos distintos — o defeito mais comum e mais difícil
   de detectar em documentação gerada em paralelo.
3. **Redação paralela dos artefatos.** Os 12 arquivos foram escritos por agentes distintos, cada um
   recebendo o mesmo briefing, o mesmo documento fonte e o mesmo registro canônico. Paralelizar
   aumenta a cobertura e o custo de coerência na mesma proporção — daí o movimento seguinte.
4. **Rodada de revisão adversarial cruzada.** Cada artefato foi relido contra os demais com a tarefa
   explícita de encontrar contradição, não de aprovar. Foi a etapa mais produtiva: apareceram a
   colisão entre "48 h para liberar" e "48 h para emitir" o certificado, a vaga que voltava ao
   público entre a liberação e o convite (contra RN-12 e CT-10), a dupla trava que tornava RF-32
   inalcançável, e várias contagens de cobertura que estavam plausíveis e erradas.

Duas honestidades sobre o uso da ferramenta. A primeira: a IA é boa em produzir volume coerente e
ruim em perceber que o volume não era necessário — a poda foi humana em todos os artefatos. A
segunda: número plausível é o erro mais difícil de enxergar em texto gerado, e por isso todas as
contagens deste README e da matriz de rastreabilidade foram recontadas contra os arquivos, não
aceitas como escritas.

---

## 7. Como a IA apoiou cada etapa

Etapas conforme o item 3.4.2 do enunciado.

| Etapa da atividade (3.4.2) | Papel da IA | Papel da decisão humana |
|---|---|---|
| 1. Analisar as informações do documento de elicitação | Leitura integral, codificação das 23 entradas, agrupamento por stakeholder e separação entre pedido, restrição e omissão | Reformulou a conclusão. A IA entregou OB1–OB9 como lista de pendências; a leitura adotada foi a de que oito delas são parâmetros de um mesmo objeto de configuração — decisão que reorganizou todo o restante do trabalho |
| 2a. Identificar requisitos funcionais | Primeira lista de RF agrupada nos módulos M1–M11 | Manteve a modularização e rejeitou o formato. Uma linha por requisito não comporta origem, prioridade, dependência e critério de verificação; cada RF passou a exigir os cinco. Descartou os CRUDs genéricos e exigiu que o requisito nomeasse o comportamento específico do domínio |
| 2b. Identificar requisitos não funcionais | Categorias ISO/IEC 25010 e metas iniciais para OB9 | Substituiu toda meta qualitativa por número com método de verificação, e recusou a apresentação como acordo: nenhum dos 24 RNF foi pedido pelo cliente, o que passou a constar como advertência de origem no próprio documento |
| 2c. Identificar regras de negócio | Enunciados candidatos derivados dos RF | Acrescentou a tipagem em cinco classes, o vínculo com o parâmetro de política e, para cada regra, um exemplo **e um contraexemplo** — o contraexemplo é o que revela a regra mal escrita, e reprovou várias na primeira redação |
| 2d. Identificar ambiguidades, inconsistências e lacunas | Varredura inicial de AMB, INC e LAC | Mudou a obrigação de saída: nenhuma entrada pode terminar em "pendente". Toda questão passou a exigir pergunta fechada, opções, recomendação padrão, decisor nomeado e impacto de não decidir. Também cortou "lacunas" que eram apenas ausência de detalhe de implementação |
| 3. Solicitar sugestões de artefatos | Lista de candidatos com prós e contras, incluindo BPMN, sequência, classes, IEEE 830, RACI e backlog | Tratou a lista como insumo, não como decisão. Nenhum artefato entrou por ser usual |
| 4. Selecionar e justificar os artefatos | Aplicação do critério de corte quando pedido, e argumentação a favor e contra cada candidato | Definiu o critério — um artefato só entra se responder a uma pergunta que nenhum outro responde — e arbitrou os casos de fronteira. O protótipo entrou por ser o único instrumento que leva a política a quem a homologa; BPMN e sequência saíram porque suas perguntas já tinham dono |
| 5a. Elaborar os artefatos | Redação dos 12 documentos a partir do briefing e do registro canônico, incluindo diagramas em Mermaid, cenários Gherkin e as tabelas de decisão | Fixou antes o vocabulário, os defaults e a instância de referência, e cortou conteúdo tecnicamente correto porém sem função — o volume gerado era maior do que o entregue |
| 5b. Revisar e ajustar | Releitura cruzada de cada artefato contra os demais e contra o registro canônico, procurando contradição | Arbitrou cada divergência encontrada e decidiu o que ficaria como buraco declarado em vez de ser fechado com conteúdo inventado. Os 13 buracos da seção 5 de `08` são resultado dessa decisão |

---

## 8. Sugestões aceitas

1. **Tratar as indefinições como parâmetros de uma entidade de política, e não como pendências.**
   Aceita porque é o que torna o sistema construível antes da homologação e o que dá unidade aos
   oito artefatos. Materializada em `RF-19`, `RN-03` e nas sete tabelas de decisão.
2. **Reserva temporária de vaga com expiração e contador visível.** Aceita porque resolve OB6 com
   mecanismo verificável em vez de acordo verbal: `RF-13` define o hold de 30 minutos, `RNF-06`
   exige zero sobrevenda sob 200 requisições concorrentes e `RNF-08` exige devolução da vaga em até
   60 segundos do vencimento.
3. **Lista de espera com convite nominal, prazo e promoção em cascata.** Aceita porque impede que
   "lista de espera" continue sendo caixa-preta: `TD-05` e `UC-03` descrevem o que acontece quando o
   convite expira, e `RN-12` garante que a vaga não retorna ao público durante a vigência.
4. **Check-in por sessão como fonte de verdade da elegibilidade ao certificado.** Aceita porque
   substitui declaração por dado observável, e porque resolve OB4 sem depender de confiança:
   `RN-23` calcula o percentual, `RN-24` calcula a carga horária declarada, `TD-03` decide a
   liberação.
5. **Distinguir comprovante de solicitação de comprovante de inscrição confirmada.** Aceita porque
   reconcilia P2 e F3 sem sacrificar nenhum dos dois: o participante recebe algo imediatamente, e
   esse algo declara em destaque que não garante vaga (`RN-05`, `INC-01`).
6. **Visibilidade de dados por papel construída sobre minimização e consentimento.** Aceita porque
   transforma a LGPD em regra de composição da resposta, e não em aviso legal no rodapé: `TD-07`
   resolve campo a campo, `RNF-17` exige teste automatizado a cada versão e propagação da revogação
   em até 60 segundos.
7. **Trilha de auditoria imutável para toda transição de estado.** Aceita porque, sem ela, nenhuma
   contestação sobre vaga ou dinheiro é decidível — e contestação é certa em um sistema que nega
   vaga, expira reserva e recusa reembolso (`RF-34`, `RNF-16`).
8. **Matriz de transições proibidas na máquina de estados.** Aceita porque, para quem implementa, a
   lista do que não pode acontecer é mais informativa que a lista do que pode: ela descreve
   exatamente o que o motor de estados deve recusar e registrar.
9. **Declarar os buracos de cobertura em vez de fechá-los com conteúdo.** Aceita porque um indicador
   de 59 % de cobertura de teste com a lista dos RF descobertos é útil; um indicador de 100 % obtido
   inventando casos de teste é pior que nenhum.

---

## 9. Sugestões modificadas

**1. Registro das indefinições**
- *O que a IA propôs:* listar OB1–OB9 como pendências a esclarecer com os stakeholders, no formato
  pergunta e status.
- *O que foi adotado:* toda questão aberta passou a exigir cinco campos — pergunta fechada, opções
  possíveis, recomendação padrão, decisor nomeado e impacto de não decidir —, com as 24 questões
  organizadas por precedência em uma agenda de homologação de 11 blocos.
- *Por quê:* documento que só lista dúvidas transfere o problema de volta a quem já demonstrou não
  ter a resposta. Documento que propõe default marcado `⚠️` permite construir agora e ajustar depois
  por configuração.

**2. Metas numéricas de desempenho, disponibilidade e segurança**
- *O que a IA propôs:* os 24 RNF redigidos na voz de requisito acordado ("o sistema deve responder
  em até 2 s"), como se houvesse elicitação por trás.
- *O que foi adotado:* as metas foram mantidas, com número e método de verificação, mas o documento
  abre com advertência de origem declarando que **nada disso foi pedido pelo cliente** — OB9 é
  exatamente a ausência desses requisitos — e cada meta carrega a marca de decisão proposta, com
  `LAC-09` designando Téo Miranda como responsável pela homologação da linha de base.
- *Por quê:* a meta é necessária, porque requisito não verificável não é requisito; mas apresentá-la
  como acordada seria fabricar consenso. O leitor precisa saber a diferença entre o que a Eventus
  disse e o que este trabalho propôs.

**3. Matriz de permissões**
- *O que a IA propôs:* uma matriz genérica de papéis por operação, com marcação de verbos do tipo
  criar, ler, atualizar e excluir.
- *O que foi adotado:* matriz de 40 operações por 5 papéis reescrita em torno de **escopo e
  condição**: `P` (apenas dados próprios), `E` (dentro do escopo atribuído — evento do organizador,
  atividade do palestrante, dia e sala do operador), `T` (transversal), `C` (condicionado a
  consentimento, parâmetro de política, segregação de função ou justificativa registrada) e `—`
  (negado, inclusive por caminho administrativo).
- *Por quê:* o verbo não é onde mora o risco aqui. "Organizador pode ler inscritos" é verdadeiro e
  inútil: o que importa é *de quais eventos* e *quais campos*. Sem escopo e condição, a matriz não
  expressa nem a restrição de OB8 nem a minimização exigida pela LGPD.

**4. Casos de uso expandidos**
- *O que a IA propôs:* um caso de uso expandido para cada requisito funcional relevante.
- *O que foi adotado:* critério de corte objetivo com sete gatilhos e promoção a partir de três,
  resultando em 8 casos de uso e 12 fluxos explicitamente não promovidos, cada um com o lugar onde
  ficou especificado.
- *Por quê:* caso de uso é caro de escrever e caro de manter. Aplicado a tudo, produz duplicação: o
  mesmo comportamento passa a existir em dois documentos que divergem na primeira mudança. O
  critério torna a decisão auditável em vez de arbitrária — inclusive no caso de fronteira `UC-08`,
  que marcou exatamente três.

**5. Prazo de 48 horas do certificado**
- *O que a IA propôs:* tratar as 48 horas de `RN-25` como prazo de emissão do certificado.
- *O que foi adotado:* 48 horas passaram a ser prazo de **liberação**; a emissão é autosserviço e
  pode ocorrer a qualquer momento depois disso.
- *Por quê:* a leitura original criava um estado proibido para quem emitisse o certificado no quinto
  dia — comportamento normal de autosserviço e obrigatório no caminho de revisão de presença
  deferida, em que a reapuração acontece depois do prazo. A contradição só apareceu na revisão
  cruzada, ao confrontar `RN-25` com a transição 34 da máquina de estados.

**6. Exibição do contato do participante ao palestrante**
- *O que a IA propôs:* exigir simultaneamente consentimento do titular **e** o valor `ampliada` no
  parâmetro de visibilidade do evento.
- *O que foi adotado:* `RN-15` exige apenas consentimento específico e vigente; o parâmetro do
  evento passou a graduar outra classe de campos.
- *Por quê:* duas travas independentes tornavam `RF-32` inalcançável sob o perfil mínimo, que é
  justamente o valor recomendado — o requisito existiria no papel e nunca na prática. Além disso,
  condicionar o efeito do consentimento a uma configuração do organizador inverte a lógica da LGPD:
  quem decide sobre o dado é o titular, não o controlador do evento.

---

## 10. Sugestões descartadas

1. **Gerar os diagramas formais antes de fechar as regras.** Descartada. A IA propôs começar por
   BPMN e diagramas de sequência, o que é rápido e parece produtivo. Mas com OB1–OB8 em aberto,
   qualquer diagrama de fluxo estaria desenhando uma política entre várias possíveis, e cada
   homologação exigiria redesenhar tudo. A ordem adotada foi inversa: fixar o Perfil de Política,
   depois as regras, depois as representações — e mesmo assim as representações escolhidas foram as
   que toleram parametrização (tabela de decisão e máquina de estados).
2. **Especificar a integração com um gateway de pagamento nomeado.** Descartada. A elicitação diz
   apenas que há eventos pagos (F1) e que o pagamento precisa ser confirmado antes de liberar
   determinadas inscrições (F3). Não há uma palavra sobre prestador, meios aceitos, parcelamento ou
   emissão fiscal. Especificar endpoints, webhooks e códigos de retorno de um prestador específico
   seria inventar elicitação. O que ficou foi o contrato observável — notificação autenticada e
   idempotente, não retenção de dados de cartão, estorno pelo mesmo meio — e `LAC-10` registrando a
   decisão pendente com Cleide Barros.
3. **Propor SLA contratual de disponibilidade.** Descartada na forma proposta. A IA sugeriu SLA com
   penalidade e crédito por indisponibilidade. Não existe histórico de operação — o sistema não
   existe, o processo atual é planilha — e SLA sem baseline é número inventado com consequência
   jurídica. Ficou apenas `RNF-09` como meta interna proposta (99,5 % mensal, 99,9 % em janela
   crítica), marcada como decisão proposta e sem qualquer efeito contratual.
4. **Criar histórias de usuário para funcionalidades que ninguém pediu.** Descartadas várias, entre
   elas rede de contatos entre inscritos com perfil visível, aplicativo nativo dedicado ao check-in
   e prioridade paga na lista de espera. Nenhuma tem origem na elicitação; a primeira amplia
   exatamente a superfície de dados pessoais que `RN-15` e `RNF-17` trabalham para reduzir; a
   segunda não resolve nada que o navegador com armazenamento local cifrado já não resolva
   (`RNF-12`); a terceira conflita com `RN-27`, que deriva a posição apenas da ordem cronológica.
   As descartadas ficaram registradas com a condição que as reabriria, ao final de
   [02-historias-de-usuario.md](especificacao/02-historias-de-usuario.md).
5. **Produzir um "resumo da análise" e um "esboço do sistema para apresentação".** Descartados.
   Ambos seriam integralmente derivados de artefatos existentes, sem responder a nenhuma pergunta
   nova, e envelheceriam em silêncio — documento derivado que ninguém atualiza é pior que documento
   ausente, porque é lido como verdade.
6. **Fechar os indicadores de cobertura em 100 %.** Descartada. A sugestão foi criar casos de teste
   para os RF descobertos até zerar o buraco. Casos de teste escritos para preencher indicador não
   verificam nada: viram texto. Preferiu-se declarar 59 % de cobertura de teste nos RF e 46 % nos
   RNF, com a lista nominal do que está descoberto e a condição objetiva de fechamento de cada um
   dos 13 buracos.

---

## 11. Limites do que foi produzido

- **Nenhum default foi homologado.** Os 48 h de cancelamento, os 30 min de reserva, as faixas de
  100 %/50 %/0 %, as 24 h do convite, o limiar de 75 % de presença e o teto de R$ 500,00 de
  aprovação automática são recomendações deste trabalho. Todas estão marcadas `⚠️` e com decisor
  nomeado, mas nenhuma tem ata.
- **Não houve validação com usuário real.** As cinco personas são fictícias e derivadas dos grupos
  de stakeholders da elicitação. O protótipo foi desenhado para uma sessão de validação que ainda
  não aconteceu; o roteiro existe, os resultados não.
- **As metas de desempenho, disponibilidade e capacidade carecem de baseline.** Números como p95 de
  1,5 s no catálogo, 3.000 sessões simultâneas no pico de abertura e 200.000 inscrições por ano
  foram dimensionados por analogia, não por medição — não há sistema em operação para medir. São
  hipóteses de projeto a confirmar com teste de carga antes da primeira abertura com dados reais.
- **O protótipo é de baixa fidelidade e não define identidade visual.** Não há paleta, tipografia,
  espaçamento, componente ou navegação definitiva. Ele valida conteúdo e comportamento; qualquer
  leitura dele como especificação de interface é indevida. Duas telas relevantes ficaram fora deste
  ciclo — check-in móvel e editor do Perfil de Política — por exigirem rodada própria.
- **A cobertura de verificação está atrás da cobertura de análise.** 59 % dos RF e 46 % dos RNF têm
  caso de teste; requisitos que sustentam a defesa jurídica do produto (`RF-33`, `RF-34`, `RNF-13`
  a `RNF-16`) ainda dependem de inspeção manual.
- **Três leituras seguem sem confirmação do stakeholder.** `AMB-04` (a sentença circular de O5 sobre
  simultaneidade), o item 2 de `TD-01` (representação de "não cancelável") e o item 7 de `TD-03`
  (carga horária no critério automático) foram fechados sobre interpretação própria, sinalizada
  com `❓` no ponto exato em que foi assumida.
- **Nenhum artefato de projeto foi produzido.** Não há arquitetura, modelo de dados físico, escolha
  de tecnologia nem estimativa de esforço. O escopo desta atividade termina na especificação.

---

## 12. Próximos passos

1. **Realizar a reunião de homologação das 24 decisões pendentes**, conforme a agenda de 11 blocos
   da seção 4 de [duvidas-e-lacunas.md](analise/duvidas-e-lacunas.md): sessão única de 3 h 30, com
   os decisores presentes apenas nos blocos em que respondem, e registro de decisão preenchido ao
   vivo. Sai com ata; o que não for decidido no bloco vira default por omissão após 5 dias úteis.
2. **Confirmar as três interpretações marcadas com `❓`** antes de qualquer construção — em especial
   `AMB-04`, cuja leitura sustenta `TD-04` inteira. Uma leitura diferente de O5 não muda um número:
   muda o objeto da tabela.
3. **Propagar as decisões homologadas para os artefatos**, na ordem de dependência já mapeada
   (cada bloco da agenda declara quais arquivos atualiza) e atualizar o registro canônico antes de
   qualquer edição, para que nenhum identificador nasça fora dele.
4. **Executar a sessão de validação do protótipo** com Rafael Nunes, Cleide Barros, Dra. Helena
   Prado e Téo Miranda, seguindo o roteiro da seção 5 de
   [07-prototipo-lo-fi.md](especificacao/07-prototipo-lo-fi.md), e desenhar a segunda rodada com as
   duas telas adiadas: check-in móvel e editor do Perfil de Política.
5. **Fechar os buracos de verificação prioritários** da seção 5 de
   [08-matriz-de-rastreabilidade.md](especificacao/08-matriz-de-rastreabilidade.md), começando por
   B-1 (RF-01 e RF-02 sem nenhum critério de aceitação), B-4 (RF-33 e RF-34 sem caso de teste) e
   B-12 (defaults numéricos sem teste parametrizado que force a falha quando o valor mudar), e
   montar o plano de verificação dos RNF com teste de carga, varredura de retenção de dados de
   cartão e ensaio trimestral de restauração.
