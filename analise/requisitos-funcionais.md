# Requisitos Funcionais — Eventus SGE

Cobre os 34 requisitos funcionais do produto que substitui os formulários on-line e as planilhas da Eventus, distribuídos em 11 módulos: da descoberta do evento até o certificado verificável por terceiros. Ficam de fora deste arquivo as metas numéricas de qualidade (`requisitos-nao-funcionais.md`), os enunciados de regra (`regras-de-negocio.md`) e as questões em aberto detalhadas (`duvidas-e-lacunas.md`); aqui está **o que o sistema faz**, não sob que regra.

**Legenda dos campos de cada bloco.** *Módulo* — agrupamento funcional M1–M11. *Prioridade* — MoSCoW conforme a tabela abaixo. *Origem* — códigos do documento de elicitação: falas de stakeholder (P1–P5, O1–O5, F1–F3, L1) transcritas literalmente entre aspas; observações de indefinição (OB1–OB9), que não têm fala associada; `Derivado` quando o requisito é consequência técnica ou legal, não pedido explícito. *Depende de* — RFs que precisam existir antes, sob pena de o requisito não ser testável. *Regras aplicáveis* — RN do registro canônico que incide diretamente sobre o requisito; `-` significa que o registro não vincula nenhuma regra a ele. *Pendência vinculada* — ambiguidade (AMB), inconsistência (INC) ou lacuna (LAC) cuja homologação altera o comportamento especificado.

O símbolo ⚠️ marca todo valor numérico ou limiar proposto por este trabalho: **DECISÃO PROPOSTA — requer homologação do stakeholder responsável**.

## Prioridade MoSCoW — significado adotado no projeto

| Prioridade | Significado operacional na Eventus | Consequência de não entregar |
|---|---|---|
| **Deve ter** | Sem ele a primeira abertura de inscrições com dados reais (MVP) não pode acontecer. | A data de abertura é adiada. |
| **Deveria ter** | Existe contorno manual conhecido, aceito por prazo determinado e com responsável nomeado. | O contorno vira dívida operacional; entra na R2. |
| **Poderia ter** | Agrega valor mensurável, mas é o primeiro a sair sob pressão de prazo e não exige contorno. | Nada quebra; o benefício é adiado. |
| **Não terá agora** | Decisão explícita de exclusão do MVP, com efeito arquitetural registrado hoje para não custar retrabalho depois. | Nenhuma, desde que o ponto de extensão exista. |

## Visão geral dos módulos

| Módulo | RFs | Qtd. | Objetivo em uma linha |
|---|---|---|---|
| M1 Contas e Perfis | RF-01 a RF-03 | 3 | Estabelecer identidade verificada e dar ao titular controle sobre os próprios dados antes de qualquer inscrição. |
| M2 Catálogo e Programação | RF-04 a RF-07 | 4 | Compor, verificar, publicar e expor evento e atividades com a política aplicável à vista do participante. |
| M3 Inscrições | RF-08 a RF-11 | 4 | Registrar a intenção do participante em dois níveis e conduzi-la até um estado vigente único e rastreável. |
| M4 Vagas e Lista de Espera | RF-12 a RF-15 | 4 | Impedir sobrevenda sob concorrência e converter vaga liberada em convite com prazo e ordem auditável. |
| M5 Pagamentos e Reembolsos | RF-16 a RF-18 | 3 | Ligar liquidação financeira a estado de inscrição e devolver dinheiro sob alçada, segregação e rastro. |
| M6 Cancelamento e Políticas | RF-19 a RF-21 | 3 | Converter as oito indefinições da elicitação em parâmetros configuráveis, congelados e explicáveis. |
| M7 Presença e Certificados | RF-22 a RF-25 | 4 | Reconciliar agenda concorrente, apurar presença por sessão e emitir certificado conferível por terceiros. |
| M8 Notificações e Comprovantes | RF-26 a RF-28 | 3 | Separar comprovante de pedido de comprovante de vaga e provar o que foi enviado, entregue ou falhou. |
| M9 Painéis e Relatórios | RF-29, RF-30 | 2 | Entregar número com defasagem declarada ao organizador e exportação com finalidade registrada ao financeiro. |
| M10 Área do Palestrante | RF-31, RF-32 | 2 | Dar ao palestrante o suficiente para preparar a atividade, sob minimização de dados pessoais. |
| M11 Administração e Auditoria | RF-33, RF-34 | 2 | Amarrar cada ação a papel, escopo e prazo, e preservar prova imutável do que aconteceu. |

Distribuição por prioridade: **Deve ter 29 · Deveria ter 3 (RF-11, RF-17, RF-30) · Poderia ter 1 (RF-32) · Não terá agora 1 (RF-28)**.

Dois desses 29 têm **prioridade desdobrada por capacidade**, sem criar identificador novo: RF-05 e RF-15 reúnem, cada um, uma capacidade indispensável ao MVP e outra que só se realiza na R2. A ficha de cada um traz a prioridade e o critério de verificação separados por capacidade, além do contorno operacional aceito no intervalo. A contagem acima considera a capacidade de maior prioridade de cada requisito.

---

## M1 — Contas e Perfis

### RF-01 — Conta do participante com verificação de titularidade

| Campo | Valor |
|---|---|
| Módulo | M1 Contas e Perfis |
| Prioridade | Deve ter |
| Origem | Derivado (consequência de tratar dados pessoais e de vincular certificado a titular) · OB9 (ausência de requisitos de segurança e privacidade na elicitação) |
| Depende de | - |
| Regras aplicáveis | - |
| Pendência vinculada | LAC-12 |

**Descrição.** O sistema deve permitir o autocadastro com nome completo, nome social opcional, e-mail e aceite dos termos, mantendo a conta em situação não verificada até a confirmação de titularidade por vínculo de uso único válido por 24 h ⚠️. Conta não verificada deve poder navegar no catálogo, mas o sistema deve recusar a conclusão de inscrição, a entrada em lista de espera e a emissão de certificado até a verificação. O sistema deve usar o nome social, quando informado, em toda exibição a terceiros e no certificado.

**Critério de verificação.** Criar conta com e-mail inédito e tentar concluir inscrição no Workshop de Engenharia de Prompt: a operação é recusada com a razão explícita e a oferta de reenvio do vínculo. Após abrir o vínculo, a mesma inscrição conclui sem novo preenchimento. Reutilizar o vínculo já consumido e usar um vínculo com 24 h e 1 min de idade produzem recusa. Com nome social preenchido, nenhuma tela de terceiro nem o PDF do certificado exibem o nome de registro.

### RF-02 — Autenticação, segundo fator por papel e ciclo de sessão

| Campo | Valor |
|---|---|
| Módulo | M1 Contas e Perfis |
| Prioridade | Deve ter |
| Origem | OB9 (não foram levantados requisitos de segurança, desempenho, disponibilidade, acessibilidade e privacidade) |
| Depende de | RF-01 |
| Regras aplicáveis | - |
| Pendência vinculada | LAC-09 |

**Descrição.** O sistema deve autenticar por e-mail e senha, exigir segundo fator dos papéis com acesso a dados de terceiros ou a dinheiro — organizador, financeiro, palestrante e equipe de TI — e oferecê-lo como opção ao participante. O sistema deve oferecer recuperação de acesso por vínculo de uso único e uma lista das sessões ativas com revogação individual ou total.

**Critério de verificação.** Para cada um dos quatro papéis administrativos, a autenticação sem segundo fator não conclui e não cria sessão. O participante ativa o segundo fator opcional e o fluxo passa a exigi-lo. Cinco falhas em 15 min ⚠️ acionam bloqueio progressivo. Sessão administrativa ociosa por 30 min ⚠️ exige nova autenticação; sessão de participante expira em 12 h ⚠️, com limite absoluto de 24 h ⚠️. Revogar uma sessão em outro dispositivo invalida a próxima requisição daquele dispositivo (metas em RNF-15).

### RF-03 — Central de privacidade do titular

| Campo | Valor |
|---|---|
| Módulo | M1 Contas e Perfis |
| Prioridade | Deve ter |
| Origem | OB8 (não foi definido quais informações dos participantes poderão ser visualizadas pelos palestrantes) · OB9 |
| Depende de | RF-01, RF-02 |
| Regras aplicáveis | RN-04, RN-15 |
| Pendência vinculada | LAC-08, LAC-12 |

**Descrição.** O sistema deve reunir em uma única área do participante todos os consentimentos concedidos, um por finalidade, com data, origem da coleta e situação, permitindo revogação individual de efeito imediato. O sistema deve oferecer exportação dos próprios dados em formato aberto e solicitação de eliminação com protocolo e prazo, informando quais registros permanecem por obrigação legal e sob qual base.

**Critério de verificação.** Marina Alves revoga o consentimento "exibir meu contato ao palestrante da atividade": a consulta da Dra. Helena Prado deixa de retornar o campo em até 60 s ⚠️ e o registro da revogação aparece na trilha (RF-34). A exportação produz JSON e CSV com dicionário de campos. A solicitação de eliminação devolve protocolo com prazo de 15 dias corridos ⚠️ e a resposta lista nominalmente o que foi eliminado e o que foi retido, com a base de retenção.

---

## M2 — Catálogo e Programação

### RF-04 — Composição do evento e da programação em rascunho

| Campo | Valor |
|---|---|
| Módulo | M2 Catálogo e Programação |
| Prioridade | Deve ter |
| Origem | O5 — "Os workshops que acontecem no mesmo horário devem ocorrer simultaneamente." · P5 — "Gostaria de me inscrever em vários workshops que acontecerão no mesmo dia." · L1 — "Gostaria de consultar a lista de participantes inscritos em minhas atividades." · F1 — "Alguns eventos são gratuitos e outros exigem pagamento." · Derivado |
| Depende de | RF-02, RF-33 |
| Regras aplicáveis | RN-01 |
| Pendência vinculada | AMB-04, LAC-11 |

**Descrição.** O sistema deve permitir compor um evento em rascunho com ficha, tipo, período, modalidade, janela de inscrição, capacidade e lotes de preço, e detalhá-lo em atividades com data, hora de início, duração, sala ou canal, palestrante designado, carga horária e marcação de exigência de presença. O sistema deve recusar a gravação de atividade que aloque a mesma sala ou o mesmo palestrante em intervalos sobrepostos, indicando a atividade concorrente. Rascunho não deve aparecer no catálogo público nem aceitar inscrição.

**Critério de verificação.** Compor o "Congresso Eventus de Tecnologia 2026" com duas atividades no Auditório B, 14h00–16h00 e 15h45–17h00: a gravação da segunda é recusada nomeando a primeira e os 15 min de interseção. Designar a Dra. Helena Prado para dois workshops sobrepostos produz a mesma recusa. O evento em rascunho não é retornado pela busca pública nem pela URL direta a visitante não autenticado.

### RF-05 — Publicação verificada e mudanças na programação publicada

| Campo | Valor |
|---|---|
| Módulo | M2 Catálogo e Programação |
| Prioridade | **(a) Verificação de prontidão na publicação — Deve ter (MVP, HU-13)** · **(b) Alteração, adiamento e cancelamento de programação publicada — Deveria ter (R2, HU-16)** |
| Origem | Derivado · O1 — "Precisamos controlar automaticamente o número de vagas." · O3 — "Nem todos os eventos permitem o cancelamento da inscrição." · O5 — "Os workshops que acontecem no mesmo horário devem ocorrer simultaneamente." · F2 — "Em alguns casos o participante tem direito ao reembolso, em outros não." |
| Depende de | RF-04, RF-19, RF-27 |
| Regras aplicáveis | RN-03, RN-30 |
| Pendência vinculada | AMB-02 |

**Por que a prioridade é dividida.** "Deve ter" foi definido neste documento como aquilo sem o que a primeira abertura de inscrições não pode acontecer. Isso descreve a capacidade (a) e não descreve a capacidade (b): abre-se inscrição sem saber tratar mudança de programação já publicada, não se abre inscrição sem porta de qualidade na publicação. Manter as duas sob uma prioridade única tornaria metade do requisito mal classificada e o critério de verificação inaceitável como unidade — daí o desdobramento, mantido sob o identificador canônico RF-05.

**Descrição (a) — MVP.** O sistema deve condicionar a publicação a uma verificação de prontidão que exige política completa, capacidade definida, janela de inscrição válida, toda atividade com horário e sala ou canal e, em evento pago, ao menos um lote de preço vigente.

**Descrição (b) — R2.** Publicado o evento, o sistema deve registrar toda alteração, adiamento ou cancelamento, notificar inscritos e enfileirados e produzir a relação nominal das agendas que passaram a conflitar em razão da mudança. **Contorno aceito no intervalo:** até a entrega de (b), a mudança de programação publicada é operada por cancelamento administrativo com motivo (RF-11) e comunicado manual da organização, contorno declarado e homologado por Rafael Nunes (seção 8 de `01-mapa-de-historias.md`).

**Critério de verificação (a).** Tentar publicar o Congresso com o parâmetro `criterioCertificado` em branco: a publicação é recusada e a mensagem nomeia o parâmetro e a atividade pendente. Repetir com capacidade ausente, com atividade sem horário e com evento pago sem lote vigente: cada tentativa é recusada nomeando a pendência, e o evento permanece em rascunho.

**Critério de verificação (b).** Com o evento já publicado, mover o Workshop de Engenharia de Prompt de 14h00 para 16h00: todos os inscritos e enfileirados recebem notificação (RF-27) e o organizador recebe a lista nominal das inscrições que passaram a se sobrepor a outra atividade. Cancelar a atividade por iniciativa da Eventus abre caso de restituição integral (RF-18) para cada inscrição paga.

### RF-06 — Catálogo público com busca, filtros e rótulo de disponibilidade

| Campo | Valor |
|---|---|
| Módulo | M2 Catálogo e Programação |
| Prioridade | Deve ter |
| Origem | P1 — "Gostaria de visualizar todos os eventos disponíveis em um único lugar." · O1 — "Precisamos controlar automaticamente o número de vagas." · O4 — "Gostaríamos de acompanhar a quantidade de inscritos em tempo real." |
| Depende de | RF-05, RF-12 |
| Regras aplicáveis | RN-20, RN-26 |
| Pendência vinculada | AMB-02, AMB-01 |

**Descrição.** O sistema deve apresentar sem autenticação todos os eventos publicados, com busca textual por título, descrição e palestrante e filtros combináveis por período, tipo, modalidade, cidade, gratuidade e disponibilidade. Cada item deve exibir rótulo de disponibilidade derivado da ocupação corrente e o instante da última atualização desse rótulo.

**Critério de verificação.** Sem sessão, a busca por "prompt" retorna o Workshop de Engenharia de Prompt; a combinação de filtros período + gratuito + com vaga devolve apenas itens que satisfazem os três. O "Encontro Corporativo Nexa", marcado como fechado, não aparece na busca pública nem por URL direta. Ao ocupar a 36ª de 40 vagas — 4 disponíveis, exatamente 10 % da capacidade —, o rótulo passa a "últimas vagas" em até 5 s ⚠️ (RNF-03), conforme a faixa de RN-26; na 35ª ocupação, com 5 disponíveis, o rótulo ainda é "disponível", e essa fronteira 35ª/36ª é caso de borda obrigatório do teste. Esgotado com fila habilitada, o rótulo distingue-se do esgotado sem fila. Resposta com p95 ≤ 1,5 s ⚠️ (RNF-01).

### RF-07 — Página do evento com grade por dia e política vigente em destaque

| Campo | Valor |
|---|---|
| Módulo | M2 Catálogo e Programação |
| Prioridade | Deve ter |
| Origem | P1 — "Gostaria de visualizar todos os eventos disponíveis em um único lugar." · P5 — "Gostaria de me inscrever em vários workshops que acontecerão no mesmo dia." · O5 — "Os workshops que acontecem no mesmo horário devem ocorrer simultaneamente." · O3 — "Nem todos os eventos permitem o cancelamento da inscrição." · OB1, OB2, OB3, OB4 |
| Depende de | RF-04, RF-05, RF-19 |
| Regras aplicáveis | RN-01, RN-09, RN-26 |
| Pendência vinculada | INC-02 |

**Descrição.** O sistema deve exibir a programação em grade horária por dia, marcando visualmente as atividades simultâneas e a disponibilidade de cada uma. Antes do início do fluxo de inscrição, o sistema deve apresentar o resumo objetivo da política aplicável ao item — janela de cancelamento, faixas de reembolso, existência e modo da lista de espera e critério de emissão do certificado — em linguagem afirmativa e sem remissão a documento externo.

**Critério de verificação.** No dia 12 do Congresso, as atividades das 14h00 aparecem na mesma faixa da grade com marcação de simultaneidade. O bloco de política é alcançável sem nenhum clique adicional a partir do botão de inscrição (RNF-22) e exibe os quatro itens. Em atividade marcada como não cancelável, a página informa "cancelamento não permitido após a confirmação" antes do botão, e não apenas na confirmação.

---

## M3 — Inscrições

### RF-08 — Inscrição em dois níveis com seleção múltipla

| Campo | Valor |
|---|---|
| Módulo | M3 Inscrições |
| Prioridade | Deve ter |
| Origem | P5 — "Gostaria de me inscrever em vários workshops que acontecerão no mesmo dia." · O5 — "Os workshops que acontecem no mesmo horário devem ocorrer simultaneamente." |
| Depende de | RF-01, RF-07, RF-12 |
| Regras aplicáveis | RN-02 |
| Pendência vinculada | AMB-06 |

**Descrição.** O sistema deve permitir a inscrição no evento e em atividades específicas, com controle de vaga independente em cada nível, concluindo várias seleções em uma única operação e, havendo cobrança, em um único pagamento. Se um dos itens selecionados estiver indisponível no instante da submissão, o sistema deve concluir os demais, informar o item recusado e oferecer a alternativa cabível (lista de espera ou outro horário), sem desfazer o que já foi confirmado.

**Critério de verificação.** Marina seleciona o Congresso e três workshops; um deles esgota entre a seleção e a submissão. O resultado é: inscrição no evento e nas duas atividades disponíveis concluída, uma única cobrança gerada com a soma correta, mensagem nomeando o workshop indisponível e botão de entrada na fila daquele item. Repetir a submissão não duplica nenhuma das inscrições já concluídas.

### RF-09 — Confirmação imediata do item gratuito e solicitação protocolada do item oneroso

| Campo | Valor |
|---|---|
| Módulo | M3 Inscrições |
| Prioridade | Deve ter |
| Origem | F1 — "Alguns eventos são gratuitos e outros exigem pagamento." · F3 — "Precisamos confirmar os pagamentos antes de liberar determinadas inscrições." · P2 — "Seria interessante receber um comprovante logo após a inscrição." · Derivado |
| Depende de | RF-08, RF-12 |
| Regras aplicáveis | RN-08, RN-10 |
| Pendência vinculada | AMB-05, INC-01 |

**Descrição.** O sistema deve confirmar a inscrição em item gratuito no ato da submissão, sem criar reserva temporária, e criar para item oneroso uma solicitação com protocolo, valor devido, prazo de pagamento e instante de expiração. Toda submissão deve carregar chave de idempotência, de modo que envios concorrentes ou repetidos do mesmo formulário produzam uma única inscrição e a mesma resposta.

**Critério de verificação.** Inscrição gratuita passa a "Confirmada" (E-04) na resposta da submissão, sem consumir reserva. Inscrição onerosa passa a "Aguardando liquidação com reserva ativa" (E-02) e a resposta traz protocolo, valor e instante-limite. Duas requisições simultâneas com a mesma chave de idempotência criam uma única inscrição e retornam corpo idêntico; a chave permanece válida por 24 h ⚠️ (RNF-07). Tentar segunda inscrição ativa na mesma atividade é recusado com o protocolo da inscrição existente.

### RF-10 — Minhas Inscrições com estados, pendências, linha do tempo e retomada

| Campo | Valor |
|---|---|
| Módulo | M3 Inscrições |
| Prioridade | Deve ter |
| Origem | P2 — "Seria interessante receber um comprovante logo após a inscrição." · P3 — "Quando não puder participar, gostaria de cancelar minha inscrição sem precisar entrar em contato com a organização." · P4 — "Quero conseguir emitir meu certificado depois do evento." · OB6 |
| Depende de | RF-09, RF-13, RF-20, RF-34 |
| Regras aplicáveis | RN-02, RN-28 |
| Pendência vinculada | LAC-06 |

**Descrição.** O sistema deve reunir em uma única área as inscrições do participante com o estado vigente de cada uma, as ações permitidas pela política congelada naquela inscrição, as pendências com o tempo restante e o histórico cronológico das transições. O sistema deve permitir retomar uma inscrição interrompida enquanto a reserva estiver válida, levando o participante direto ao pagamento, sem nova disputa de vaga.

**Critério de verificação.** A tela exibe, para cada inscrição, estado, ação disponível e pendência. Numa inscrição com 12 min restantes de reserva, o botão "retomar pagamento" leva ao prestador e a disponibilidade da atividade não é alterada nesse retorno. Expirada a reserva, o mesmo item exibe "Reserva vencida" (E-03), sem botão de retomada, e a linha do tempo lista, na ordem, submissão, reserva criada, reserva vencida e vaga liberada, cada uma com data, hora e origem.

### RF-11 — Gestão de inscrições pelo organizador

| Campo | Valor |
|---|---|
| Módulo | M3 Inscrições |
| Prioridade | Deveria ter |
| Origem | O1 — "Precisamos controlar automaticamente o número de vagas." · O3 — "Nem todos os eventos permitem o cancelamento da inscrição." · O4 — "Gostaríamos de acompanhar a quantidade de inscritos em tempo real." · Derivado |
| Depende de | RF-08, RF-33, RF-34 |
| Regras aplicáveis | RN-28 |
| Pendência vinculada | AMB-03, LAC-13 |

**Descrição.** O sistema deve permitir ao organizador consultar os inscritos de seus eventos com filtros por atividade, estado, lote e data, inscrever terceiro nominalmente, importar inscrições em lote a partir de arquivo e cancelar inscrições individualmente ou em lote mediante motivo obrigatório. Toda ação administrativa sobre inscrição de terceiro deve identificar o autor e o motivo no registro da própria inscrição.

**Critério de verificação.** Importar arquivo com 200 linhas contendo 7 registros inválidos (e-mail malformado, atividade inexistente, duplicidade) resulta em 193 inscrições criadas e relatório de rejeição com linha, campo e motivo de cada falha, sem criar inscrição parcial. Cancelamento em lote de 15 inscrições sem preencher motivo é recusado. Rafael Nunes não consegue listar inscritos de evento sob responsabilidade de outro organizador (RF-33), e a tentativa aparece na trilha.

---

## M4 — Vagas e Lista de Espera

### RF-12 — Controle transacional de vagas em dois níveis sem sobrevenda

| Campo | Valor |
|---|---|
| Módulo | M4 Vagas e Lista de Espera |
| Prioridade | Deve ter |
| Origem | O1 — "Precisamos controlar automaticamente o número de vagas." · O5 — "Os workshops que acontecem no mesmo horário devem ocorrer simultaneamente." · OB6 |
| Depende de | RF-04 |
| Regras aplicáveis | RN-01, RN-07, RN-20 |
| Pendência vinculada | AMB-06, INC-04 |

**Descrição.** O sistema deve decrementar a disponibilidade de forma atômica e serializada por item, tanto na confirmação direta quanto na criação de reserva temporária, computando inscrições confirmadas, reservas ativas, convites pendentes e vagas bloqueadas por cortesia. O sistema deve exigir vaga válida no evento para ocupar vaga de atividade e deve recusar a redução da capacidade abaixo do número de confirmadas.

**Critério de verificação.** Duzentas requisições concorrentes disputando a última vaga do Workshop de Engenharia de Prompt produzem exatamente uma ocupação e 199 recusas explícitas, repetido 50 vezes sem uma única sobrevenda (RNF-06); ao fim de cada rodada, ocupação ≤ capacidade. Inscrever em atividade sem inscrição válida no Congresso é recusado. Reduzir a capacidade de 40 para 30 com 34 confirmadas é recusado informando o piso corrente.

### RF-13 — Reserva temporária de vaga com contador e expiração automática

| Campo | Valor |
|---|---|
| Módulo | M4 Vagas e Lista de Espera |
| Prioridade | Deve ter |
| Origem | OB6 (não está definido se a vaga é reservada ao iniciar o pagamento ou somente após sua confirmação) · F3 — "Precisamos confirmar os pagamentos antes de liberar determinadas inscrições." |
| Depende de | RF-09, RF-12 |
| Regras aplicáveis | RN-11, RN-29 |
| Pendência vinculada | INC-04, INC-05, LAC-06 |

**Descrição.** O sistema deve reservar a vaga no instante em que o participante inicia o pagamento, pelo prazo de 30 min ⚠️, exibindo contador regressivo e o instante exato de expiração em todas as telas do fluxo. Vencido o prazo sem liquidação, o sistema deve devolver a vaga ao conjunto disponível automaticamente e acionar a lista de espera do item, sem intervenção humana.

**Critério de verificação.** Ao iniciar o pagamento de uma inscrição no Congresso, a disponibilidade pública cai em uma unidade imediatamente e a tela mostra contador e horário-limite. Sem liquidação, a vaga retorna ao conjunto disponível em até 60 s ⚠️ do vencimento (RNF-08), nenhuma reserva permanece ativa por mais de 31 min ⚠️ e, havendo fila, o convite é emitido na sequência (RF-15). O protocolo vencido não permite retomada nem novo pagamento e informa a razão.

### RF-14 — Lista de espera autosserviço com posição consultável

| Campo | Valor |
|---|---|
| Módulo | M4 Vagas e Lista de Espera |
| Prioridade | Deve ter |
| Origem | O2 — "Quando um evento lotar, seria interessante criar uma lista de espera." · OB3 |
| Depende de | RF-12, RF-19 |
| Regras aplicáveis | RN-10, RN-27 |
| Pendência vinculada | LAC-03 |

**Descrição.** O sistema deve oferecer entrada e saída da fila em uma única ação sempre que o item estiver esgotado e a política do evento habilitar a lista de espera, devolvendo na resposta a posição obtida, o total de pessoas à frente e as regras do convite (prazo de aceite e corte de antecedência). O sistema deve manter a posição consultável a qualquer momento e atualizá-la a cada saída, promoção ou expiração de convite.

**Critério de verificação.** Em atividade esgotada com fila habilitada, um clique coloca Marina na fila e a resposta traz "posição 12, 11 pessoas à frente" e o texto das regras do convite. Nova tentativa de entrada na mesma fila é recusada; entrada na fila de item em que já existe inscrição ativa é recusada. Quando o 3º da fila sai, a posição de Marina passa a 11 na consulta seguinte. Em item com fila desabilitada, o botão não é exibido e a página informa que não há lista de espera.

### RF-15 — Convite com prazo, promoção em cascata e administração da fila

| Campo | Valor |
|---|---|
| Módulo | M4 Vagas e Lista de Espera |
| Prioridade | **(a) Convite com prazo e promoção em cascata — Deve ter (MVP, HU-06)** · **(b) Ampliação de capacidade com promoção em lote — Deveria ter (R2, HU-15)** |
| Origem | OB3 (não foi informado como funcionará a lista de espera) · O2 — "Quando um evento lotar, seria interessante criar uma lista de espera." · O1 — "Precisamos controlar automaticamente o número de vagas." |
| Depende de | RF-12, RF-14, RF-27 |
| Regras aplicáveis | RN-07, RN-12, RN-21, RN-27, RN-29 |
| Pendência vinculada | LAC-03 |

**Por que a prioridade é dividida.** Sem (a) a fila de RF-14 é uma lista que nunca avança, e a primeira abertura de inscrições não pode acontecer com ela; (b) só se torna necessária quando há troca de sala, hipótese que não bloqueia a abertura. As duas capacidades são verificadas em blocos separados, sob o identificador canônico RF-15.

**Descrição (a) — MVP.** Ao liberar uma vaga em item com fila ativa, o sistema deve emitir convite de aceite ao primeiro elegível, mantendo a vaga reservada com exclusividade até o instante-limite, e promover automaticamente o próximo em caso de recusa ou expiração, em cascata. O sistema deve permitir ao organizador promover fora de ordem mediante justificativa registrada, remover enfileirado e reenviar convite não entregue.

**Descrição (b) — R2.** O sistema deve permitir ao organizador ampliar a capacidade do item e converter, em uma única ação, as posições da fila em convites, na ordem cronológica. **Contorno aceito no intervalo:** até a entrega de (b), a ampliação de capacidade é seguida da promoção individual repetida, com a promoção fora de ordem e a justificativa registrada já disponíveis em (a) — contorno declarado e homologado por Rafael Nunes (seção 8 de `01-mapa-de-historias.md`).

**Critério de verificação (a).** Liberada uma vaga, o convite chega ao primeiro elegível em até 2 min ⚠️ (RNF-08), com instante-limite igual ao menor valor entre 24 h ⚠️ após a emissão e 6 h ⚠️ antes do início. Durante a validade, a vaga não aparece como disponível ao público em nenhuma consulta. Recusado ou vencido o convite, o segundo da fila é convidado sem que a disponibilidade pública volte a subir em momento algum. Promoção fora de ordem sem justificativa é recusada.

**Critério de verificação (b).** Ampliar a capacidade do Workshop de Engenharia de Prompt de 40 para 60 com 20 pessoas na fila emite 20 convites em uma única ação, na ordem das posições, sem cancelar nenhum convite já vigente e sem que a disponibilidade pública suba em momento algum.

---

## M5 — Pagamentos e Reembolsos

### RF-16 — Cobrança da inscrição e confirmação automática na liquidação

| Campo | Valor |
|---|---|
| Módulo | M5 Pagamentos e Reembolsos |
| Prioridade | Deve ter |
| Origem | F1 — "Alguns eventos são gratuitos e outros exigem pagamento." · F3 — "Precisamos confirmar os pagamentos antes de liberar determinadas inscrições." · OB9 |
| Depende de | RF-09, RF-13 |
| Regras aplicáveis | RN-08, RN-18 |
| Pendência vinculada | AMB-05, LAC-10 |

**Descrição.** O sistema deve gerar uma cobrança para toda inscrição com valor devido maior que zero, delegar a captura do meio de pagamento ao prestador e converter a reserva em inscrição confirmada ao reconhecer a liquidação por notificação autenticada. O sistema não deve reter número, validade ou código de segurança de cartão, persistindo apenas o identificador tokenizado, a bandeira, os quatro últimos dígitos e a situação da transação.

**Critério de verificação.** A notificação autenticada do prestador leva a inscrição de E-02 para E-04, dispara o comprovante de confirmação (RF-26) e a notificação da transição (RF-27). Mil reenvios da mesma notificação produzem uma única confirmação e um único comprovante (RNF-07). Notificação com assinatura inválida é rejeitada e registrada. Uma varredura na base, nos registros de aplicação e na cópia de segurança não encontra nenhum número completo de cartão, validade ou código de segurança (RNF-14).

### RF-17 — Confirmação manual de pagamento e conciliação com fila de exceções

| Campo | Valor |
|---|---|
| Módulo | M5 Pagamentos e Reembolsos |
| Prioridade | Deveria ter |
| Origem | F3 — "Precisamos confirmar os pagamentos antes de liberar determinadas inscrições." · F1 — "Alguns eventos são gratuitos e outros exigem pagamento." · Derivado |
| Depende de | RF-16, RF-33 |
| Regras aplicáveis | - |
| Pendência vinculada | INC-05, LAC-10 |

**Descrição.** O sistema deve permitir à equipe financeira registrar liquidação recebida fora do prestador — típica do faturamento de eventos corporativos — com anexo obrigatório de comprovante, e conciliar automaticamente o extrato importado contra as cobranças em aberto. O sistema deve manter uma fila de exceções contendo pagamentos órfãos, divergência de valor e liquidação posterior à expiração da reserva, e não deve permitir encerrar item da fila sem desfecho nomeado e autor identificado.

**Critério de verificação.** Importar extrato do Encontro Corporativo Nexa com 100 lançamentos, sendo 3 sem cobrança correspondente, 2 com valor divergente e 1 recebido após a expiração da reserva: 94 conciliam automaticamente e a fila de exceções abre com 6 itens classificados por tipo. Cada item só sai da fila com desfecho registrado (confirmar, estornar, reprocessar ou arquivar com justificativa). Registro manual sem anexo é recusado. Cleide Barros consegue operar a fila; Rafael Nunes não.

### RF-18 — Caso de reembolso com alçada, segregação e execução do estorno

| Campo | Valor |
|---|---|
| Módulo | M5 Pagamentos e Reembolsos |
| Prioridade | Deve ter |
| Origem | F2 — "Em alguns casos o participante tem direito ao reembolso, em outros não." · OB2 · O3 — "Nem todos os eventos permitem o cancelamento da inscrição." |
| Depende de | RF-16, RF-21, RF-33 |
| Regras aplicáveis | RN-16, RN-22, RN-30 |
| Pendência vinculada | LAC-02 |

**Descrição.** O sistema deve abrir automaticamente um caso de reembolso sempre que o cancelamento gerar valor devolvível, aprovar sem intervenção humana os casos até o teto configurado — R$ 500,00 ⚠️ (decisão de Cleide Barros) — e exigir, acima dele, aprovador distinto do solicitante e executor distinto do aprovador. O sistema deve executar o estorno pelo mesmo meio do pagamento original e expor ao participante cada transição do caso com o prazo estimado de crédito.

**Critério de verificação.** Cancelamento com R$ 120,00 devolvíveis gera caso aprovado automaticamente, sem fila humana. Cancelamento com R$ 900,00 exige aprovação de usuário distinto de quem solicitou; a tentativa de a mesma identidade aprovar e executar é recusada com a razão. O estorno é dirigido ao mesmo meio do pagamento e, quando este não admite estorno, o caso migra para tratamento manual com registro. Marina acompanha as transições do caso e o prazo estimado sem acionar a organização.

---

## M6 — Cancelamento e Políticas

### RF-19 — Editor do Perfil de Política do Evento

| Campo | Valor |
|---|---|
| Módulo | M6 Cancelamento e Políticas |
| Prioridade | Deve ter |
| Origem | OB1, OB2, OB3, OB4, OB5, OB6, OB7, OB8 (as oito indefinições da elicitação, tratadas como parâmetros e não como pendências abertas) |
| Depende de | RF-04 |
| Regras aplicáveis | RN-03 |
| Pendência vinculada | LAC-01, LAC-02, LAC-03, LAC-04, LAC-05, LAC-06, LAC-07, LAC-08 |

**Descrição.** O sistema deve oferecer um editor único com os oito parâmetros do Perfil de Política do Evento — `janelaCancelamento`, `politicaReembolso`, `modoListaEspera`, `criterioCertificado`, `canaisNotificacao`, `reservaDeVaga`, `politicaConflitoHorario` e `visibilidadePalestrante` —, apresentando para cada um o valor padrão recomendado e a frase que descreve o efeito prático sobre o participante. As atividades devem herdar o perfil do evento, admitindo sobrescrita individual sinalizada na interface, e a publicação deve ser recusada com qualquer parâmetro em branco.

**Critério de verificação.** O editor abre com os oito parâmetros preenchidos com os padrões recomendados e cada um acompanhado do efeito prático em uma frase (por exemplo, "o participante poderá cancelar até 48 h antes do início"). Apagar `modoListaEspera` e tentar publicar produz recusa nomeando o parâmetro. Sobrescrever `politicaConflitoHorario` na oficina da Dra. Helena Prado exibe o selo de sobrescrita, o valor herdado e o valor local. Alteração de parâmetro passa a valer em até 1 min ⚠️ sem indisponibilidade (RNF-24) e grava autor e justificativa.

### RF-20 — Congelamento e versionamento da política

| Campo | Valor |
|---|---|
| Módulo | M6 Cancelamento e Políticas |
| Prioridade | Deve ter |
| Origem | OB1, OB2 · Derivado (previsibilidade contratual da relação com o participante) |
| Depende de | RF-09, RF-19 |
| Regras aplicáveis | RN-14 |
| Pendência vinculada | LAC-01, LAC-02 |

**Descrição.** O sistema deve gravar na inscrição, no instante da confirmação, uma cópia imutável dos parâmetros de política vigentes e usar exclusivamente essa cópia em toda avaliação posterior de cancelamento, reembolso, conflito e certificado. O sistema deve permitir alterar a política após a abertura das inscrições apenas por exceção autorizada, com justificativa registrada, e nunca com efeito retroativo.

**Critério de verificação.** Confirmar inscrição sob reembolso escalonado; alterar o parâmetro para "não reembolsável"; cancelar a inscrição anterior: o cálculo aplica a faixa escalonada da cópia congelada, e uma inscrição criada após a mudança aplica a nova regra. A inscrição exibe a versão de política a que está submetida e o comparativo com a vigente. A alteração aparece na trilha (RF-34) com autor, instante, valor anterior, valor posterior e justificativa.

### RF-21 — Cancelamento autosserviço com simulação, cancelamento parcial e explicação

| Campo | Valor |
|---|---|
| Módulo | M6 Cancelamento e Políticas |
| Prioridade | Deve ter |
| Origem | P3 — "Quando não puder participar, gostaria de cancelar minha inscrição sem precisar entrar em contato com a organização." · O3 — "Nem todos os eventos permitem o cancelamento da inscrição." · OB1, OB2 · F2 — "Em alguns casos o participante tem direito ao reembolso, em outros não." |
| Depende de | RF-10, RF-20 |
| Regras aplicáveis | RN-09, RN-14, RN-22, RN-29 |
| Pendência vinculada | INC-02, LAC-01, LAC-02 |

**Descrição.** O sistema deve permitir ao participante cancelar sem contato com a organização enquanto a janela da política congelada permitir, exibindo antes da confirmação o valor exato a restituir, a faixa aplicada, a memória de cálculo e o prazo estimado do crédito. O sistema deve permitir cancelar uma atividade preservando as demais inscrições do mesmo evento e, quando a ação estiver indisponível, deve informar o motivo, a data-limite já esgotada e o canal alternativo de atendimento.

**Critério de verificação.** Para uma inscrição de R$ 400,00: a 8 dias do início a simulação mostra R$ 400,00 (fator 1,00), a 3 dias mostra R$ 200,00 (fator 0,50) e a 24 h mostra R$ 0,00, sempre com a memória de cálculo antes do botão de confirmação. Cancelar um de três workshops mantém os outros dois em E-04 e libera apenas a vaga do item cancelado, acionando a fila daquele item. Em item não cancelável, a ação é recusada com motivo, data-limite e canal, e o estado da inscrição permanece inalterado.

---

## M7 — Presença e Certificados

### RF-22 — Agenda pessoal com detecção de sobreposição e política de conflito

| Campo | Valor |
|---|---|
| Módulo | M7 Presença e Certificados |
| Prioridade | Deve ter |
| Origem | P5 — "Gostaria de me inscrever em vários workshops que acontecerão no mesmo dia." · O5 — "Os workshops que acontecem no mesmo horário devem ocorrer simultaneamente." · OB7, OB4 |
| Depende de | RF-04, RF-08, RF-19 |
| Regras aplicáveis | RN-13 |
| Pendência vinculada | AMB-04, INC-03, LAC-07 |

**Descrição.** O sistema deve montar a agenda pessoal do participante a partir das inscrições ativas e marcar as sobreposições tanto na agenda quanto durante o fluxo de inscrição, antes da confirmação. O sistema deve aplicar o parâmetro de conflito do evento: alertar nomeando a atividade concorrente e exigir confirmação consciente registrada, ou bloquear quando qualquer das atividades envolvidas exigir presença para certificado, oferecendo horários alternativos do mesmo conteúdo quando existirem.

**Critério de verificação.** Com inscrição ativa das 14h00 às 16h00, selecionar atividade das 15h00 às 17h00 dispara alerta que nomeia a atividade já inscrita e o intervalo em conflito; a conclusão só ocorre após aceite explícito, gravado na inscrição com instante e texto apresentado. Se qualquer das duas exigir presença para certificado, a submissão é bloqueada e a tela lista as turmas alternativas do mesmo workshop. Atividades encostadas (16h00–18h00 após 14h00–16h00) não geram alerta, por fim exclusivo.

### RF-23 — Check-in por sessão com código de uso único e modo degradado

| Campo | Valor |
|---|---|
| Módulo | M7 Presença e Certificados |
| Prioridade | Deve ter |
| Origem | OB4 (não foi definido se os certificados são automáticos ou dependem de confirmação de presença) · Derivado |
| Depende de | RF-26, RF-33 |
| Regras aplicáveis | RN-05 |
| Pendência vinculada | LAC-04, LAC-11, LAC-13 |

**Descrição.** O sistema deve registrar presença por atividade mediante código alfanumérico ou QR de uso único vinculado ao par inscrição e sessão, aceitando o registro apenas na janela que abre 30 min ⚠️ antes e fecha 30 min ⚠️ após o horário de início. O sistema deve operar sem conectividade, com armazenamento local cifrado e sincronização posterior sem duplicidade, e deve admitir correção manual pelo organizador mediante justificativa registrada.

**Critério de verificação.** Leitura 45 min antes do início é recusada com a razão; leitura 20 min antes é aceita; leitura 31 min após o início é recusada. A segunda leitura do mesmo código não gera segundo registro. Operando 4 h sem rede, o terminal continua registrando e, restabelecida a conexão, sincroniza em até 2 min ⚠️ (RNF-12) sem duplicar nenhum par inscrição–sessão. Correção manual sem justificativa é recusada; com justificativa, aparece na trilha com autor e valores anterior e posterior.

### RF-24 — Apuração de elegibilidade e emissão autosserviço do certificado

| Campo | Valor |
|---|---|
| Módulo | M7 Presença e Certificados |
| Prioridade | Deve ter |
| Origem | P4 — "Quero conseguir emitir meu certificado depois do evento." · OB4 |
| Depende de | RF-20, RF-23 |
| Regras aplicáveis | RN-14, RN-19, RN-23, RN-24, RN-25 |
| Pendência vinculada | LAC-04, LAC-11 |

**Descrição.** O sistema deve apurar a elegibilidade ao certificado pelo critério da política congelada na inscrição, liberar a emissão autosserviço em até 48 h ⚠️ após o encerramento do item e consolidar a carga horária somando apenas as atividades efetivamente frequentadas. Ao participante inelegível, o sistema deve informar o critério não atendido com o percentual apurado e abrir pedido de revisão de presença dentro do prazo definido.

**Critério de verificação.** Participante com 75 % ⚠️ de presença nas sessões obrigatórias emite o certificado sem solicitar nada à organização; com 74 %, recebe recusa que declara "75 % exigidos, 74 % apurados" e o botão de pedido de revisão. O certificado do Congresso soma somente as atividades com check-in confirmado, ignorando atividade sobreposta sem presença registrada. Nenhum certificado é liberado antes do encerramento do item, e todos ficam disponíveis dentro de 48 h após ele.

### RF-25 — Código de verificação, validação pública e revogação do certificado

| Campo | Valor |
|---|---|
| Módulo | M7 Presença e Certificados |
| Prioridade | Deve ter |
| Origem | P4 — "Quero conseguir emitir meu certificado depois do evento." · OB9 |
| Depende de | RF-24 |
| Regras aplicáveis | RN-06, RN-19 |
| Pendência vinculada | - |

**Descrição.** O sistema deve atribuir a cada certificado emitido um código de verificação único e permanente, impresso no documento, e publicar uma página acessível sem autenticação que, dado o código, confirme titular, atividade, carga horária, data de realização e situação (válido ou revogado). A página não deve expor qualquer outro dado pessoal nem permitir listagem, varredura ou busca por nome, e-mail ou documento.

**Critério de verificação.** Sem sessão, informar o código do certificado de Marina retorna exatamente cinco campos e nenhum outro. Buscar por "Marina" ou pelo e-mail dela não retorna resultado. Enumerar códigos sequenciais não produz certificados válidos e é limitado por taxa. Revogado o certificado, a mesma consulta passa a exibir "revogado" com a data, sem apagar o registro anterior.

---

## M8 — Notificações e Comprovantes

### RF-26 — Comprovante de solicitação e comprovante de inscrição confirmada

| Campo | Valor |
|---|---|
| Módulo | M8 Notificações e Comprovantes |
| Prioridade | Deve ter |
| Origem | P2 — "Seria interessante receber um comprovante logo após a inscrição." · F3 — "Precisamos confirmar os pagamentos antes de liberar determinadas inscrições." · OB6 |
| Depende de | RF-09, RF-16 |
| Regras aplicáveis | RN-05 |
| Pendência vinculada | INC-01 |

**Descrição.** O sistema deve emitir, imediatamente após a submissão, um comprovante de solicitação contendo protocolo, itens selecionados, valor devido e prazo de pagamento, com declaração destacada de que o documento não garante vaga. O sistema deve emitir, na confirmação, um artefato distinto — o comprovante de inscrição confirmada — que é o único a conter o código de check-in.

**Critério de verificação.** A submissão onerosa produz, em até 1 min, um PDF cujo título é "Comprovante de solicitação", com aviso em destaque de que não garante vaga e sem nenhum código de check-in. A liquidação produz um segundo PDF, "Comprovante de inscrição confirmada", com o código. Tentar fazer check-in com o primeiro documento é recusado. Ambos os PDFs têm texto selecionável, idioma declarado e marcação PDF/UA (RNF-21).

### RF-27 — Notificação por transição de estado com registro do ciclo de entrega

| Campo | Valor |
|---|---|
| Módulo | M8 Notificações e Comprovantes |
| Prioridade | Deve ter |
| Origem | OB5 (não foi informado como serão enviados comprovantes e notificações) · P2 — "Seria interessante receber um comprovante logo após a inscrição." |
| Depende de | RF-19, RF-26 |
| Regras aplicáveis | RN-04 |
| Pendência vinculada | LAC-05 |

**Descrição.** O sistema deve notificar o participante em toda transição relevante da inscrição por e-mail, canal oficial que não pode ser desativado por preferência, com espelho obrigatório na central in-app, download em PDF e reenvio autosserviço. O sistema deve registrar por mensagem a situação de entrega — enviada, entregue, falhou, reenviada — com retentativa automática em caso de falha, e deve suspender convites de fila dependentes de e-mail não entregue.

**Critério de verificação.** Cada transição de estado gera exatamente uma mensagem por e-mail e um item na central. A central exibe a situação de entrega por mensagem, e uma falha simulada produz três retentativas automáticas (RNF-11). O participante não encontra opção de desativar o e-mail transacional, mas encontra a de recusar divulgação. O reenvio autosserviço gera nova entrada com a marcação "reenviada", sem alterar o documento original.

### RF-28 — Canais adicionais por WhatsApp e SMS

| Campo | Valor |
|---|---|
| Módulo | M8 Notificações e Comprovantes |
| Prioridade | Não terá agora |
| Origem | OB5 (não foi informado como serão enviados comprovantes e notificações) |
| Depende de | RF-27 |
| Regras aplicáveis | - |
| Pendência vinculada | LAC-05 |

**Descrição.** O sistema poderá, em evolução posterior ao MVP, entregar a mesma notificação por canais adicionais — WhatsApp e SMS —, complementares ao e-mail e jamais substitutivos. O e-mail permanece canal oficial obrigatório e não desativável (RN-04); os canais adicionais são habilitados por evento pelo parâmetro `canaisNotificacao` e aceitos ou recusados individualmente pelo participante. O sistema deve registrar a situação de entrega separadamente por canal e permitir reenvio autosserviço por canal, e a falha em canal adicional não pode impedir nem atrasar a entrega por e-mail.

**Critério de verificação.** Com o WhatsApp habilitado no Congresso e aceito por Marina, uma transição de estado da inscrição produz duas entregas para ela — e-mail e WhatsApp — e apenas uma para o participante que recusou o canal adicional; a central de notificações exibe enviada, entregue, falhou ou reenviada para cada canal em linha própria. Forçar falha permanente do canal adicional não altera a situação de entrega do e-mail nem o instante em que ele chega. A tentativa de desativar o e-mail é recusada em qualquer combinação de canais.

**Nota de escopo.** A exigência de que a central de notificações nasça com abstração de canal é **restrição de projeto**, não comportamento observável, e por isso está declarada na seção 11 de `requisitos-nao-funcionais.md`, sob manutenibilidade. Ela vale no MVP mesmo com este RF fora de escopo — é o que torna o adiamento barato.

---

## M9 — Painéis e Relatórios

### RF-29 — Painel de ocupação e inscrições com defasagem declarada e alertas

| Campo | Valor |
|---|---|
| Módulo | M9 Painéis e Relatórios |
| Prioridade | Deve ter |
| Origem | O4 — "Gostaríamos de acompanhar a quantidade de inscritos em tempo real." · O1 — "Precisamos controlar automaticamente o número de vagas." · O2 — "Quando um evento lotar, seria interessante criar uma lista de espera." |
| Depende de | RF-12, RF-15, RF-16 |
| Regras aplicáveis | RN-20 |
| Pendência vinculada | AMB-01 |

**Descrição.** O sistema deve apresentar ao organizador, por evento e por atividade, capacidade publicada, inscrições confirmadas, reservas ativas, convites pendentes, tamanho da fila e vagas disponíveis, sempre com o instante da última atualização visível na própria tela. O sistema deve permitir configurar alertas por limiar de lotação, tamanho de fila, volume de reservas expiradas e falha de conciliação ou de notificação.

**Critério de verificação.** O painel do Congresso exibe os seis números por atividade e o carimbo de atualização; a soma confirmadas + reservas + convites + disponíveis é sempre igual à capacidade. Uma confirmação registrada aparece no painel em até 30 s ⚠️ e no rótulo público em até 5 s ⚠️ (RNF-03). Configurar alerta de lotação em 90 % ⚠️ dispara notificação a Rafael Nunes ao atingir 36 de 40 vagas, uma única vez por limiar cruzado.

### RF-30 — Relatórios de funil, fila, presença e fechamento financeiro

| Campo | Valor |
|---|---|
| Módulo | M9 Painéis e Relatórios |
| Prioridade | Deveria ter |
| Origem | O4 — "Gostaríamos de acompanhar a quantidade de inscritos em tempo real." · O2 — "Quando um evento lotar, seria interessante criar uma lista de espera." · F1 — "Alguns eventos são gratuitos e outros exigem pagamento." · F2 — "Em alguns casos o participante tem direito ao reembolso, em outros não." · F3 — "Precisamos confirmar os pagamentos antes de liberar determinadas inscrições." · OB4, OB8, OB9 |
| Depende de | RF-29, RF-33, RF-34 |
| Regras aplicáveis | RN-15 |
| Pendência vinculada | AMB-03, LAC-08 |

**Descrição.** O sistema deve produzir cinco relatórios: conversão do funil de inscrição (visualização, solicitação, reserva, confirmação), desempenho da lista de espera (tempo de espera, taxa de aceite, cascatas), presença e ausência por atividade, certificados emitidos e recusados, e fechamento financeiro do evento. O sistema deve condicionar a exportação a papel autorizado e finalidade declarada, mascarar por padrão os campos pessoais e registrar autor, filtros aplicados e volume exportado.

**Critério de verificação.** Cada relatório abre com filtros por evento, atividade e período e exporta em CSV. A exportação por perfil sem autorização é recusada; a exportação sem finalidade declarada não é submetida. Por padrão, as colunas de e-mail e documento saem mascaradas, e a exibição em claro exige autorização específica registrada. Cada exportação gera entrada na trilha com autor, filtros e número de linhas (RF-34).

---

## M10 — Área do Palestrante

### RF-31 — Programação própria e lista de inscritos em perfil mínimo

| Campo | Valor |
|---|---|
| Módulo | M10 Área do Palestrante |
| Prioridade | Deve ter |
| Origem | L1 — "Gostaria de consultar a lista de participantes inscritos em minhas atividades." · OB8 |
| Depende de | RF-04, RF-33 |
| Regras aplicáveis | RN-15 |
| Pendência vinculada | LAC-08 |

**Descrição.** O sistema deve oferecer ao palestrante a visão exclusiva das atividades que conduz, com data, horário, sala ou canal, capacidade, ocupação e destaque para as alterações recentes. O sistema deve exibir a lista de inscritos limitada a nome social ou completo, organização e situação da inscrição, sem qualquer dado de contato, documento ou informação sensível.

**Critério de verificação.** A Dra. Helena Prado vê apenas as suas atividades; a URL de atividade de outro palestrante retorna acesso negado e gera registro. A lista de inscritos tem exatamente três colunas, e a inspeção da resposta do serviço não contém e-mail, telefone, documento, dados de pagamento nem necessidades de acessibilidade ou alimentares. Mudanças ocorridas nas últimas 72 h ⚠️ aparecem destacadas com valor anterior e novo.

### RF-32 — Indicadores agregados do público e contato mediante consentimento

| Campo | Valor |
|---|---|
| Módulo | M10 Área do Palestrante |
| Prioridade | Poderia ter |
| Origem | L1 — "Gostaria de consultar a lista de participantes inscritos em minhas atividades." · OB8 |
| Depende de | RF-03, RF-31 |
| Regras aplicáveis | RN-15 |
| Pendência vinculada | LAC-08 |

**Descrição.** O sistema deve apresentar ao palestrante totais e distribuição agregada do público de suas atividades — por área de atuação, organização e experiência declarada —, suprimindo qualquer recorte com menos de cinco pessoas ⚠️. O sistema deve exibir o contato de um participante apenas enquanto houver consentimento específico e vigente daquele titular para essa finalidade.

**Critério de verificação.** Num recorte com 4 participantes, a célula aparece suprimida e não é recuperável por combinação de filtros. Dos 30 inscritos na oficina, apenas os 6 que consentiram têm contato exibido; revogado o consentimento de um deles, o campo desaparece da tela e das exportações em até 60 s ⚠️ (RNF-17). Cada consulta a dados de terceiro gera registro de acesso com autor, finalidade e volume (RF-34).

---

## M11 — Administração e Auditoria

### RF-33 — Autorização por papel com escopo por evento e concessão temporária

| Campo | Valor |
|---|---|
| Módulo | M11 Administração e Auditoria |
| Prioridade | Deve ter |
| Origem | L1 — "Gostaria de consultar a lista de participantes inscritos em minhas atividades." · O1 — "Precisamos controlar automaticamente o número de vagas." · OB8, OB9 · Derivado |
| Depende de | RF-02 |
| Regras aplicáveis | - |
| Pendência vinculada | AMB-03, LAC-13 |

**Descrição.** O sistema deve avaliar toda autorização pelo par papel e escopo, restringindo o organizador aos eventos sob sua responsabilidade, o palestrante às atividades em que está designado e o financeiro aos dados de cobrança de forma transversal. O sistema deve permitir conceder e revogar papéis com prazo de validade, inclusive o de operador de credenciamento, encerrado automaticamente ao fim do evento ou do dia designado.

**Critério de verificação.** Rafael Nunes, organizador do Congresso, recebe negativa ao acessar qualquer tela do Encontro Corporativo Nexa, e a tentativa fica registrada. O operador de credenciamento criado para o dia 12 perde o acesso à meia-noite sem nenhuma ação humana, e a revogação aparece na trilha. Uma matriz de teste cobrindo os cinco papéis contra as operações de cada módulo não apresenta nenhuma combinação permitida fora do escopo declarado.

### RF-34 — Trilha de auditoria imutável com registro de acesso a dados pessoais

| Campo | Valor |
|---|---|
| Módulo | M11 Administração e Auditoria |
| Prioridade | Deve ter |
| Origem | OB9 (ausência de requisitos de segurança e privacidade) · OB8 |
| Depende de | RF-02 |
| Regras aplicáveis | RN-17 |
| Pendência vinculada | - |

**Descrição.** O sistema deve registrar em trilha somente de inclusão toda transição de inscrição, cobrança, política, papel e certificado, além de todo acesso de terceiros a dados pessoais, gravando autor, papel, motivo, valores anterior e posterior e identificador de correlação. O sistema não deve oferecer a nenhum perfil, inclusive administrativo, operação de alteração ou exclusão de registro de auditoria.

**Critério de verificação.** A linha do tempo completa de uma inscrição — da submissão à emissão do certificado — é reconstituída em consulta de até 10 s ⚠️ (RNF-16), com todos os campos preenchidos. Nenhuma interface expõe editar ou excluir registro de trilha, e a tentativa direta na base é rejeitada pelo controle de acesso e detectada pela verificação diária de integridade encadeada. A consulta da Dra. Helena Prado à lista de inscritos aparece na trilha como acesso a dados pessoais, com finalidade e volume.

---

## Fora do escopo desta versão

| # | Item excluído | Justificativa da exclusão | Referência |
|---|---|---|---|
| 1 | Emissão de documento fiscal (nota fiscal de serviço, recibo fiscal) | A elicitação não menciona obrigação fiscal e o regime tributário da Eventus não foi levantado; o MVP entrega exportação conciliável para o sistema contábil, o que preserva o fechamento sem assumir regra fiscal errada. | LAC-10, RF-30 |
| 2 | Canais WhatsApp e SMS | O e-mail é suficiente como canal oficial e cada canal adicional traz cadastro em provedor, custo por mensagem e consentimento próprio; o custo de adiar é zero porque a abstração de canal já nasce pronta. | RF-28, LAC-05 |
| 3 | Aplicativo móvel nativo (iOS/Android) | Todos os fluxos críticos, inclusive a leitura de QR no check-in, operam pela câmera do navegador em layout responsivo a partir de 320 px; um app duplicaria superfície de manutenção sem função nova. | RNF-23, RF-23 |
| 4 | Credenciamento físico com impressão de crachá e integração com catraca | Exige hardware, fila presencial e operação no local, que a elicitação não descreve; a presença é apurada por código ou QR de uso único, que resolve a finalidade (certificado) sem depender de equipamento. | RF-23, LAC-13 |
| 5 | Marketplace de eventos de terceiros | O escopo é o portfólio próprio da Eventus; abrir a organizadores externos criaria repasse financeiro, curadoria, contrato e suporte a terceiros, nenhum deles elicitado. | AMB-02, RF-06 |
| 6 | Boleto bancário e parcelamento em cartão | A compensação em dias é incompatível com a reserva de 30 min e exigiria um segundo modelo de vaga; enquanto a decisão não for homologada, o MVP oferece cartão e PIX, além do faturamento manual do evento corporativo. | INC-05, RF-13, RF-17 |
| 7 | Submissão e avaliação de trabalhos (chamada de trabalhos, revisão por pares) | Nenhum stakeholder mencionou o fluxo; é um subsistema com atores, prazos e estados próprios, cujo acoplamento à inscrição é fraco e pode ser tratado como produto separado. | Sem origem na elicitação |
| 8 | Transmissão ao vivo e integração com plataforma de webconferência | A modalidade das atividades ainda não foi definida; no MVP a presença remota, se existir, é apurada por tempo mínimo de conexão informado por integração simples, sem o sistema assumir a entrega do vídeo. | LAC-11, RF-04 |

## Requisitos condicionados a decisão pendente

Nenhum requisito abaixo está parado: cada linha declara o que já é construível com o valor padrão recomendado e o que muda quando o responsável homologar a decisão.

| RF | Questão aberta que o condiciona | O que já pode ser implementado mesmo assim |
|---|---|---|
| RF-01, RF-03 | LAC-12 — faixa etária mínima e dados de menores | Todo o cadastro, verificação de titularidade e central de privacidade. Falta apenas o bloco de consentimento do responsável legal, isolado como etapa opcional do fluxo. |
| RF-02 | LAC-09 — ausência integral de requisitos não funcionais | Autenticação, segundo fator por papel e ciclo de sessão com a linha de base RNF-01 a RNF-24; a homologação pode ajustar limiares, não a arquitetura. |
| RF-05, RF-06 | AMB-02 — o que é "evento disponível" no catálogo | Catálogo, busca, filtros e rótulos completos, com evento corporativo fechado marcado como não listável. A homologação define apenas a regra de exibição de evento com abertura futura. |
| RF-08, RF-12 | AMB-06 — unidade de inscrição (evento ou atividade) | Modelo de dois níveis com capacidade e fila próprias em cada um; a decisão apenas configura se a inscrição em atividades é obrigatória, opcional ou inexistente por evento. |
| RF-09, RF-16 | AMB-05 — quais inscrições exigem confirmação de pagamento | Toda a máquina de estados com a regra "valor devido maior que zero exige liquidação"; a homologação não muda código, muda a leitura contratual. |
| RF-11, RF-30, RF-33 | AMB-03 — alcance de "gerenciar participantes" | Consulta filtrada, inscrição em nome de terceiro, importação, cancelamento com motivo e exportação com finalidade, todos já restritos por escopo e auditados. Pendente apenas a lista final de campos exportáveis. |
| RF-13, RF-17 | INC-05 — reserva de 30 min versus meios de compensação lenta | Reserva, contador, expiração e devolução da vaga para cartão e PIX. Boleto fica desabilitado até a decisão; a alternativa (inscrição pendente sem consumir vaga) já tem o estado previsto. |
| RF-14, RF-15 | LAC-03 — funcionamento da lista de espera e níveis em que existe | Fila FIFO por item, posição consultável, convite com prazo e cascata. A decisão altera parâmetros (`modoListaEspera`, prazo do convite), não o mecanismo. |
| RF-16, RF-17 | LAC-10 — meios aceitos, prestador, parcelamento e documento fiscal | Integração com prestador único, notificação autenticada e idempotente, conciliação e fila de exceções. Emissão fiscal permanece fora do escopo. |
| RF-18, RF-21 | LAC-02 — hipóteses, percentuais, meio e prazo do reembolso | Simulação, memória de cálculo, caso de reembolso, alçada e segregação com as faixas padrão de 100 %, 50 % e 0 %. As faixas são configuráveis por evento. |
| RF-19, RF-20 | LAC-01 a LAC-08 — as oito indefinições da elicitação | O editor de política, a herança pelas atividades, a verificação de prontidão na publicação e o congelamento na inscrição. É exatamente o requisito que transforma pendência em parâmetro. |
| RF-21, RF-07 | INC-02 — cancelamento autosserviço versus eventos não canceláveis | Autosserviço condicionado à política, com a restrição visível antes da inscrição e recusa explicada. Pendente apenas a lista de tipos de evento com janela zero por padrão. |
| RF-22 | INC-03, LAC-07, AMB-04 — tratamento da sobreposição de horário | Detecção de interseção, alerta com confirmação consciente registrada e bloqueio quando há exigência de presença. A homologação escolhe o padrão do parâmetro, que já é configurável. |
| RF-23, RF-24 | LAC-04, LAC-11 — critério de certificado e apuração em atividade remota | Check-in por sessão com código de uso único, janela de registro, modo degradado, apuração de percentual e emissão autosserviço com limiar de 75 % ⚠️. Pendente a forma de apurar presença remota. |
| RF-26 | INC-01 — comprovante imediato versus liberação após pagamento | Os dois artefatos distintos, com o aviso destacado no primeiro e o código de check-in apenas no segundo. A decisão homologa a redação, não a existência dos documentos. |
| RF-27, RF-28 | LAC-05 — canais de envio e tratamento de falha de entrega | E-mail obrigatório, espelho in-app, PDF, reenvio, retentativa e situação de entrega por mensagem, sobre abstração de canal. Canais adicionais entram sem retrabalho. |
| RF-29, RF-06 | AMB-01 — significado de "tempo real" | Painéis e rótulos com defasagem máxima declarada de 30 s e 5 s ⚠️ e carimbo de última atualização visível. A homologação apenas confirma ou aperta os números. |
| RF-31, RF-32 | LAC-08 — dados do participante visíveis ao palestrante | Perfil mínimo, indicadores agregados com supressão de recortes menores que cinco e contato condicionado a consentimento vigente. A decisão pode ampliar o perfil, nunca reduzir o registro de acesso. |
