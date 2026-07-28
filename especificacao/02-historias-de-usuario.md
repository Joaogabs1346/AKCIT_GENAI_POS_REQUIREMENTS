# 02 — Histórias de Usuário e Critérios de Aceitação

Artefato central da especificação do **Eventus SGE**. Cada história descreve uma necessidade
observada na elicitação e a converte em comportamento verificável. O mapa de releases e a
distribuição por persona estão em `01-mapa-de-historias.md`; aqui está o detalhe executável.

**Formato adotado.** Cada história traz: (a) a sentença Como/quero/para, fixada no registro
canônico; (b) uma ficha de rastreabilidade — persona, release, prioridade MoSCoW, requisitos
funcionais cobertos, regras de negócio aplicáveis e a pendência que ainda pode alterar o
comportamento; (c) o contexto real de uso, com nomes, horários e valores concretos; (d) os
critérios de aceitação; (e) notas de implementação e de interface que não cabem no critério.

**Por que Gherkin.** Os critérios são escritos em Dado / Quando / Então porque cada cenário deve
virar teste automatizado **sem tradução intermediária**: o mesmo texto lido pelo organizador na
homologação é o passo executado na integração contínua. Isso elimina a distância entre "critério
aceito em reunião" e "teste que roda", e torna a regressão de política — o risco central deste
sistema — detectável a cada versão. Cenários que dependem de tempo (expiração de reserva, prazo de
convite, janela de check-in) são executados com relógio controlado, não com espera real.

**Prioridade.** A prioridade MoSCoW da história é a do requisito funcional dominante — aquele que
dá nome à história — conforme o registro canônico. Release e prioridade são dimensões distintas:
uma história `Deveria ter` dentro do MVP é a primeira candidata a corte se o prazo apertar, e não
uma história de R2.

**Pendência.** A coluna aponta o identificador de `analise/duvidas-e-lacunas.md` (AMB, INC ou LAC)
cuja homologação pode mudar o comportamento especificado. `-` significa que a história não depende
de decisão em aberto.

**Números.** Todo valor de política citado nos cenários (30 minutos, 48 horas, 24 horas, 6 horas,
75 %, 7 dias, 60 segundos) é o default recomendado deste trabalho e está marcado
`⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável` no artefato de análise. A
marcação não se repete a cada cenário para não poluir o texto; ela reaparece apenas onde este
documento propõe um número **novo**, ainda inexistente no registro canônico.

---

## Definição de Pronto (Definition of Done)

Vale para as 24 histórias. O que está aqui não se repete como critério de aceitação de nenhuma
delas; uma história só é considerada entregue quando os oito itens são verdadeiros.

| # | Item | Verificação |
|---|---|---|
| 1 | Todos os cenários `CA-nn.n` da história estão automatizados e verdes na integração contínua, inclusive o cenário de exceção, negação, prazo ou concorrência. | Execução da suíte no pipeline, com relógio controlado nos cenários temporais. |
| 2 | Toda transição de estado da inscrição gerada pela história grava registro na trilha imutável (RF-34) com autor, papel, motivo, valores anterior e posterior e identificador de correlação. | Teste que reconstitui a linha do tempo após executar o fluxo. |
| 3 | Toda transição relevante ao participante dispara notificação por e-mail com espelho na central in-app (RF-27), e a situação de entrega fica consultável. | Teste de integração com o serviço de mensagens em modo simulado. |
| 4 | Nenhuma avaliação de cancelamento, reembolso ou certificado lê a política vigente do evento: lê a cópia congelada na inscrição (RF-20, RN-14). | Teste que altera o parâmetro do evento após a confirmação e verifica ausência de efeito retroativo. |
| 5 | O fluxo é operável apenas por teclado e atende WCAG 2.2 nível AA (RNF-20); documentos gerados atendem PDF/UA com texto selecionável (RNF-21). | Auditoria automatizada de acessibilidade mais roteiro manual de teclado e leitor de tela. |
| 6 | A resposta da interface e da API devolve apenas os campos necessários ao papel solicitante (RN-15, RNF-17), e todo acesso de terceiro a dado pessoal é registrado. | Teste de contrato que falha se qualquer campo de contato vazar para papel sem consentimento vigente. |
| 7 | Toda recusa exibida ao usuário informa motivo, data-limite ou prazo aplicável e ação alternativa (RNF-22), em português do Brasil, com fuso America/Sao_Paulo explícito e valores em reais (RNF-23). | Revisão dos textos de erro no roteiro de aceitação. |
| 8 | Nenhum valor de política está fixo em código: todos vêm do Perfil de Política ou da configuração administrativa (RNF-24), com efeito em até 1 minuto e sem indisponibilidade. | Teste que altera o parâmetro por interface e observa o novo comportamento sem reimplantação. |

---

## Histórias

### HU-01 — Política à vista antes de decidir

> **Como** participante, **quero** ler a política de cancelamento, reembolso, fila e certificado antes de iniciar a inscrição, **para** escolher sabendo o que perco se desistir.

| Campo | Valor |
|---|---|
| Persona | Marina Alves (participante) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-07, RF-06 |
| Regras | RN-01, RN-09, RN-26 |
| Pendência | AMB-02, INC-02 |

**Contexto.** Marina compara, às 22h de uma terça, duas atividades do Congresso Eventus de
Tecnologia 2026: o Workshop de Engenharia de Prompt (R$ 180,00) e a Oficina de Observabilidade em
Produção (R$ 180,00). Uma delas é marcada como não cancelável, e hoje essa informação só aparece
no e-mail de confirmação — depois do pagamento. Ela precisa da regra antes do primeiro passo do
fluxo, não depois.

**Critérios de aceitação**

```gherkin
Cenário: CA-01.1 — Política resumida a um clique do botão de inscrição
  Dado que o Congresso Eventus de Tecnologia 2026 está publicado com o Perfil de Política completo
  Quando Marina abre a página do evento sem estar autenticada
  Então o sistema exibe o bloco "Antes de se inscrever" com a janela de cancelamento de 48 horas,
    a faixa de reembolso de 100 %, 50 % e 0 %, o modo da lista de espera e o critério de certificado
  E o bloco está acessível a no máximo 1 clique do botão "Inscrever-se"
  E cada regra indica a atividade a que se aplica quando a atividade sobrescreve o evento

Cenário: CA-01.2 — Item não cancelável avisado antes e repetido na revisão
  Dado que a Oficina de Observabilidade em Produção tem janela de cancelamento igual a zero
  Quando Marina abre o detalhe da atividade
  Então o sistema exibe em destaque "Esta atividade não permite cancelamento após a confirmação"
  E repete o mesmo aviso na tela de revisão, antes de iniciar o pagamento
  E informa o canal de contato para situações excepcionais

Cenário: CA-01.3 — Evento corporativo fechado fora do catálogo público
  Dado que o Encontro Corporativo Nexa está publicado como evento fechado
  Quando um visitante não autenticado busca por "Nexa" no catálogo
  Então o sistema não retorna nenhum resultado
  E ao acessar o endereço direto sem convite nem vínculo organizacional, responde com recusa
    e canal de contato, sem revelar datas, capacidade ou disponibilidade de vagas

Cenário: CA-01.4 — Rótulo de disponibilidade derivado da ocupação corrente
  Dado que o Workshop de Engenharia de Prompt tem capacidade 40 e 38 vagas já ocupadas
  Quando Marina consulta o catálogo
  Então o sistema exibe o rótulo "Últimas vagas", por restarem 5 % da capacidade
  E exibe o instante da última atualização do número, defasado em no máximo 5 segundos
```

**Notas de implementação/UX.** O bloco de política é gerado a partir dos oito parâmetros, nunca de
texto livre digitado pelo organizador — texto livre é onde nasce a divergência entre o prometido e
o executado. Onde a atividade sobrescreve o evento, mostrar a regra da atividade com a marca de
sobrescrita, e não as duas em paralelo. O rótulo de disponibilidade é derivado (RN-26) e admite
cache curto; o número absoluto de vagas não é exibido ao público, para não expor a operação.

---

### HU-02 — Grade do dia inteiro em uma única operação

> **Como** participante, **quero** selecionar o evento e várias atividades e concluir tudo em um só pagamento, **para** não repetir o fluxo a cada workshop.

| Campo | Valor |
|---|---|
| Persona | Marina Alves (participante) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-08, RF-09 |
| Regras | RN-01, RN-02, RN-08, RN-10 |
| Pendência | AMB-06 |

**Contexto.** Marina monta a grade dos dias 12 e 13 de maio: inscrição no congresso (R$ 480,00),
Oficina de Modelagem de Dados (R$ 180,00) e Workshop de Engenharia de Prompt (R$ 180,00). No
processo atual, cada item é um formulário e um pagamento separado; três boletos para uma pessoa
só, e nenhuma garantia de que o terceiro ainda terá vaga quando o segundo terminar.

**Critérios de aceitação**

```gherkin
Cenário: CA-02.1 — Seleção múltipla concluída em um único pagamento
  Dado que Marina selecionou a inscrição no congresso e duas atividades, somando R$ 840,00
  Quando ela submete a solicitação
  Então o sistema cria uma única solicitação com protocolo, contendo os três itens discriminados
  E gera uma única cobrança de R$ 840,00
  E cria uma reserva de vaga independente para cada item, no evento e em cada atividade

Cenário: CA-02.2 — Item que esgota durante a seleção não desfaz os demais
  Dado que Marina selecionou três itens e que a Oficina de Modelagem de Dados esgotou
    entre a seleção e a submissão
  Quando ela submete a solicitação
  Então o sistema mantém os dois itens disponíveis na mesma solicitação
  E recalcula o valor devido para R$ 660,00 antes de qualquer cobrança
  E oferece a entrada na lista de espera do item indisponível como ação separada
  E informa qual item saiu e por quê, sem exigir refazer a seleção

Cenário: CA-02.3 — Envios concorrentes da mesma solicitação produzem efeito único
  Dado que Marina submete a solicitação e a rede repete o envio três vezes com a mesma
    chave de idempotência
  Quando o sistema processa os três envios
  Então apenas uma solicitação é criada e apenas uma cobrança é gerada
  E os envios repetidos recebem a resposta da solicitação já existente, com o mesmo protocolo
  E nenhuma vaga adicional é consumida

Cenário: CA-02.4 — Atividade recusada sem vaga válida no evento
  Dado que o Congresso Eventus de Tecnologia 2026 está esgotado no nível de evento
  Quando Marina tenta selecionar apenas o Workshop de Engenharia de Prompt, que ainda tem vagas
  Então o sistema recusa a seleção informando que a atividade exige vaga válida no evento
  E oferece a entrada na fila do evento como caminho alternativo
```

**Notas de implementação/UX.** A chave de idempotência é gerada no cliente na abertura da tela de
revisão e viaja com a submissão; retida por 24 h (RNF-07). O recálculo de CA-02.2 acontece antes
da chamada ao prestador de pagamento — cobrar e depois estornar por indisponibilidade é o
antipadrão que esta história existe para evitar. A definição de que a inscrição em atividades é
obrigatória, opcional ou inexistente é por evento (AMB-06) e precisa estar resolvida antes de
codificar a tela de seleção.

---

### HU-03 — Vaga segurada enquanto eu pago

> **Como** participante, **quero** a vaga reservada por 30 minutos com contador regressivo visível durante o pagamento, **para** não perdê-la no meio da transação.

| Campo | Valor |
|---|---|
| Persona | Marina Alves (participante) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-13, RF-12 |
| Regras | RN-07, RN-11, RN-20 |
| Pendência | LAC-06, INC-04, INC-05 |

**Contexto.** No dia da abertura das inscrições do Congresso Eventus de Tecnologia 2026, às 09h00
de 06 de abril, 3.000 pessoas disputam simultaneamente as 40 vagas do Workshop de Engenharia de
Prompt. Marina inicia o pagamento às 21h04 de outro dia, com 12 vagas restantes, e leva 9 minutos
até concluir a autenticação do cartão. Sem reserva, ela pode pagar por uma vaga que já não existe.

**Critérios de aceitação**

```gherkin
Cenário: CA-03.1 — Reserva criada com contador e disponibilidade decrementada na hora
  Dado que o Workshop de Engenharia de Prompt tem 12 vagas disponíveis
  Quando Marina inicia o pagamento às 21h04
  Então o sistema cria a reserva com expiração às 21h34 e estado "Aguardando liquidação
    com reserva ativa"
  E exibe contador regressivo e o instante exato de expiração durante todo o fluxo de pagamento
  E a disponibilidade do item passa a 11 imediatamente, antes de qualquer liquidação

Cenário: CA-03.2 — Reserva vencida devolve a vaga e inutiliza o protocolo
  Dado que a reserva de Marina expira às 21h34 e nenhuma liquidação foi reconhecida
  Quando o relógio atinge 21h34
  Então o sistema transita a inscrição para "Reserva vencida"
  E devolve a vaga ao conjunto disponível em até 60 segundos
  E aciona a lista de espera do item, se houver fila ativa
  E recusa qualquer tentativa de retomar o pagamento pelo mesmo protocolo, orientando nova
    solicitação sujeita à disponibilidade corrente

Cenário: CA-03.3 — Duzentas tentativas pela última vaga geram uma única reserva
  Dado que resta 1 vaga no Workshop de Engenharia de Prompt
  Quando 200 participantes iniciam o pagamento no mesmo segundo
  Então exatamente 1 reserva é criada
  E os 199 restantes recebem "esgotado" com a oferta de entrada na fila
  E a ocupação final permanece menor ou igual à capacidade publicada

Cenário: CA-03.4 — Reserva iniciada perto do início expira no início da atividade
  Dado que o Workshop de Engenharia de Prompt começa às 14h00 e Marina inicia o pagamento às 13h42
  Quando o sistema cria a reserva
  Então a expiração é fixada em 14h00, e não em 14h12
  E o contador exibe o prazo efetivo de 18 minutos
```

**Notas de implementação/UX.** O decremento é atômico e serializado por item (RF-12); a
verificação de invariante ao fim de cada rodada de teste de carga é o critério de RNF-06. O
contador do cliente é apenas visual — a autoridade é o instante de expiração persistido no
servidor, reconsultado a cada retorno de tela. Meios de compensação lenta não cabem em 30 minutos:
enquanto INC-05 não for homologado, boleto não é oferecido nos itens com fila ativa.

---

### HU-04 — Aviso de choque de horário no momento da escolha

> **Como** participante, **quero** ser avisada da sobreposição com uma atividade já inscrita antes de confirmar, **para** decidir conscientemente ou trocar de horário.

| Campo | Valor |
|---|---|
| Persona | Marina Alves (participante) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-22 |
| Regras | RN-01, RN-03, RN-13 |
| Pendência | LAC-07, INC-03, AMB-04 |

**Contexto.** Marina já está confirmada no Workshop de Engenharia de Prompt (13/05, 14h00–18h00,
sala Aurora) e se interessa pela Oficina de Observabilidade em Produção (13/05, 16h00–19h00, sala
Bandeirante). São trilhas paralelas: há duas horas de choque. Ela pode querer assistir à segunda
metade da oficina mesmo assim — desde que saiba que está abrindo mão de duas horas da primeira.

**Critérios de aceitação**

```gherkin
Cenário: CA-04.1 — Alerta com confirmação consciente registrada
  Dado que Marina está confirmada no Workshop de Engenharia de Prompt, das 14h00 às 18h00
  E que nenhuma das duas atividades exige presença para certificado
  Quando ela seleciona a Oficina de Observabilidade em Produção, das 16h00 às 19h00
  Então o sistema exibe alerta nomeando a atividade concorrente e a interseção das 16h00 às 18h00
  E oferece as turmas alternativas sem sobreposição, se existirem
  E só conclui a inscrição após marcação explícita de ciência
  E grava na inscrição o instante da confirmação consciente e a versão do texto apresentado

Cenário: CA-04.2 — Bloqueio quando a presença é exigida para certificado
  Dado que o Workshop de Engenharia de Prompt exige presença para certificado
  Quando Marina seleciona a Oficina de Observabilidade em Produção, sobreposta a ele
  Então o sistema bloqueia a inscrição e mantém o botão de confirmação indisponível
  E informa qual atividade impõe a exigência e por quê
  E oferece as turmas alternativas e a opção de cancelar a atividade conflitante, quando a
    política de cancelamento permitir

Cenário: CA-04.3 — Fim exclusivo não caracteriza conflito
  Dado que Marina está confirmada em atividade das 14h00 às 16h00
  Quando ela seleciona atividade das 16h00 às 18h00 no mesmo dia
  Então o sistema conclui a inscrição sem alerta de sobreposição
  E a agenda pessoal exibe as duas atividades em sequência
```

**Notas de implementação/UX.** Intervalo com início inclusivo e fim exclusivo (RN-13) — a regra
precisa estar em uma única função, usada pela agenda, pelo fluxo de inscrição e pela verificação
de impacto de HU-16. O alerta nomeia a atividade concorrente: "choque de horário" genérico faz o
participante abandonar o fluxo sem entender o que precisa decidir. A confirmação consciente é
prova em contestação futura, por isso guarda a versão do texto, não apenas o booleano.

---

### HU-05 — Fila com posição visível em vez de porta fechada

> **Como** participante, **quero** entrar na lista de espera do item esgotado e ver minha posição e as regras do convite, **para** decidir se vale esperar.

| Campo | Valor |
|---|---|
| Persona | Marina Alves (participante) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-14 |
| Regras | RN-10, RN-26, RN-27 |
| Pendência | LAC-03 |

**Contexto.** O Workshop de Acessibilidade Digital esgotou 40 minutos após a abertura. Hoje a
Eventus responde a esses casos por e-mail, manualmente, e ninguém sabe quantas pessoas estão na
frente. Marina precisa decidir se compra passagem para o dia 14 ou não — e essa decisão depende de
saber que é a sétima da fila, não a septuagésima.

**Critérios de aceitação**

```gherkin
Cenário: CA-05.1 — Entrada na fila em uma ação, com posição e regras devolvidas
  Dado que o Workshop de Acessibilidade Digital está esgotado e a política habilita a fila
  Quando Marina aciona "Entrar na lista de espera"
  Então o sistema registra a entrada com carimbo de tempo com precisão de segundo
  E devolve a posição 7 e o total de 6 pessoas à frente
  E informa que o convite terá prazo de 24 horas, limitado a 6 horas antes do início da atividade
  E informa que a posição não garante vaga

Cenário: CA-05.2 — Segunda posição na mesma fila é recusada
  Dado que Marina já ocupa a posição 7 na fila do Workshop de Acessibilidade Digital
  Quando ela tenta entrar novamente na mesma fila, ou entrar na fila de item em que já
    possui inscrição ativa
  Então o sistema recusa a operação informando a posição vigente ou a inscrição existente
  E nenhuma posição adicional é criada
  E a fila mantém o mesmo tamanho

Cenário: CA-05.3 — Fila de item não cancelável avisa que quase não anda
  Dado que a atividade tem janela de cancelamento igual a zero
  Quando Marina consulta sua posição na fila
  Então o sistema informa que a fila só avança por expiração de reserva, ampliação de capacidade
    ou cancelamento administrativo
  E exibe a ação "Sair da fila" na mesma tela, concluída em uma única confirmação
```

**Notas de implementação/UX.** A posição é derivada da ordem cronológica (RN-27) e recalculada a
cada saída, promoção ou expiração — não é campo persistido, para não divergir. Não exibir previsão
de chamada: sem base estatística, previsão vira promessa. Fila existe nos dois níveis, evento e
atividade, com contagem independente (AMB-06).

---

### HU-06 — Convite de vaga com prazo explícito

> **Como** participante convidada pela fila, **quero** receber o convite com a vaga reservada em meu nome e o instante-limite de aceite, **para** não disputar a vaga outra vez.

| Campo | Valor |
|---|---|
| Persona | Marina Alves (participante) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-15 |
| Regras | RN-12, RN-21, RN-27, RN-29 |
| Pendência | LAC-03, LAC-05 |

**Contexto.** Às 11h20 de 08 de maio, alguém cancela a inscrição no Workshop de Acessibilidade
Digital. Marina é a primeira da fila. Se o convite apenas avisar "abriu vaga", ela volta à mesma
corrida que perdeu — e a fila deixa de ter função. O convite precisa vir com a vaga já retirada do
conjunto público e com hora-limite escrita.

**Critérios de aceitação**

```gherkin
Cenário: CA-06.1 — Convite emitido com vaga exclusiva e instante-limite
  Dado que uma vaga é liberada às 11h20 de 08/05 e Marina é a primeira elegível da fila
  Quando o sistema processa a liberação
  Então o convite é emitido em até 2 minutos
  E o instante-limite é 11h20 de 09/05, por ser anterior ao corte de 6 horas antes do início
  E a vaga fica reservada com exclusividade e não retorna ao conjunto público durante o prazo
  E o convite informa o valor devido, se houver, e o que acontece se o prazo vencer

Cenário: CA-06.2 — Convite vencido promove o próximo em cascata
  Dado que o convite de Marina vence às 11h20 de 09/05 sem aceite nem recusa
  Quando o prazo se esgota
  Então a inscrição transita para "Convite encerrado sem aceite" e Marina deixa a fila
  E o próximo elegível recebe convite em até 2 minutos
  E em nenhum instante da transição a vaga é ofertada ao conjunto público
  E a cascata prossegue até o aceite, o esgotamento da fila ou o corte de 6 horas antes do início

Cenário: CA-06.3 — Nenhum convite abaixo do corte de 6 horas
  Dado que a atividade começa às 09h00 de 14/05 e uma vaga é liberada às 05h00 do mesmo dia
  Quando o sistema processa a liberação
  Então nenhum convite é gerado
  E a vaga é devolvida ao conjunto disponível público
  E os enfileirados são informados de que a fila foi encerrada por proximidade do início

Cenário: CA-06.4 — E-mail do convite não entregue suspende o prazo e passa a vez
  Dado que o e-mail do convite falhou nas três retentativas automáticas
  Quando a falha definitiva é registrada
  Então o convite é suspenso sem consumir o prazo do convidado
  E a posição do convidado é preservada para a próxima liberação
  E o próximo elegível é convidado
  E o participante é alertado na central in-app sobre a falha de entrega e a necessidade de
    corrigir o endereço
```

**Notas de implementação/UX.** O prazo é `min(emissão + 24 h; início − 6 h)` (RN-21) e precisa
aparecer como data e hora absolutas no e-mail — contador regressivo em e-mail não é confiável. A
suspensão de CA-06.4 depende do webhook de entrega do provedor; sem sinal de falha, o convite
segue o prazo normal. Aceite e recusa são idempotentes: clicar duas vezes no link não gera duas
inscrições.

---

### HU-07 — Cancelar sozinha sabendo quanto volta e quando

> **Como** participante, **quero** cancelar sem falar com a organização, vendo antes o valor exato a restituir, a faixa aplicada e o prazo do crédito, **para** não depender de resposta por e-mail.

| Campo | Valor |
|---|---|
| Persona | Marina Alves (participante) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-21, RF-18 |
| Regras | RN-09, RN-14, RN-22, RN-29 |
| Pendência | LAC-01, LAC-02, INC-02 |

**Contexto.** Marina pagou R$ 180,00 pelo Workshop de Engenharia de Prompt, marcado para 13/05 às
14h00. Uma viagem de trabalho aparece. Hoje ela escreveria para a organização e esperaria dois
dias por uma resposta que pode ser "não". Ela quer ver o número antes de decidir e cancelar apenas
esse workshop, mantendo o resto da grade.

**Critérios de aceitação**

```gherkin
Cenário: CA-07.1 — Simulação com memória de cálculo antes da confirmação
  Dado que Marina pagou R$ 180,00 e faltam 8 dias para o início da atividade
  Quando ela aciona "Cancelar inscrição"
  Então o sistema exibe, antes de qualquer confirmação, o valor a restituir de R$ 180,00
  E exibe a faixa aplicada de 100 %, por haver 7 dias ou mais de antecedência
  E exibe o meio de devolução, igual ao meio do pagamento, e o prazo estimado do crédito
  E o cancelamento só é efetivado após confirmação explícita nessa tela

Cenário: CA-07.2 — Faixa intermediária aplicada a 3 dias do início
  Dado que Marina pagou R$ 180,00 e faltam 3 dias para o início da atividade
  Quando ela aciona "Cancelar inscrição"
  Então o sistema exibe o valor a restituir de R$ 90,00 com o fator 0,50
  E informa que a partir de 11/05 às 14h00 o cancelamento deixa de ser permitido

Cenário: CA-07.3 — Cancelamento fora da janela é recusado com saída explicada
  Dado que faltam 24 horas para o início da atividade
  Quando Marina aciona "Cancelar inscrição"
  Então o sistema recusa a operação
  E informa o motivo, a data-limite esgotada de 11/05 às 14h00 e o canal alternativo de contato
  E o estado da inscrição permanece "Confirmada", sem qualquer transição registrada

Cenário: CA-07.4 — Cancelamento parcial preserva os demais itens e aciona a fila
  Dado que Marina possui inscrição confirmada no congresso e em duas atividades
  Quando ela cancela apenas o Workshop de Engenharia de Prompt, dentro da janela
  Então as demais inscrições permanecem confirmadas e inalteradas
  E o cálculo usa a cópia da política congelada na inscrição, ainda que o organizador tenha
    alterado o parâmetro depois da confirmação
  E a vaga liberada aciona o convite ao primeiro elegível da fila em até 2 minutos
```

**Notas de implementação/UX.** A memória de cálculo mostra as três parcelas — valor líquido pago,
fator da faixa, estornos anteriores da mesma inscrição — porque a reclamação típica não é sobre a
regra, é sobre a conta. Quando o valor apurado for zero, o texto diz "sem restituição" e explica a
faixa, em vez de exibir R$ 0,00 sem contexto. A abertura do caso de reembolso é assíncrona
(HU-18); o cancelamento não fica pendente da execução do estorno.

---

### HU-08 — Check-in em segundos na porta da sala

> **Como** participante, **quero** registrar presença por código ou QR de uso único na entrada da sessão, **para** garantir a contagem que sustenta meu certificado.

| Campo | Valor |
|---|---|
| Persona | Marina Alves (participante) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-23 |
| Regras | RN-05, RN-19, RN-23 |
| Pendência | LAC-11, LAC-13 |

**Contexto.** Sala Aurora, 13/05, 13h55: 40 pessoas na porta e uma sessão que começa às 14h00. A
lista impressa em prancheta custa três minutos de fila e produz rasuras que ninguém consegue
auditar depois. O credenciamento roda em rede de centro de convenções, que cai.

**Critérios de aceitação**

```gherkin
Cenário: CA-08.1 — Check-in aceito dentro da janela da sessão
  Dado que o Workshop de Engenharia de Prompt começa às 14h00 e Marina está confirmada
  Quando ela apresenta o código de uso único às 13h55
  Então o sistema registra a presença na sessão e transita a inscrição para "Presença registrada"
  E devolve confirmação visível ao operador em menos de 2 segundos
  E o registro passa a contar na apuração do percentual de presença do item

Cenário: CA-08.2 — Fora da janela o registro é recusado com motivo
  Dado que a janela de check-in abre às 13h30 e fecha às 14h30
  Quando um participante apresenta o código às 14h47
  Então o sistema recusa o registro informando o horário-limite já esgotado
  E oferece ao operador a correção manual com justificativa obrigatória
  E a correção manual fica registrada com autor, motivo e instante

Cenário: CA-08.3 — Código já utilizado e inscrição não confirmada não registram presença
  Dado que o código de Marina já registrou presença nesta sessão
  Quando o mesmo código é apresentado outra vez
  Então o sistema informa que a presença já consta e não cria segundo registro
  E códigos vinculados a comprovante de solicitação, sem inscrição confirmada, são recusados
    com a indicação de que a vaga ainda não está garantida

Cenário: CA-08.4 — Operação sem conectividade sincroniza sem duplicar
  Dado que o terminal de credenciamento está sem rede há 3 horas e registrou 128 presenças
    em armazenamento local cifrado
  Quando a conectividade é restabelecida
  Então os 128 registros são sincronizados em até 2 minutos
  E nenhum par de inscrição e sessão gera registro em duplicidade
  E conflitos entre registro local e registro do servidor são resolvidos pelo primeiro instante
    válido, com o descarte anotado na trilha
```

**Notas de implementação/UX.** O código de check-in nasce no comprovante de inscrição confirmada
(RN-05) — é a diferença material entre os dois comprovantes de INC-01. QR lido pela câmera do
navegador, sem aplicativo (RNF-23). O terminal precisa de modo de alto contraste e leitura à
distância: o operador está de pé, sob luz difícil. Presença em atividade remota depende de LAC-11
e não entra no MVP como código.

---

### HU-09 — Certificado com a carga horária que realmente cursei

> **Como** participante, **quero** emitir sozinha o certificado somando apenas as atividades com check-in registrado, **para** comprovar horas complementares sem pedir nada à organização.

| Campo | Valor |
|---|---|
| Persona | Marina Alves (participante) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-24 |
| Regras | RN-19, RN-23, RN-24, RN-25 |
| Pendência | LAC-04, LAC-11 |

**Contexto.** O Congresso Eventus de Tecnologia 2026 encerra em 14/05 às 18h00. Marina precisa do
certificado para horas complementares e a faculdade exige carga horária discriminada. Ela esteve
em 6 das 8 sessões obrigatórias; em uma das tardes, estava inscrita em duas atividades sobrepostas
e assistiu a apenas uma.

**Critérios de aceitação**

```gherkin
Cenário: CA-09.1 — Elegível emite sozinha, com carga horária das sessões frequentadas
  Dado que o item encerrou em 14/05 às 18h00 e o critério da política congelada é presença mínima
  E que Marina tem check-in em 6 das 8 sessões obrigatórias, correspondendo a 75 %
  Quando ela aciona "Emitir certificado" em 15/05
  Então o sistema emite o certificado sem qualquer solicitação à organização
  E declara a carga horária somando apenas as atividades com check-in confirmado
  E a emissão está disponível desde, no máximo, 48 horas após o encerramento

Cenário: CA-09.2 — Inelegível recebe o critério não atendido e a via de revisão
  Dado que outro participante tem check-in em 5 das 8 sessões obrigatórias, correspondendo a 62 %
  Quando ele aciona "Emitir certificado"
  Então o sistema recusa a emissão informando o percentual apurado e o limiar de 75 %
  E lista as sessões sem registro de presença
  E oferece pedido de revisão de presença dentro de 7 dias corridos após o encerramento
    ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável
  E o pedido em curso suspende a conclusão da apuração para aquele participante

Cenário: CA-09.3 — Emissão antes do encerramento é indisponível, com prazo informado
  Dado que o item ainda não encerrou
  Quando Marina acessa a área de certificados
  Então a ação de emissão aparece indisponível
  E o sistema informa o instante de encerramento e o prazo-limite de liberação, 48 horas depois

Cenário: CA-09.4 — Atividade sobreposta sem presença não entra na carga horária
  Dado que Marina estava inscrita no Workshop de Engenharia de Prompt, das 14h00 às 18h00,
    e na Oficina de Observabilidade em Produção, das 16h00 às 19h00
  E que há check-in apenas no workshop
  Quando o certificado é emitido
  Então a carga horária daquele dia declara 4 horas
  E a atividade sobreposta sem registro de presença é desconsiderada, sem aparecer no documento
```

**Notas de implementação/UX.** O percentual é arredondado para baixo (RN-23): 74,9 % é 74 % e não
atinge o limiar — a fronteira precisa estar no roteiro de teste, porque é onde nasce a contestação.
Eventos de participação única, como o Encontro Corporativo Nexa, usam critério automático e não
passam por apuração de percentual. O certificado é gerado com texto selecionável e marcação PDF/UA;
documento rasterizado é rejeitado na revisão.

---

### HU-10 — Certificado que terceiros conferem sem me acionar

> **Como** participante, **quero** que meu certificado traga código de verificação consultável em página pública, **para** que o RH confirme a autenticidade sem depender de mim.

| Campo | Valor |
|---|---|
| Persona | Marina Alves (participante) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-25 |
| Regras | RN-06, RN-19 |
| Pendência | LAC-09 |

**Contexto.** O RH da empresa de Marina recebe PDFs de certificado por e-mail e não tem como
distinguir um documento legítimo de um editado. Hoje a conferência, quando acontece, é uma ligação
para a Eventus. O código de verificação transfere a conferência para quem precisa dela, sem expor
mais nada sobre a participante.

**Critérios de aceitação**

```gherkin
Cenário: CA-10.1 — Verificação pública confirma o essencial e nada além
  Dado o certificado com código de verificação EVT-2026-7F3K-92QD
  Quando um terceiro consulta o código na página pública, sem autenticação
  Então o sistema confirma titular, atividade, carga horária, data de emissão e situação "Válido"
  E não exibe e-mail, documento, telefone, valor pago nem qualquer outro dado do titular
  E o acesso não exige cadastro, login ou identificação do consultante

Cenário: CA-10.2 — Código inválido responde igual e a enumeração é barrada
  Dado um código inexistente ou adulterado
  Quando alguém o consulta na página pública
  Então o sistema responde "código não encontrado", sem distinguir inexistente de revogado
    por caminho diferente
  E não existe endpoint que liste certificados, titulares ou códigos
  E tentativas sucessivas acima do limite por origem são recusadas e registradas

Cenário: CA-10.3 — Revogação refletida na página sem apagar o código
  Dado que a presença que sustentava o certificado foi corrigida e o certificado, revogado
  Quando o código é consultado depois da revogação
  Então a página exibe a situação "Revogado" com a data da revogação
  E o código permanece consultável, em vez de deixar de existir
  E nenhum motivo com dado pessoal é exposto ao consultante
```

**Notas de implementação/UX.** Código permanente e único (RN-06), com retenção de 10 anos
(RNF-19): a página de verificação sobrevive ao certificado no tempo. Formato não sequencial, para
não permitir dedução do volume de emissões nem varredura. A situação "Revogado" existir na página
é o que dá valor à consulta — um código que simplesmente some não distingue fraude de erro.

---

### HU-11 — Comprovante reenviado e rastreável

> **Como** participante, **quero** reenviar o comprovante e ver se a mensagem foi entregue ou falhou, **para** não ficar na dúvida quando nada chega à caixa de entrada.

| Campo | Valor |
|---|---|
| Persona | Marina Alves (participante) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-27, RF-26 |
| Regras | RN-04, RN-05 |
| Pendência | INC-01, LAC-05 |

**Contexto.** Marina submeteu a inscrição às 21h04 e não recebeu nada até as 22h. Ela não sabe se
o e-mail caiu em spam, se o pagamento falhou ou se a inscrição não existe. Hoje a única saída é
escrever para a organização e esperar. A distinção entre "recebemos seu pedido" e "sua vaga está
garantida" é o que evita esse telefonema.

**Critérios de aceitação**

```gherkin
Cenário: CA-11.1 — Comprovante de solicitação emitido no ato da submissão
  Dado que Marina tem uma solicitação em item oneroso pronta para envio, com pagamento pendente
  Quando ela submete a solicitação às 21h04
  Então o sistema emite imediatamente o comprovante de solicitação com protocolo, itens, valor
    e prazo de pagamento
  E o comprovante declara em destaque que não garante vaga

Cenário: CA-11.2 — Comprovante de inscrição confirmada é o segundo artefato, em outro instante
  Dado que a solicitação submetida às 21h04 aguarda liquidação, com o comprovante de solicitação
    já emitido
  Quando a liquidação é reconhecida às 21h11
  Então o sistema emite o comprovante de inscrição confirmada, artefato distinto, contendo o
    código de check-in
  E a central de notificações mantém os dois documentos disponíveis para download em PDF

Cenário: CA-11.3 — Reenvio autosserviço com situação de entrega por mensagem
  Dado que Marina não localizou o comprovante na caixa de entrada
  Quando ela aciona "Reenviar" na central de notificações
  Então o sistema reenvia a mesma mensagem, sem alterar protocolo nem conteúdo
  E exibe a situação de cada envio entre enviada, entregue, falhou e reenviada, com o instante
    de cada transição

Cenário: CA-11.4 — Falha de entrega tratada sem deixar a participante sem documento
  Dado que o e-mail falhou nas três retentativas automáticas
  Quando a falha definitiva é registrada
  Então a mensagem aparece com situação "falhou" e o motivo reportado pelo provedor
  E o download em PDF permanece disponível na central in-app
  E o sistema orienta a correção do endereço de e-mail
  E a tentativa de desativar o e-mail como canal é recusada, por ser canal oficial
```

**Notas de implementação/UX.** A central nasce com abstração de canal (RF-28) mesmo sem WhatsApp e
SMS no MVP — trocar o canal depois não deve reescrever o disparo de cada transição. Só comunicações
de divulgação admitem recusa (RN-04); transacionais, não. O reenvio é limitado por janela para não
virar vetor de abuso, com o limite informado ao participante.

---

### HU-12 — Perfil de política definido antes de abrir inscrições

> **Como** organizador, **quero** configurar os oito parâmetros da política com o efeito prático de cada escolha à vista, **para** que cancelamento, fila, reserva e certificado tenham regra única e explicável.

| Campo | Valor |
|---|---|
| Persona | Rafael Nunes (organizador) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-19, RF-20 |
| Regras | RN-03, RN-14 |
| Pendência | LAC-01, LAC-02, LAC-03, LAC-04, LAC-05, LAC-06, LAC-07, LAC-08 |

**Contexto.** Rafael coordena três eventos com regras diferentes: o Congresso Eventus de Tecnologia
2026 (pago, cancelável, fila ativa), a Oficina de Observabilidade em Produção como atividade não
cancelável dentro dele e o Encontro Corporativo Nexa (fechado, faturado à empresa contratante,
certificado automático).
Na planilha, essas diferenças viviam em uma coluna de observações que ninguém lia.

**Critérios de aceitação**

```gherkin
Cenário: CA-12.1 — Editor único com default recomendado e efeito prático declarado
  Dado que Rafael abre o Perfil de Política do Congresso Eventus de Tecnologia 2026
  Quando ele percorre os oito parâmetros
  Então cada parâmetro exibe o valor padrão recomendado já preenchido
  E exibe, em uma frase, o efeito prático sobre o participante da opção selecionada
  E toda alteração registra autor, instante e valores anterior e posterior

Cenário: CA-12.2 — Herança pela atividade com sobrescrita sinalizada
  Dado que o evento define janela de cancelamento de 48 horas
  Quando Rafael sobrescreve a janela da Oficina de Observabilidade em Produção para zero
  Então a atividade passa a exibir a marca de parâmetro sobrescrito no editor
  E as demais atividades continuam herdando as 48 horas do evento
  E a página pública da atividade exibe a regra da atividade, e não a do evento

Cenário: CA-12.3 — Alteração posterior não retroage sobre quem já confirmou
  Dado que Marina confirmou a inscrição em 20/04 com faixa de reembolso escalonada
  Quando Rafael altera a política para não reembolsável em 25/04, com justificativa registrada
  Então o cancelamento de Marina continua avaliado pela cópia congelada na inscrição
  E as inscrições submetidas a partir de 25/04 passam a usar a versão vigente
  E a alteração após a abertura fica registrada como exceção, com autor e justificativa
```

**Notas de implementação/UX.** O editor é a materialização de OB1 a OB8 em um único lugar: sem ele,
cada indefinição vira uma decisão informal por evento. A frase de efeito prático precisa ser
gerada do parâmetro, para que a página pública de HU-01 e o editor nunca divirjam. O congelamento
copia os parâmetros para a inscrição no instante da confirmação (RF-20) — referência por ponteiro
para o evento reintroduz a retroatividade que RN-14 proíbe.

---

### HU-13 — Publicação barrada enquanto houver pendência

> **Como** organizador, **quero** que o sistema recuse publicar evento com política incompleta, capacidade ausente ou lote inválido, **para** nunca abrir inscrições com regra indefinida.

| Campo | Valor |
|---|---|
| Persona | Rafael Nunes (organizador) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-05, RF-04 |
| Regras | RN-01, RN-03 |
| Pendência | AMB-02 |

**Contexto.** A abertura das inscrições do congresso está marcada para 06/04 às 09h00 e a equipe
monta a programação até a véspera. Já aconteceu de um workshop ir ao ar sem capacidade definida e
receber 300 inscrições para uma sala de 40. A publicação é a última porta antes de o público
entrar.

**Critérios de aceitação**

```gherkin
Cenário: CA-13.1 — Publicação recusada com a lista do que falta
  Dado que o evento está em rascunho com o parâmetro de lista de espera em branco,
    uma atividade sem capacidade e um lote de preço com vigência vencida
  Quando Rafael aciona "Publicar"
  Então o sistema recusa a publicação
  E lista as três pendências, cada uma com o item afetado e o atalho para corrigi-la
  E o evento permanece em rascunho, invisível no catálogo público

Cenário: CA-13.2 — Publicação concluída torna o evento visível e a política aplicável
  Dado que a verificação de prontidão não aponta pendências
  Quando Rafael aciona "Publicar"
  Então o sistema registra o instante da publicação, o autor e a versão da política vigente
  E o evento passa a aparecer no catálogo público em até 5 segundos
  E o resumo da política fica disponível na página do evento antes do fluxo de inscrição

Cenário: CA-13.3 — Sobreposição de sala ou de palestrante impede a composição
  Dado que a Dra. Helena Prado já conduz atividade das 14h00 às 18h00 na sala Aurora em 13/05
  Quando Rafael a designa para outra atividade das 16h00 às 19h00 no mesmo dia
  Então o sistema recusa a alocação nomeando a atividade conflitante e o intervalo em choque
  E aplica a mesma recusa à tentativa de alocar a sala Aurora em horário sobreposto
```

**Notas de implementação/UX.** A verificação de prontidão é executável também em rascunho, como
lista de tarefas — descobrir as pendências só no clique de publicar é o pior momento possível.
Eventos corporativos fechados são publicados sem entrar na busca pública (AMB-02); a verificação é
a mesma, a visibilidade é que muda.

---

### HU-14 — Painel de ocupação com número confiável e defasagem declarada

> **Como** organizador, **quero** ver capacidade, confirmadas, reservas ativas, convites pendentes e tamanho da fila com o instante da última atualização, **para** agir antes de lotar ou de esvaziar.

| Campo | Valor |
|---|---|
| Persona | Rafael Nunes (organizador) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-29 |
| Regras | RN-07, RN-20 |
| Pendência | AMB-01 |

**Contexto.** Na véspera do congresso, Rafael decide se troca a sala Aurora (40 lugares) pelo
auditório (120). A planilha diz "38 inscritos", número que ignora reservas em curso, convites
pendentes e as cortesias bloqueadas para a imprensa. Ele precisa da decomposição, não do total.

**Critérios de aceitação**

```gherkin
Cenário: CA-14.1 — Ocupação decomposta em vez de total agregado
  Dado que o Workshop de Engenharia de Prompt tem capacidade 40, com 31 confirmadas,
    3 reservas ativas, 2 convites pendentes e 1 vaga bloqueada por cortesia
  Quando Rafael abre o painel do evento
  Então o sistema exibe as cinco parcelas separadamente
  E exibe 3 vagas disponíveis, resultado da capacidade menos as parcelas
  E exibe o tamanho da fila de espera do item
  E exibe o instante da última atualização dos números

Cenário: CA-14.2 — Defasagem declarada e número velho identificado como velho
  Dado que uma confirmação é persistida às 10h02min00s
  Quando Rafael observa o painel
  Então o número reflete a confirmação em até 30 segundos
  E o rótulo público de disponibilidade reflete a mudança em até 5 segundos
  Quando a atualização do painel falha por mais de 30 segundos
  Então o painel exibe "desatualizado desde 10h02" no lugar do número, em vez de manter
    silenciosamente o valor anterior

Cenário: CA-14.3 — Alertas configuráveis disparam antes do problema
  Dado que Rafael configurou alerta em 90 % de lotação e alerta de falha de conciliação
  Quando a ocupação do item atinge 36 de 40
  Então o sistema notifica Rafael com o item, o percentual e o tamanho da fila
  E dispara alerta equivalente para expiração de reservas acima do limiar configurado,
    falha de conciliação financeira e falha de entrega de notificação
```

**Notas de implementação/UX.** "Tempo real" foi substituído por defasagem máxima medida e visível
(AMB-01): 30 s no painel, 5 s no rótulo público. Exibir número sem carimbo de atualização é o que
faz o organizador confiar em dado velho. As parcelas do painel usam exatamente a fórmula de RN-20,
para que painel e regra de vaga nunca discordem.

---

### HU-15 — Sala maior convertida em vagas para quem já espera

> **Como** organizador, **quero** ampliar a capacidade e promover a fila em lote em uma única ação, **para** aproveitar a troca de sala sem convidar pessoa por pessoa.

| Campo | Valor |
|---|---|
| Persona | Rafael Nunes (organizador) |
| Release | R2 |
| Prioridade | Deve ter |
| Requisitos | RF-15, RF-12 |
| Regras | RN-07, RN-21, RN-27, RN-29 |
| Pendência | LAC-03 |

**Contexto.** A quatro dias do congresso, a Oficina de Observabilidade em Produção muda da sala
Bandeirante (25 lugares) para o auditório (60). Há 22 pessoas na fila. Convidar uma a uma, por
e-mail manual, é o que a Eventus faz hoje — e leva dois dias, tempo em que metade da fila já se
comprometeu com outra coisa.

**Critérios de aceitação**

```gherkin
Cenário: CA-15.1 — Ampliação promove a fila em lote respeitando a ordem
  Dado que a capacidade passa de 25 para 60 e a fila tem 22 pessoas
  Quando Rafael confirma a ampliação
  Então o sistema emite 22 convites em até 2 minutos, na ordem cronológica da fila
  E cada convite tem prazo próprio, contado da sua emissão
  E as 22 vagas ficam reservadas com exclusividade aos convidados enquanto os prazos correrem
  E o painel passa a exibir 22 convites pendentes e 13 vagas disponíveis

Cenário: CA-15.2 — Redução abaixo das confirmadas é recusada
  Dado que a atividade tem 31 inscrições confirmadas
  Quando Rafael tenta reduzir a capacidade para 25
  Então o sistema recusa a alteração informando o número de confirmadas
  E indica que a redução só é possível até 31, ou mediante cancelamento administrativo prévio
  E nenhuma inscrição confirmada é afetada

Cenário: CA-15.3 — Promoção fora de ordem exige justificativa registrada
  Dado que Rafael precisa promover a pessoa na posição 9, por ser palestrante convidada
  Quando ele aciona a promoção fora de ordem
  Então o sistema exige justificativa em texto antes de emitir o convite
  E registra autor, justificativa, posição promovida e posições preteridas na trilha imutável
  E as posições restantes são recalculadas
```

**Notas de implementação/UX.** Promoção em lote não pode gerar 22 disparos síncronos de e-mail no
mesmo instante sem controle de vazão; a emissão é enfileirada com prazo contado da emissão
individual. O corte de 6 horas antes do início (RN-21) vale também para o lote: ampliações muito
próximas do início liberam vagas ao público em vez de convidar. Ampliar capacidade e trocar de sala
são ações distintas, mas a interface as apresenta juntas, porque na operação real vêm juntas.

---

### HU-16 — Mudança de programação com impacto explícito

> **Como** organizador, **quero** que alterar horário ou sala notifique inscritos e enfileirados e aponte os conflitos de agenda criados, **para** tratar o problema antes do dia do evento.

| Campo | Valor |
|---|---|
| Persona | Rafael Nunes (organizador) |
| Release | R2 |
| Prioridade | Deve ter |
| Requisitos | RF-05, RF-22 |
| Regras | RN-03, RN-13, RN-30 |
| Pendência | INC-03, AMB-04 |

**Contexto.** A Dra. Helena Prado pede para adiantar a oficina de 16h00 para 14h00 no dia 13/05.
São 31 inscritos, 9 na fila, e a mudança coloca a oficina em choque frontal com o Workshop de
Engenharia de Prompt, no qual 12 dessas pessoas também estão inscritas. Rafael precisa ver esse
número antes de confirmar, não depois.

**Critérios de aceitação**

```gherkin
Cenário: CA-16.1 — Impacto calculado antes de aplicar a mudança
  Dado que Rafael propõe mover a Oficina de Observabilidade em Produção de 16h00 para 14h00
  Quando ele solicita a simulação da alteração
  Então o sistema informa quantos inscritos passariam a ter sobreposição de horário
  E separa os que ficariam apenas em alerta dos que ficariam bloqueados por exigência de
    presença para certificado
  E a alteração só é aplicada após confirmação explícita, com o número à vista

Cenário: CA-16.2 — Mudança aplicada notifica inscritos e enfileirados
  Dado que a alteração de sala ou horário foi confirmada
  Quando o sistema aplica a mudança
  Então notifica inscritos e enfileirados do item em até 5 minutos, indicando o que mudou,
    de qual valor para qual valor
  E destaca a alteração na área do palestrante responsável
  E oferece ao participante em conflito o cancelamento da atividade concorrente dentro da
    política, ainda que a janela já tenha se esgotado

Cenário: CA-16.3 — Cancelamento pela organização restitui integralmente
  Dado que a atividade é cancelada por iniciativa da Eventus a 20 horas do início
  Quando o cancelamento é aplicado
  Então todos os pagamentos são restituídos integralmente, sem dedução e independentemente
    da janela e da faixa configuradas
  E os enfileirados são informados do encerramento da fila
  E a inscrição transita para "Cancelada pela organização"
```

**Notas de implementação/UX.** A simulação de CA-16.1 reutiliza a mesma função de sobreposição de
HU-04. Quando a mudança cria bloqueio para quem já está confirmado, o sistema não desfaz inscrição
automaticamente: oferece a saída ao participante e sinaliza o caso ao organizador. A restituição
integral de RN-30 é independente da janela, e essa exceção precisa estar visível no texto da
notificação, para não parecer erro de cálculo.

---

### HU-17 — Extrato conciliado com fila de exceções

> **Como** analista financeira, **quero** conciliar o extrato importado e tratar órfãos, divergência de valor e liquidação após a expiração da reserva em uma fila com desfecho obrigatório, **para** fechar o dia sem planilha paralela.

| Campo | Valor |
|---|---|
| Persona | Cleide Barros (analista financeira) |
| Release | MVP |
| Prioridade | Deveria ter |
| Requisitos | RF-17, RF-16 |
| Regras | RN-08, RN-11, RN-18 |
| Pendência | INC-05, AMB-05, LAC-10 |

**Contexto.** Cleide fecha o dia 06/04, data da abertura das inscrições do congresso: 412
lançamentos no extrato do prestador. Hoje ela cruza duas planilhas e um relatório em PDF, e as
sobras — pagamento sem inscrição, valor a menor, PIX liquidado 40 minutos depois de a reserva
vencer — viram e-mails soltos que ninguém fecha.

**Critérios de aceitação**

```gherkin
Cenário: CA-17.1 — Conciliação automática separa o casado do excepcional
  Dado o extrato importado com 412 lançamentos do dia 06/04
  Quando Cleide executa a conciliação
  Então o sistema casa automaticamente os lançamentos com identificador de transação
    correspondente e confirma as inscrições pendentes de liquidação
  E encaminha os lançamentos restantes à fila de exceções, classificados por motivo entre
    órfão, divergência de valor e liquidação após a expiração da reserva
  E cada exceção exibe protocolo, participante, item, valor esperado e valor recebido

Cenário: CA-17.2 — Liquidação após a expiração não confirma sozinha
  Dado que a reserva do protocolo venceu às 21h34 e o PIX foi liquidado às 22h14
  Quando a notificação do prestador é recebida
  Então o sistema não confirma a inscrição
  E cria exceção com o motivo "liquidação após a expiração da reserva"
  E oferece dois desfechos: reinstalar a inscrição se houver vaga disponível no instante da
    decisão, ou abrir caso de restituição integral
  E o dia não pode ser encerrado com exceções sem desfecho registrado

Cenário: CA-17.3 — Liquidação fora do prestador registrada com comprovante
  Dado que o Encontro Corporativo Nexa é faturado diretamente à empresa contratante
  Quando Cleide registra a liquidação manualmente com o comprovante anexado
  Então o sistema confirma as inscrições vinculadas ao faturamento
  E grava autor, motivo, valor e o anexo na trilha imutável
  E a confirmação manual aparece distinguida da automática nos relatórios
```

**Notas de implementação/UX.** A tela nunca exibe dado de portador de cartão: apenas bandeira,
quatro últimos dígitos e identificador tokenizado (RN-18). "Desfecho obrigatório" é regra de
fechamento, não sugestão — a exceção sem decisão trava o encerramento do dia e aparece no alerta
de HU-14. Reenvios do mesmo evento do prestador são idempotentes (RNF-07): mil notificações
repetidas produzem uma confirmação.

---

### HU-18 — Reembolso simples automático, reembolso grande com quatro olhos

> **Como** analista financeira, **quero** que casos até o teto sejam aprovados automaticamente e os demais exijam aprovador distinto do solicitante, **para** acelerar o rotineiro sem perder segregação de função.

| Campo | Valor |
|---|---|
| Persona | Cleide Barros (analista financeira) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-18 |
| Regras | RN-16, RN-22, RN-30 |
| Pendência | LAC-02 |

**Contexto.** Dos 84 cancelamentos do congresso, 79 geram restituições de até R$ 500,00 e 5 passam
disso, incluindo um pacote corporativo de R$ 4.560,00. Tratar os 84 com o mesmo rito faz o
rotineiro atrasar; tratar todos automaticamente elimina o controle justamente onde o valor é alto.

**Critérios de aceitação**

```gherkin
Cenário: CA-18.1 — Caso até o teto aprovado automaticamente
  Dado que o caso de reembolso apurou R$ 240,00 e o teto de aprovação automática é R$ 500,00
    ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável
  Quando o caso é aberto pelo cancelamento
  Então o sistema aprova automaticamente e encaminha o estorno pelo mesmo meio do pagamento
  E o participante acompanha as transições do caso com prazo estimado a cada etapa

Cenário: CA-18.2 — Acima do teto, quem solicitou não pode aprovar
  Dado que o caso apurou R$ 4.560,00 e foi registrado por Cleide
  Quando Cleide tenta aprovar o próprio caso
  Então o sistema recusa a aprovação informando a exigência de aprovador distinto
  E registra a tentativa na trilha imutável, com autor e instante
  E o caso permanece aguardando aprovação de usuário com alçada e identidade distinta
  E quem executa o estorno também deve ser distinto de quem aprovou

Cenário: CA-18.3 — Falha do estorno no prestador devolve o caso ao tratamento
  Dado que o estorno aprovado foi recusado pelo prestador por conta de destino inválida
  Quando o sistema recebe a recusa
  Então o caso retorna para "Restituição em apuração" com o motivo reportado
  E o participante é notificado com o motivo em linguagem clara e o novo prazo
  E o valor devido permanece registrado, sem baixa indevida
```

**Notas de implementação/UX.** O teto é parâmetro administrativo (RNF-24), não constante — a
homologação de LAC-02 pode mudá-lo sem nova versão do sistema. A segregação de RN-16 vale para
três papéis distintos na mesma cadeia: solicitou, aprovou, executou. Cancelamento por iniciativa
da organização entra como caso de restituição integral (RN-30) e não passa pela faixa de
antecedência.

---

### HU-19 — Fechamento financeiro por evento

> **Como** analista financeira, **quero** o consolidado de arrecadação, estornos executados e pendentes e receita líquida por evento, **para** prestar contas sem recompor lançamento a lançamento.

| Campo | Valor |
|---|---|
| Persona | Cleide Barros (analista financeira) |
| Release | R2 |
| Prioridade | Deveria ter |
| Requisitos | RF-30 |
| Regras | RN-15, RN-22 |
| Pendência | LAC-10 |

**Contexto.** Encerrado o Congresso Eventus de Tecnologia 2026, Cleide precisa entregar à diretoria
o resultado do evento em três dias. Hoje, isso significa reconstruir a arrecadação a partir do
extrato e conferir estornos um a um, com o risco de contar duas vezes o que foi cobrado e
devolvido.

**Critérios de aceitação**

```gherkin
Cenário: CA-19.1 — Consolidado do evento com estornos separados por situação
  Dado que o Congresso Eventus de Tecnologia 2026 encerrou
  Quando Cleide gera o fechamento financeiro do evento
  Então o sistema apresenta arrecadação bruta, estornos executados, estornos pendentes
    e receita líquida
  E discrimina confirmações automáticas e manuais
  E sinaliza se restam exceções de conciliação sem desfecho, impedindo considerar o
    fechamento definitivo

Cenário: CA-19.2 — Exportação com finalidade declarada e mascaramento por padrão
  Dado que Cleide exporta o fechamento em CSV
  Quando ela declara a finalidade da exportação
  Então o sistema mascara por padrão os dados pessoais não essenciais à finalidade
  E registra autor, papel, finalidade, filtros aplicados, volume de registros e instante

Cenário: CA-19.3 — Exportação sem finalidade ou sem alçada é recusada
  Dado que um usuário sem papel autorizado solicita a exportação
  Quando a solicitação é submetida sem finalidade declarada
  Então o sistema recusa a exportação
  E registra a tentativa na trilha imutável, com autor, filtros pretendidos e instante
```

**Notas de implementação/UX.** O relatório concilia contra a fila de exceções de HU-17: fechamento
que ignora exceção aberta é fechamento errado. Emissão de documento fiscal está fora do MVP
(LAC-10); a exportação nasce conciliável com o sistema contábil, com identificador de transação e
protocolo em cada linha.

---

### HU-20 — Minha programação com alteração em destaque

> **Como** palestrante, **quero** ver minhas atividades com data, horário, sala e capacidade, com as alterações recentes destacadas, **para** não descobrir mudança de sala no dia da oficina.

| Campo | Valor |
|---|---|
| Persona | Dra. Helena Prado (palestrante) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-31, RF-05 |
| Regras | RN-01, RN-15 |
| Pendência | LAC-08, LAC-13 |

**Contexto.** A Dra. Helena conduz duas atividades no congresso e uma no Encontro Corporativo Nexa.
Na edição anterior, a sala mudou na véspera e ela soube por um participante no corredor. Ela
precisa de uma tela que mostre o que mudou, quando e em relação a quê.

**Critérios de aceitação**

```gherkin
Cenário: CA-20.1 — Programação própria com dados operacionais da atividade
  Dado que a Dra. Helena está designada em três atividades
  Quando ela acessa "Minha programação"
  Então o sistema lista apenas as atividades em que está designada
  E exibe data, horário, sala ou canal, capacidade e ocupação de cada uma
  E ordena por data e hora de início

Cenário: CA-20.2 — Alteração recente destacada com o valor anterior e o novo
  Dado que a sala da oficina mudou de Bandeirante para auditório em 09/05 às 17h40
  Quando a Dra. Helena acessa a programação
  Então a atividade aparece com marca de alteração recente
  E o sistema exibe "sala: Bandeirante para auditório" e o instante da mudança
  E a marca permanece até que ela confirme a leitura
  E a mesma alteração foi enviada por e-mail no momento da aplicação

Cenário: CA-20.3 — Acesso fora do escopo de designação é recusado
  Dado que a Dra. Helena não está designada no Workshop de Acessibilidade Digital
  Quando ela tenta acessar a atividade pelo endereço direto ou pela interface de consulta
  Então o sistema recusa o acesso por falta de escopo
  E registra a tentativa com autor, papel, recurso e instante
```

**Notas de implementação/UX.** O escopo do palestrante é o par papel e designação (RF-33): a
consulta filtra na origem, e não na tela. Alterações mostram valor anterior e novo porque "sua
atividade foi alterada" obriga a pessoa a caçar a diferença. Capacidade e ocupação são operacionais
e não contêm dado pessoal; a lista nominal é objeto de HU-21.

---

### HU-21 — Lista de inscritos suficiente para preparar a oficina

> **Como** palestrante, **quero** a lista com nome, organização e situação da inscrição, **para** dimensionar material e dinâmica sem receber dados de contato de que não preciso.

| Campo | Valor |
|---|---|
| Persona | Dra. Helena Prado (palestrante) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-31 |
| Regras | RN-15, RN-17 |
| Pendência | LAC-08 |

**Contexto.** A Dra. Helena prepara a Oficina de Observabilidade em Produção para 31 inscritos e
precisa saber quantos são, de onde vêm e quantos estão confirmados, para imprimir apostilas e
montar os grupos. Hoje ela recebe uma planilha por e-mail com CPF, telefone e restrição alimentar
de cada pessoa — muito além do necessário e fora do que a LGPD autoriza.

**Critérios de aceitação**

```gherkin
Cenário: CA-21.1 — Perfil mínimo entregue por padrão
  Dado que a Dra. Helena está designada na Oficina de Observabilidade em Produção
  Quando ela abre a lista de inscritos
  Então o sistema exibe nome social ou completo, organização e situação da inscrição
  E não exibe e-mail, telefone, documento, dados de pagamento nem necessidades de
    acessibilidade ou alimentares
  E o acesso à lista é registrado com autor, papel, atividade e instante

Cenário: CA-21.2 — Nome social prevalece na exibição
  Dado que um participante informou nome social distinto do nome civil
  Quando a lista é exibida ou exportada
  Então o sistema apresenta o nome social
  E o nome civil não é exibido ao palestrante em nenhuma coluna ou detalhe

Cenário: CA-21.3 — Campo de contato não retorna por nenhum caminho
  Dado que nenhum participante concedeu consentimento de contato
  Quando a Dra. Helena exporta a lista, consulta a interface de programação ou acessa
    a resposta da aplicação diretamente
  Então nenhum campo de contato é retornado em qualquer dos caminhos
  E a exportação exige finalidade declarada e registra autor, filtros e volume
```

**Notas de implementação/UX.** A verificação de CA-21.3 é teste de contrato executado a cada versão
(RNF-17): interface limpa com resposta de aplicação vazando campo é a falha típica. A situação da
inscrição é útil e não é dado sensível — permite à palestrante distinguir 31 confirmados de 31
solicitações pendentes de pagamento.

---

### HU-22 — Perfil agregado da turma e contato só com autorização

> **Como** palestrante, **quero** indicadores agregados do público e o contato apenas de quem consentiu, **para** calibrar o conteúdo respeitando a escolha de cada participante.

| Campo | Valor |
|---|---|
| Persona | Dra. Helena Prado (palestrante) |
| Release | R3 |
| Prioridade | Poderia ter |
| Requisitos | RF-32, RF-03 |
| Regras | RN-15, RN-17 |
| Pendência | LAC-08 |

**Contexto.** Para calibrar o nível da oficina, a Dra. Helena quer saber a distribuição de senioridade
e de área de atuação dos 31 inscritos, e poder enviar material complementar a quem aceitar receber.
O risco é evidente: com turmas pequenas, um recorte de duas pessoas identifica indivíduos.

**Critérios de aceitação**

```gherkin
Cenário: CA-22.1 — Indicadores agregados com supressão de recortes pequenos
  Dado que entre os 31 inscritos há 3 pessoas na faixa de senioridade "estagiário"
  Quando a Dra. Helena abre o perfil agregado da turma
  Então o sistema exibe os totais e a distribuição por recorte
  E suprime todo recorte com menos de 5 pessoas, agrupando-o em "demais"
  E não permite combinar filtros até reduzir o recorte abaixo do limiar

Cenário: CA-22.2 — Contato exibido apenas com consentimento específico vigente
  Dado que 9 dos 31 participantes concederam consentimento de contato para esta finalidade
  Quando a Dra. Helena consulta a lista
  Então o e-mail é exibido apenas para os 9 titulares com consentimento vigente
  E para os demais o campo aparece indisponível, sem indicar recusa individual

Cenário: CA-22.3 — Revogação propagada às visões de terceiros em até 60 segundos
  Dado que um titular revoga o consentimento às 09h12
  Quando a Dra. Helena atualiza a lista
  Então o contato daquele titular deixa de ser exibido em até 60 segundos
  E o titular deixa de constar de novas exportações
  E o palestrante recebe aviso de que as exportações anteriores devem ser descartadas,
    com o instante da revogação
```

**Notas de implementação/UX.** História dependente de LAC-08, ainda em homologação — daí o release
R3. A supressão precisa resistir à combinação de filtros, não apenas ao recorte isolado: dois
filtros de cinco pessoas cada podem cruzar em uma. Consentimento é por finalidade e revogável na
central de privacidade (RF-03), sem contato com a organização.

---

### HU-23 — Histórico de uma inscrição reconstituído em segundos

> **Como** equipe de TI, **quero** reconstituir toda a linha do tempo de uma inscrição a partir de trilha imutável, **para** responder a contestação e a auditoria com evidência, não com memória.

| Campo | Valor |
|---|---|
| Persona | Téo Miranda (equipe de TI) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-34 |
| Regras | RN-17, RN-28 |
| Pendência | LAC-09 |

**Contexto.** Um participante afirma ter pago e não ter recebido a vaga do Workshop de Engenharia
de Prompt. Téo precisa demonstrar, em minutos, que a reserva venceu às 21h34, que a liquidação
chegou às 22h14 e que a exceção foi tratada com restituição — com autor e instante de cada passo,
não com a lembrança de quem estava de plantão.

**Critérios de aceitação**

```gherkin
Cenário: CA-23.1 — Linha do tempo completa devolvida em até 10 segundos
  Dado o protocolo de uma inscrição com 14 transições registradas
  Quando Téo consulta o histórico pelo protocolo
  Então o sistema devolve a sequência completa em até 10 segundos
  E cada entrada traz autor, papel, motivo, valores anterior e posterior e identificador
    de correlação
  E transições automáticas identificam o processo de origem, não um usuário genérico

Cenário: CA-23.2 — Alteração e exclusão de registro não existem para nenhum perfil
  Dado um registro de auditoria dentro do prazo de retenção de 5 anos
  Quando qualquer perfil, inclusive administrativo, tenta alterá-lo ou excluí-lo
  Então nenhuma operação de alteração ou exclusão está disponível na interface nem na aplicação
  E a tentativa é recusada e registrada como novo evento de auditoria

Cenário: CA-23.3 — Divergência de integridade vira incidente, não silêncio
  Dado que a verificação diária de integridade recalcula o encadeamento por resumo criptográfico
  Quando um elo diverge do valor esperado
  Então o sistema abre incidente identificando o intervalo afetado e o instante da divergência
  E notifica a equipe de TI
  E a trilha continua aceitando novas inclusões, sem sobrescrever o trecho divergente
```

**Notas de implementação/UX.** Registro somente de inclusão, encadeado por resumo criptográfico
(RNF-16) — a imutabilidade precisa ser propriedade do armazenamento, não disciplina de equipe. A
consulta é por protocolo, participante e identificador de correlação, porque a contestação chega
por qualquer dos três. Acesso de terceiro a dado pessoal também é evento de auditoria (RF-34), e
não apenas transição de estado.

---

### HU-24 — Credenciamento com acesso que expira sozinho

> **Como** equipe de TI, **quero** conceder o papel de operador de credenciamento limitado à atividade e ao dia, com revogação automática ao fim do evento, **para** que ninguém mantenha acesso a dados de participantes depois do necessário.

| Campo | Valor |
|---|---|
| Persona | Téo Miranda (equipe de TI) |
| Release | MVP |
| Prioridade | Deve ter |
| Requisitos | RF-33 |
| Regras | RN-15, RN-17 |
| Pendência | LAC-13 |

**Contexto.** O congresso usa 14 operadores temporários de credenciamento, contratados para três
dias. Na planilha atual, o acesso é a própria planilha compartilhada, que continua acessível
semanas depois. Téo precisa conceder pouco, por pouco tempo, e não depender de lembrar de revogar.

**Critérios de aceitação**

```gherkin
Cenário: CA-24.1 — Concessão limitada à atividade, ao dia e ao mínimo necessário
  Dado que Téo concede o papel de operador de credenciamento para 13/05, na sala Aurora
  Quando o operador autentica no terminal
  Então ele acessa apenas o registro de presença das atividades daquele dia e local
  E visualiza apenas nome, situação da inscrição e validade do código apresentado
  E não acessa e-mail, documento, valor pago nem qualquer outra atividade do evento

Cenário: CA-24.2 — Acesso encerrado automaticamente ao fim do prazo
  Dado que a concessão vale até o fim de 13/05
  Quando o relógio ultrapassa o fim do prazo com sessão ainda aberta
  Então a sessão perde o acesso na primeira operação subsequente, sem depender de ação manual
  E a tentativa de novo registro de presença é recusada com o motivo "concessão encerrada"
  E o encerramento fica registrado na trilha, com o prazo original

Cenário: CA-24.3 — Revogação imediata antes do prazo
  Dado que um operador é desligado no meio do dia 13/05
  Quando Téo revoga a concessão às 15h20
  Então o acesso é encerrado em até 60 segundos, inclusive em sessões já abertas
  E os registros de presença já efetuados por ele permanecem válidos e atribuídos ao seu autor
  E a revogação registra autor, motivo e instante
```

**Notas de implementação/UX.** Toda autorização é avaliada pelo par papel e escopo (RF-33); prazo
é atributo da concessão, não lembrete de calendário. O terminal de credenciamento opera em modo
degradado (HU-08) — a expiração precisa ser verificada na sincronização, e não apenas na
autenticação, para que um terminal offline não vire acesso perpétuo.

---

## Histórias descartadas e por quê

Ideias avaliadas durante a especificação e deliberadamente não convertidas em história. Cada
descarte tem motivo e, quando cabe, a condição que o reabriria.

| Ideia avaliada | Por que foi descartada | Condição de reabertura |
|---|---|---|
| Convite da lista de espera por WhatsApp, com aceite em um toque | O canal está fora do MVP (RF-28, "Não terá agora") e o convite tem valor jurídico de reserva exclusiva: sem comprovante de entrega equivalente ao do e-mail, o prazo de RN-21 fica indefensável em contestação. Adotar o canal antes de resolver a entrega comprovada trocaria um problema de conveniência por um de prova. | Homologação de LAC-05 com definição de prestador e registro de entrega por mensagem. |
| Boleto com reserva de vaga estendida por 3 dias | Incompatível com RN-07 e RN-11: reter vaga por 3 dias em item disputado converte a fila em espera improdutiva e permite bloqueio deliberado de disponibilidade sem custo. A recomendação de INC-05 é não oferecer o meio em item com fila ativa ou gerar inscrição pendente que não consome vaga. | Decisão de Cleide Barros sobre INC-05, com regra explícita de não consumo de vaga. |
| Emissão de nota fiscal eletrônica junto ao comprovante de confirmação | Fora do escopo do MVP por LAC-10, e depende de integração fiscal, regime tributário e município — variáveis que a elicitação não tocou. Entregar isso mal é pior do que não entregar: erro fiscal não se corrige por nova versão do sistema. | Definição fiscal completa e escolha do prestador de emissão. |
| Sala virtual integrada e presença apurada por tempo de conexão | A elicitação não informa se existem atividades on-line ou híbridas (LAC-11), e o critério de certificado depende dessa definição. Especificar presença remota agora seria projetar sobre premissa não confirmada, com efeito direto sobre RN-23 e RN-24. | Homologação de LAC-11 definindo as modalidades suportadas. |
| Prioridade paga ou compra de posição na lista de espera | Sem valor comprovado e em conflito direto com RN-27, que deriva a posição exclusivamente da ordem cronológica. Introduziria arbitragem em um mecanismo cuja legitimidade depende de ser previsível, e a promoção fora de ordem já cobre a exceção legítima, com justificativa auditada (CA-15.3). | Nenhuma prevista; a exceção legítima já está atendida. |
| Rede de contatos entre inscritos, com perfil visível e mensagens internas | Não tem origem em nenhum item da elicitação e amplia a superfície de dados pessoais que RN-15 e RNF-17 trabalham para reduzir. Um sistema que só expõe o mínimo necessário não pode, no mesmo release, publicar perfis entre participantes. | Demanda explícita de stakeholder com avaliação de impacto à proteção de dados. |
| Aplicativo nativo dedicado ao check-in | O requisito real é operar sem conectividade e ler QR (RF-23, RNF-12, RNF-23), e isso é atendido pelo navegador com armazenamento local cifrado. Um aplicativo adicionaria distribuição, atualização e suporte a 14 operadores temporários por evento, sem resolver nada que já não esteja resolvido. | Evidência de falha da leitura por navegador em campo, medida em evento real. |
