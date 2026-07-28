# Requisitos Não Funcionais — Eventus SGE

## 1. Advertência de origem: o cliente não pediu nada disto

A observação OB9 da elicitação é literal: *"não foram levantados requisitos relacionados à
segurança, desempenho, disponibilidade, acessibilidade e privacidade dos dados"*. Não houve
entrevista, número, meta de serviço, restrição de infraestrutura ou exigência regulatória
declarada por nenhum stakeholder sobre qualidade.

Consequências que o leitor precisa aceitar antes da primeira ficha:

| Fato | Implicação para este documento |
|---|---|
| Nenhum RNF foi enunciado por stakeholder | **Todos os 24 RNFs abaixo são derivados**, por dedução técnica a partir dos requisitos funcionais, ou legais, a partir da LGPD. |
| Nenhum número foi fornecido | **Todas as metas numéricas são proposta nossa**, calibradas pelo porte descrito na elicitação (congressos, workshops e eventos corporativos de uma empresa) e não por medição de produção. |
| Nenhuma meta foi homologada | Cada meta traz `⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável`. A seção 17 consolida as 24 em uma pauta única de homologação. |
| Erro de calibração é barato agora e caro depois | Metas de disponibilidade, retenção e criptografia condicionam arquitetura e contrato de infraestrutura. A homologação deve ocorrer **antes da primeira abertura de inscrições com dados reais** (LAC-09). |

Este documento não registra OB9 como pendência. Registra uma **linha de base construível**: cada
lacuna vira um número verificável, o número vira um método de verificação e o método vira uma
pergunta objetiva para quem tem autoridade de decidir.

## 2. Como ler cada ficha

| Campo | O que contém |
|---|---|
| Categoria | Característica de qualidade da ISO/IEC 25010 usada no canon, seguida da subcaracterística quando o agrupamento deste documento diferir do rótulo canônico. |
| Prioridade | MoSCoW. O canon fixa prioridade para requisito funcional, não para RNF; **os valores desta coluna são proposta deste trabalho**. |
| Origem | Códigos da elicitação (P1–P5, O1–O5, F1–F3, L1, OB1–OB9), `Derivado` quando é consequência técnica, e o dispositivo da LGPD quando é consequência legal. |
| Meta proposta | Número, unidade e condição de medição. Sem condição de medição não há meta, há adjetivo. |
| Método de verificação | Como se prova que a meta foi atingida, e em que ambiente. |
| Rastreio | Requisitos funcionais e regras de negócio sustentados pela meta, mais os casos de teste canônicos que a verificam. |

Duas convenções de leitura:

- **Concentração em "Deve ter" é deliberada.** Vinte e uma das vinte e quatro metas são obrigatórias
  porque uma linha de base de qualidade majoritariamente opcional não é linha de base. A negociação
  legítima não é *se* a meta existe, é *qual número* ela carrega — e é isso que a seção 17 abre.
- **Ambiente de medição padrão:** homologação com topologia equivalente à de produção e massa
  sintética de um congresso de 5.000 inscrições, 40 atividades e 12 trilhas paralelas, salvo
  indicação em contrário na ficha.

## 3. Índice das vinte e quatro metas

| ID | Categoria | Título | Número em jogo | Prioridade |
|---|---|---|---|---|
| RNF-01 | Eficiência de desempenho | Resposta do catálogo e da busca | p95 1,5 s · p99 3 s · 500 sessões | Deve ter |
| RNF-02 | Eficiência de desempenho | Resposta da reserva e da confirmação sob concorrência | p95 2 s · p99 4 s · 200 concorrentes | Deve ter |
| RNF-03 | Eficiência de desempenho | Defasagem do acompanhamento de inscritos | 30 s no painel · 5 s no rótulo público | Deve ter |
| RNF-04 | Eficiência de desempenho | Pico de abertura de inscrições | 3.000 sessões · 500 tentativas/min · 15 min · erro 0,5 % | Deve ter |
| RNF-05 | Eficiência de desempenho | Capacidade do portfólio | 50 eventos · 500 atividades · 100.000 contas · 200.000 inscrições/ano | Deveria ter |
| RNF-06 | Confiabilidade | Integridade do controle de vagas | 0 sobrevenda em 200 concorrentes × 50 rodadas | Deve ter |
| RNF-07 | Confiabilidade | Idempotência de solicitações e de retornos financeiros | 1.000 reenvios · 100 submissões · efeito único · chave 24 h | Deve ter |
| RNF-08 | Confiabilidade | Pontualidade dos processos temporizados | vaga em 60 s · convite em 2 min · teto de 31 min | Deve ter |
| RNF-09 | Confiabilidade | Disponibilidade | 99,5 % mensal · 99,9 % na janela crítica · aviso de 72 h | Deve ter |
| RNF-10 | Confiabilidade | Recuperabilidade | RPO 15 min · RTO 4 h · teste trimestral | Deve ter |
| RNF-11 | Confiabilidade | Entrega das mensagens transacionais | 99 % em 5 min · 3 retentativas | Deve ter |
| RNF-12 | Confiabilidade | Registro de presença em rede instável | 4 h sem rede · sincronização em 2 min · 0 duplicidade | Deve ter |
| RNF-13 | Segurança | Criptografia em trânsito e em repouso | TLS 1.3/1.2 · AES-256 · Argon2id · rotação anual | Deve ter |
| RNF-14 | Segurança | Não retenção de dados de cartão | 0 registro de PAN, validade ou CVV | Deve ter |
| RNF-15 | Segurança | Autenticação forte e ciclo de sessão | 2FA por papel · 5 falhas/15 min · 30 min · 12 h · 24 h | Deve ter |
| RNF-16 | Segurança | Auditabilidade da trilha | zero alteração aceita · adulteração detectada em 24 h · 5 anos · consulta em 10 s | Deve ter |
| RNF-17 | Conformidade legal (LGPD) | Minimização na exposição a terceiros | 0 contato sem consentimento · revogação em 60 s · 100 % registrado | Deve ter |
| RNF-18 | Conformidade legal (LGPD) | Atendimento aos direitos do titular | 100 % em 15 dias · mediana 5 dias | Deve ter |
| RNF-19 | Conformidade legal (LGPD) | Retenção e descarte por categoria | 5 anos · 10 anos · 24 meses · 6 meses · 30 dias | Deve ter |
| RNF-20 | Usabilidade → Acessibilidade | Acessibilidade das interfaces | WCAG 2.2 AA · contraste 4,5:1 · 200 % em 320 px | Deve ter |
| RNF-21 | Usabilidade → Acessibilidade | Acessibilidade dos documentos gerados | PDF/UA · 0 documento rasterizado | Deve ter / Deveria ter |
| RNF-22 | Usabilidade | Transparência das regras no ponto de decisão | 1 clique · 100 % dos eventos · memória de cálculo | Deve ter |
| RNF-23 | Compatibilidade e localização | Uso móvel, navegadores e referência temporal | 320 px · 2 versões · UTC e America/Sao_Paulo | Deve ter |
| RNF-24 | Manutenibilidade | Configurabilidade da política e dos limiares | 8 parâmetros · efeito em 1 min · 0 indisponibilidade | Deve ter |

Distribuição por categoria: eficiência de desempenho 5 · confiabilidade 7 · segurança 4 ·
conformidade legal 3 · usabilidade e acessibilidade 3 · compatibilidade 1 · manutenibilidade 1.

## 4. Adequação funcional

O canon não aloca nenhum RNF a esta característica, e a omissão é correta: adequação funcional
(completude, correção e pertinência) é verificada pelos 34 requisitos funcionais, pelas 30 regras
de negócio e pelos 26 casos de teste, não por uma meta paralela que os duplicaria.

Duas metas de outras categorias carregam, na prática, a correção funcional do núcleo do sistema e
devem ser lidas também sob esta ótica:

| Meta | Por que é adequação funcional disfarçada |
|---|---|
| RNF-06 | A invariante de RN-20 (vagas disponíveis) é o critério de correção do controle de vagas prometido em O1; violá-la não é lentidão, é resultado errado. |
| RNF-07 | Idempotência é a condição para que o mesmo pedido do participante ou o mesmo aviso do prestador produza um único efeito de negócio — correção sob repetição, não desempenho. |

## 5. Eficiência de desempenho

### RNF-01 — Resposta do catálogo e da busca

| Campo | Valor |
|---|---|
| Categoria | Eficiência de desempenho (comportamento temporal) |
| Prioridade | Deve ter |
| Origem | P1, O4, OB9 — derivado de RF-06 e RF-07 |
| Meta proposta | p95 ≤ 1,5 s e p99 ≤ 3 s de tempo de resposta no servidor, com 500 sessões simultâneas de navegação exploratória sobre catálogo com 50 eventos publicados e 2.000 atividades, medido na borda da aplicação e excluído o último quilômetro de rede do cliente. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Teste de carga com perfil de navegação (listagem, filtro combinado, abertura da página do evento) em homologação, executado a cada versão candidata; monitoração contínua de percentis em produção com alerta ao ultrapassar a meta por 5 minutos. |
| Rastreio | RF-06, RF-07 · RN-26 · sem caso de teste canônico dedicado |

**Requisito.** O sistema **deve** responder às consultas do catálogo público dentro do orçamento
acima e, quando o orçamento for excedido, **deve** degradar primeiro a busca textual e os filtros
combináveis, preservando a listagem e o rótulo de disponibilidade de RN-26 — nunca o contrário,
porque um rótulo de vagas desatualizado produz decisão errada e uma busca lenta produz apenas
espera.

### RNF-02 — Resposta da reserva e da confirmação sob concorrência

| Campo | Valor |
|---|---|
| Categoria | Eficiência de desempenho (comportamento temporal) |
| Prioridade | Deve ter |
| Origem | OB6, O1, OB9 — derivado de RF-12, RF-13 e RF-16 |
| Meta proposta | p95 ≤ 2 s e p99 ≤ 4 s entre a submissão e a resposta com protocolo, com 200 requisições concorrentes disputando vagas da mesma atividade, medido no servidor e incluindo a chamada ao prestador de pagamento conforme o orçamento da seção 15. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Teste de carga com concorrência dirigida a um único item (Workshop de Engenharia de Prompt, 40 vagas), com prestador de pagamento simulado calibrado em 700 ms; medição separada do trecho interno e do trecho de terceiros. |
| Rastreio | RF-12, RF-13, RF-16 · RN-07, RN-11, RN-20 · CT-02, CT-06 |

**Requisito.** O sistema **deve** concluir a criação da reserva temporária e a confirmação da
inscrição dentro do orçamento, e **deve** publicar as duas medições — total e trecho interno —
separadamente, para que a parcela sob responsabilidade do prestador de pagamento não seja
confundida com desempenho próprio no momento da apuração do serviço.

### RNF-03 — Defasagem do acompanhamento de inscritos

| Campo | Valor |
|---|---|
| Categoria | Eficiência de desempenho (comportamento temporal) |
| Prioridade | Deve ter |
| Origem | O4 — derivado de RF-29 e RF-06; quantificação de AMB-01 |
| Meta proposta | Defasagem ≤ 30 s entre a persistência da transição e o número exibido no painel do organizador, e ≤ 5 s no rótulo público de disponibilidade, medida por marcação de instante na origem e na tela, com carimbo de última atualização sempre visível. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Teste instrumentado que confirma 300 inscrições no Congresso Eventus de Tecnologia 2026 e compara o instante de commit com o instante de leitura do painel e do rótulo; verificação da presença do carimbo por inspeção de interface. |
| Rastreio | RF-29, RF-06 · RN-20, RN-26 · CT-24 |

**Requisito.** O sistema **deve** exibir, em todo indicador derivado de contagem, o instante da
última atualização junto ao próprio número, e **não deve** empregar a expressão "tempo real" em
nenhuma interface: O4 pediu acompanhamento imediato, e a resposta honesta é um número com
defasagem declarada, não um advérbio.

### RNF-04 — Pico de abertura de inscrições

| Campo | Valor |
|---|---|
| Categoria | Eficiência de desempenho (capacidade) |
| Prioridade | Deve ter |
| Origem | O1, O4, OB9 — derivado de RF-08 e RF-12 |
| Meta proposta | 3.000 sessões simultâneas e 500 tentativas de inscrição por minuto sustentadas por 15 minutos, com taxa de erro ≤ 0,5 % das requisições e sem ultrapassar os percentis de RNF-01 e RNF-02. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Teste de carga com rampa de 60 s até o pico, mantido por 15 min, repetido com e sem cache de borda aquecido; ensaio obrigatório antes de cada abertura de evento com expectativa acima de 1.500 inscrições. |
| Rastreio | RF-06, RF-08, RF-12 · RN-07 · CT-06 (cobertura parcial) |

**Requisito.** O sistema **deve** sustentar o pico sem sobrevenda e sem recusa silenciosa; quando a
capacidade de processamento for atingida, **deve** aplicar fila de admissão com posição e tempo
estimado visíveis ao participante, e **não deve** derrubar requisição já admitida ao fluxo de
pagamento — perder uma sessão de reserva em curso equivale a perder a vaga que ela já consumiu.

### RNF-05 — Capacidade do portfólio

| Campo | Valor |
|---|---|
| Categoria | Eficiência de desempenho (capacidade) |
| Prioridade | Deveria ter |
| Origem | Derivado, OB9 — dimensionamento de RF-04, RF-12 e RF-29 |
| Meta proposta | 50 eventos ativos simultâneos, 500 atividades por evento, 100.000 contas e 200.000 inscrições por ano sem redesenho de arquitetura, verificado com volume sintético equivalente a 3 anos de operação. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Teste de volume com base carregada no envelope declarado, medindo a estabilidade dos percentis de RNF-01 e RNF-02 e o tempo das consultas de painel e de relatório; revisão de plano de execução das consultas críticas. |
| Rastreio | RF-04, RF-12, RF-29, RF-30 · RN-01 · sem caso de teste canônico dedicado |

**Requisito.** O sistema **deve** operar dentro do envelope declarado sem alteração estrutural de
arquitetura, e a documentação de operação **deve** registrar qual componente satura primeiro
quando o envelope for ultrapassado — o envelope só é útil se vier acompanhado do próximo gargalo
conhecido.

## 6. Compatibilidade

### RNF-23 — Uso móvel, navegadores e referência temporal

| Campo | Valor |
|---|---|
| Categoria | Compatibilidade e localização (coexistência e interoperabilidade com o ambiente do usuário) |
| Prioridade | Deve ter |
| Origem | P4, OB4, Derivado — sustenta RF-23 e RF-06 |
| Meta proposta | Layout operável a partir de viewport de 320 px nas duas versões mais recentes de Chrome, Safari, Firefox e Edge, com leitura de QR pela câmera do navegador sem instalação de aplicativo; 100 % dos instantes armazenados em UTC e exibidos em America/Sao_Paulo com fuso explícito, valores em reais, e zero divergência de horário na virada do horário de verão. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Matriz de compatibilidade executada a cada versão nos quatro navegadores em dois tamanhos (320 px e 1.280 px); teste de câmera em dispositivo físico de baixo custo; teste automatizado de fronteira temporal com relógio deslocado para a virada do horário de verão e para 23h59 do dia anterior ao evento. |
| Rastreio | RF-23, RF-06, RF-07 · RN-01, RN-13 · sem caso de teste canônico dedicado |

**Requisito.** O sistema **deve** operar o check-in de RF-23 no navegador do dispositivo do
participante e do operador de credenciamento, sem instalação, e **deve** persistir todo instante em
UTC, convertendo apenas na apresentação — a detecção de sobreposição de RN-13, o corte de 6 horas
do convite de RN-21 e a janela de cancelamento de RN-09 são cálculos sobre instantes, e uma
comparação feita em horário local produz erro de uma hora exatamente nos dois dias do ano em que
ninguém está testando.

## 7. Interação e usabilidade

### RNF-22 — Transparência das regras no ponto de decisão

| Campo | Valor |
|---|---|
| Categoria | Usabilidade (reconhecibilidade da adequação e proteção contra erro do usuário) |
| Prioridade | Deve ter |
| Origem | P2, P3, O3, OB1, OB2 — derivado de RF-07, RF-13 e RF-21 |
| Meta proposta | Política de cancelamento, reembolso, lista de espera e critério de certificado a no máximo 1 clique da página de inscrição em 100 % dos eventos publicados; contador regressivo da reserva visível durante 100 % da vigência do hold de 30 minutos; memória de cálculo do reembolso exibida antes de 100 % das confirmações de cancelamento oneroso. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Verificação automatizada de conteúdo em todos os eventos publicados a cada versão (nenhuma página de inscrição sem o bloco de política); teste de interface do contador e da memória de cálculo; teste com 5 participantes por ciclo, medindo acerto ao responder "quanto volta se eu cancelar amanhã". |
| Rastreio | RF-07, RF-13, RF-21 · RN-09, RN-22, RN-26 · CT-02, CT-13 |

**Requisito.** O sistema **deve** apresentar a regra aplicável no instante em que ela muda a decisão
do participante, e não em página de termos: a política de cancelamento antes de iniciar a
inscrição, o prazo da reserva durante o pagamento e a faixa de restituição antes de confirmar o
cancelamento. Regra publicada depois da decisão não é transparência, é justificativa.

## 8. Acessibilidade

O canon classifica RNF-20 e RNF-21 sob "Usabilidade", rótulo compatível com a ISO/IEC 25010, em
que acessibilidade é subcaracterística de usabilidade e de capacidade de interação. Este documento
as agrupa em seção própria porque o método de verificação, o padrão normativo e o responsável pela
homologação são distintos dos demais itens de usabilidade.

### RNF-20 — Acessibilidade das interfaces

| Campo | Valor |
|---|---|
| Categoria | Usabilidade → Acessibilidade |
| Prioridade | Deve ter |
| Origem | OB9 — derivado de RF-06, RF-08, RF-21, RF-23 e RF-24; LBI (Lei 13.146/2015, art. 63) para sítios de uso público |
| Meta proposta | Conformidade WCAG 2.2 nível AA nos cinco fluxos essenciais (descoberta, inscrição, cancelamento, check-in e emissão de certificado), com zero violação de nível A ou AA nesses fluxos, operação completa por teclado sem armadilha de foco, contraste mínimo 4,5:1 em texto e 3:1 em componente de interface, e ampliação a 200 % em viewport de 320 px sem rolagem horizontal. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Checklist WCAG 2.2 AA aplicado por avaliador treinado nos cinco fluxos a cada versão; varredura automatizada na esteira, bloqueando a entrega em violação de nível A ou AA; teste assistido com leitor de tela (NVDA e VoiceOver) a cada release; teste exclusivo por teclado no fluxo de reserva com contador ativo. |
| Rastreio | RF-06, RF-08, RF-21, RF-23, RF-24 · — · CT-26 |

**Requisito.** O sistema **deve** permitir a conclusão integral dos cinco fluxos essenciais sem
mouse e com leitor de tela, e **deve** anunciar por região dinâmica as mudanças de estado que
governam prazo — expiração da reserva, esgotamento de vaga e resultado do check-in — porque o
contador regressivo de RF-13 é informação temporal crítica: exibi-lo apenas como número em tela
transfere ao participante cego o risco de perder a vaga.

### RNF-21 — Acessibilidade dos documentos gerados

| Campo | Valor |
|---|---|
| Categoria | Usabilidade → Acessibilidade |
| Prioridade | Deve ter (texto selecionável e idioma declarado) · Deveria ter (marcação PDF/UA completa) |
| Origem | OB9, P4 — derivado de RF-26, RF-24 e RF-25 |
| Meta proposta | 100 % dos comprovantes e certificados em PDF com texto selecionável, idioma pt-BR declarado no documento e estrutura de marcação conforme PDF/UA (ISO 14289-1); zero documento gerado como imagem rasterizada; código de verificação de RN-06 presente também como texto, além do QR. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Validação automatizada de cada modelo de documento com verificador PDF/UA na esteira; extração de texto do PDF comparada ao conteúdo esperado; leitura do certificado do Workshop de Engenharia de Prompt com leitor de tela. |
| Rastreio | RF-26, RF-24, RF-25 · RN-05, RN-06 · CT-26 |

**Requisito.** O sistema **deve** gerar comprovantes e certificados como documentos estruturados, e
**não deve** depender do QR como único portador do código de verificação — quem usa leitor de tela
precisa copiar o código para a página pública de RF-25, e um QR sem alternativa textual torna a
verificação de autenticidade inacessível justamente para o titular do documento.

## 9. Confiabilidade

### RNF-06 — Integridade do controle de vagas

| Campo | Valor |
|---|---|
| Categoria | Confiabilidade (maturidade e tolerância a falhas) |
| Prioridade | Deve ter |
| Origem | O1, OB6 — derivado de RF-12 |
| Meta proposta | Zero sobrevenda em 200 requisições concorrentes pela última vaga do mesmo item, repetido 50 vezes; ao fim de cada rodada, confirmadas + reservas ativas + convites pendentes + bloqueios ≤ capacidade publicada, verificado por consulta de invariante. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Teste de concorrência determinístico em homologação (50 rodadas, invariante de RN-20 avaliada após cada uma); verificação diária da mesma invariante em produção sobre todos os itens com inscrições abertas, com alerta imediato em qualquer violação. |
| Rastreio | RF-12 · RN-07, RN-20 · CT-06 |

**Requisito.** O sistema **deve** serializar por item a operação de ocupação de vaga e **deve** falhar
fechado: na impossibilidade de garantir a serialização — indisponibilidade do mecanismo de bloqueio
ou partição do banco — recusa a nova reserva com mensagem de indisponibilidade temporária em vez de
confirmar sob risco. Uma sobrevenda no Congresso Eventus de Tecnologia 2026 não é revertida por
retentativa; é revertida por alguém dizendo a um participante confirmado que ele não entra.

### RNF-07 — Idempotência de solicitações e de retornos financeiros

| Campo | Valor |
|---|---|
| Categoria | Confiabilidade (tolerância a falhas) |
| Prioridade | Deve ter |
| Origem | F3, Derivado — sustenta RF-09 e RF-16 |
| Meta proposta | 1.000 reenvios da mesma notificação do prestador de pagamento e 100 submissões com a mesma chave de idempotência produzem exatamente um efeito de negócio; chave de idempotência retida por 24 h e identificador de transação do prestador por 30 dias para deduplicação tardia. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Teste automatizado de repetição com envio concorrente e sequencial, comparando o número de inscrições, cobranças e mensagens geradas; injeção de reenvio aleatório do prestador em homologação por 24 h. |
| Rastreio | RF-09, RF-16 · RN-08, RN-10 · CT-07 |

**Requisito.** O sistema **deve** tratar toda submissão de inscrição e toda notificação assíncrona do
prestador como potencialmente repetida, e **deve** responder à repetição com o resultado original —
mesmo protocolo, mesmo estado — e não com erro de duplicidade, para que o cliente que perdeu a
resposta possa reconciliar sem intervenção humana.

### RNF-08 — Pontualidade dos processos temporizados

| Campo | Valor |
|---|---|
| Categoria | Confiabilidade (maturidade) |
| Prioridade | Deve ter |
| Origem | OB6, OB3 — derivado de RF-13 e RF-15 |
| Meta proposta | Vaga devolvida ao conjunto disponível em até 60 s do vencimento da reserva (p99), convite da lista de espera emitido em até 2 min da liberação (p95) e nenhuma reserva observada ativa por mais de 31 minutos em varredura de 1 minuto. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Teste temporizado com 500 reservas vencendo em janela de 5 minutos, medindo o atraso de liberação e de emissão de convite; sonda contínua em produção sobre a idade máxima das reservas ativas, com alerta acima de 31 minutos. |
| Rastreio | RF-13, RF-15 · RN-11, RN-21, RN-29 · CT-04, CT-09 |

**Requisito.** O sistema **deve** executar a expiração de reservas e a promoção da fila por processo
próprio, independente de acesso do usuário, e **deve** manter a pontualidade sob acúmulo: se a
varredura atrasar, processa por ordem de vencimento e registra o atraso, nunca descarta o lote.
O teto de 31 minutos pressupõe que nenhum meio de pagamento de compensação lenta consuma vaga
(INC-05); admitir boleto com reserva invalida esta meta e exige recalibrá-la.

### RNF-09 — Disponibilidade

| Campo | Valor |
|---|---|
| Categoria | Confiabilidade (disponibilidade) |
| Prioridade | Deve ter |
| Origem | OB9, O1 — sustenta RF-06, RF-08 e RF-23 |
| Meta proposta | 99,5 % mensal em regime normal (orçamento de 3 h 39 min de indisponibilidade por mês) e 99,9 % na janela crítica, definida como de 24 h antes a 48 h depois da abertura das inscrições e os dias de realização do evento (orçamento de 43 s por dia crítico); manutenção programada apenas fora dessas janelas, anunciada com 72 h de antecedência. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Sonda externa a cada 30 s sobre os fluxos de catálogo, inscrição e check-in, com apuração mensal por fluxo e não apenas por disponibilidade agregada do servidor; revisão trimestral do consumo do orçamento de erro. |
| Rastreio | RF-06, RF-08, RF-23 · — · sem caso de teste canônico dedicado |

**Requisito.** O sistema **deve** apurar disponibilidade por fluxo essencial, considerando indisponível
o fluxo que responda com erro ou fora do orçamento de RNF-01 e RNF-02, e o cronograma de manutenção
**deve** consultar o calendário de aberturas antes de agendar janela. Observação de calibração: um
único acionamento do plano de recuperação de RNF-10 (RTO de 4 h) consome mais do que o orçamento
mensal inteiro de 99,5 % — as duas metas precisam ser homologadas em conjunto.

### RNF-10 — Recuperabilidade

| Campo | Valor |
|---|---|
| Categoria | Confiabilidade (recuperabilidade) |
| Prioridade | Deve ter |
| Origem | OB9 — protege RF-16, RF-23 e RF-34 |
| Meta proposta | RPO ≤ 15 min e RTO ≤ 4 h para dados de inscrição, pagamento, presença e trilha de auditoria, com teste de restauração completo documentado a cada trimestre e cópia mantida em região distinta da de produção. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Exercício trimestral de restauração em ambiente limpo, cronometrado, com conferência de integridade referencial entre inscrições, cobranças e registros de presença e verificação da cadeia de resumos da trilha (RNF-16). |
| Rastreio | RF-16, RF-23, RF-34 · RN-17 · sem caso de teste canônico dedicado |

**Requisito.** O sistema **deve** garantir que a restauração preserve a consistência entre vaga
ocupada, cobrança liquidada e presença registrada; quando a restauração implicar perda de janela
temporal, a operação **deve** reprocessar as notificações do prestador retidas no período usando as
chaves de idempotência de RNF-07, e não reconstruir estados de inscrição manualmente.

### RNF-11 — Entrega das mensagens transacionais

| Campo | Valor |
|---|---|
| Categoria | Confiabilidade (maturidade) |
| Prioridade | Deve ter |
| Origem | OB5, P2 — derivado de RF-26 e RF-27 |
| Meta proposta | 99 % das mensagens transacionais aceitas pelo destinatário em até 5 min do gatilho, com três retentativas automáticas em intervalos crescentes, domínio remetente com SPF, DKIM e DMARC configurados em política de rejeição, e situação de entrega registrada por mensagem. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Medição contínua do intervalo entre gatilho e confirmação de entrega do provedor de e-mail; caixa de teste em quatro provedores populares por versão; inspeção mensal dos relatórios DMARC e da taxa de rejeição por domínio. |
| Rastreio | RF-26, RF-27 · RN-04, RN-05 · CT-25 |

**Requisito.** O sistema **deve** registrar e exibir a situação de cada mensagem (enviada, entregue,
falhou, reenviada) e **deve** suspender o consumo do prazo do convite de lista de espera enquanto o
e-mail correspondente não constar como entregue, reemitindo-o — cobrar do participante um prazo de
24 h que começou a correr numa mensagem que nunca chegou transfere ao titular uma falha de
infraestrutura.

### RNF-12 — Registro de presença em rede instável

| Campo | Valor |
|---|---|
| Categoria | Confiabilidade (tolerância a falhas) |
| Prioridade | Deve ter |
| Origem | OB4, OB9 — derivado de RF-23 |
| Meta proposta | Operação de check-in por até 4 h sem conectividade, com armazenamento local cifrado, limite de 2.000 registros por dispositivo e sincronização completa em até 2 min do restabelecimento, com zero duplicidade para o mesmo par inscrição e sessão. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Teste de campo com rede desligada por 4 h em dois dispositivos registrando a mesma sessão, seguido de sincronização simultânea; verificação de que o armazenamento local não é legível fora do aplicativo autenticado. |
| Rastreio | RF-23 · RN-19, RN-23 · CT-19 |

**Requisito.** O sistema **deve** manter o credenciamento operante sem rede — o salão do Encontro
Corporativo Nexa não tem cobertura garantida — e **deve** resolver conflito de sincronização pelo
instante local do primeiro registro válido, descartando repetições do mesmo par inscrição e sessão
sem gerar erro ao operador.

## 10. Segurança

### RNF-13 — Criptografia em trânsito e em repouso

| Campo | Valor |
|---|---|
| Categoria | Segurança (confidencialidade e integridade) |
| Prioridade | Deve ter |
| Origem | OB9 — LGPD art. 46 e art. 6º, VII; sustenta RF-02 e RF-16 |
| Meta proposta | TLS 1.3 preferencial e TLS 1.2 como mínimo aceito, com protocolos anteriores recusados; dados pessoais em repouso cifrados com AES-256; senhas com Argon2id e sal por usuário; chaves em cofre dedicado com rotação anual e sem chave em código, variável de ambiente em texto claro ou cópia de segurança. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Varredura de configuração TLS a cada versão e mensalmente em produção; revisão de código dirigida ao armazenamento de segredos e ao fluxo de senha; inspeção do inventário de chaves e da data da última rotação; teste de intrusão anual. |
| Rastreio | RF-02, RF-16, RF-03 · RN-18 · sem caso de teste canônico dedicado |

**Requisito.** O sistema **deve** cifrar dados pessoais em trânsito e em repouso, incluindo cópias de
segurança, ambientes de homologação com dado real e o armazenamento local do dispositivo de
check-in de RNF-12, e **não deve** admitir ambiente de desenvolvimento com base copiada de produção
sem anonimização prévia.

### RNF-14 — Não retenção de dados de cartão

| Campo | Valor |
|---|---|
| Categoria | Segurança (confidencialidade) |
| Prioridade | Deve ter |
| Origem | F3, OB9 — implementa RN-18; sustenta RF-16 |
| Meta proposta | Zero ocorrência de número completo do cartão, data de validade ou código de segurança em base de dados, registro de aplicação, mensagem de erro, cópia de segurança ou ambiente de homologação; persistem apenas o identificador tokenizado, a bandeira, os quatro últimos dígitos e a situação da transação. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Varredura automatizada por padrão de PAN e por algoritmo de Luhn sobre base, registros e cópias, executada semanalmente e a cada versão; revisão de código de todo ponto de integração com o prestador; teste de captura de tráfego confirmando que o dado do cartão não transita pelo servidor da Eventus. |
| Rastreio | RF-16 · RN-18 · sem caso de teste canônico dedicado |

**Requisito.** O sistema **deve** delegar integralmente a captura do meio de pagamento ao prestador, e
qualquer detecção positiva na varredura **deve** ser tratada como incidente de segurança com
expurgo imediato e registro na trilha — a meta é zero, portanto não existe ocorrência tolerável.

### RNF-15 — Autenticação forte e ciclo de sessão

| Campo | Valor |
|---|---|
| Categoria | Segurança (autenticidade e responsabilização) |
| Prioridade | Deve ter |
| Origem | OB9 — derivado de RF-02 e RF-33 |
| Meta proposta | Segundo fator obrigatório para organizador, financeiro, palestrante e equipe de TI, e opcional ao participante; bloqueio progressivo após 5 falhas em 15 min; expiração por inatividade de 30 min em papéis administrativos e de 12 h no participante, com limite absoluto de sessão de 24 h para todos os papéis. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Teste automatizado por papel a cada versão (acesso sem segundo fator deve ser recusado); teste de expiração com relógio controlado; revisão trimestral das sessões ativas e dos papéis temporários concedidos por RF-33. |
| Rastreio | RF-02, RF-33 · RN-15, RN-16 · sem caso de teste canônico dedicado |

**Requisito.** O sistema **deve** exigir segundo fator de todo papel com acesso a dados pessoais de
terceiros ou a dinheiro, incluindo o operador de credenciamento enquanto o papel temporário estiver
vigente, e **deve** encerrar as sessões abertas quando o papel for revogado — inclusive a revogação
automática ao fim do evento prevista em RF-33.

### RNF-16 — Auditabilidade da trilha

| Campo | Valor |
|---|---|
| Categoria | Segurança (responsabilização e não repúdio) |
| Prioridade | Deve ter |
| Origem | OB9 — implementa RN-17; sustenta RF-34 e HU-23 |
| Meta proposta | Trilha imutável e verificável por terceiro: zero operação de alteração ou de exclusão bem-sucedida sobre registro já gravado, para qualquer perfil, dentro dos 5 anos de retenção; toda adulteração de registro anterior detectada e alertada em até 24 h; reconstituição completa do histórico de uma inscrição em consulta que responda em até 10 s, por auditor externo, sem acesso ao código-fonte. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Tentativa automatizada de alteração e de exclusão a partir de perfil administrativo, que deve falhar; injeção controlada de adulteração em registro antigo, que deve ser detectada e alertada na verificação seguinte; medição do tempo de reconstituição sobre inscrição com 40 transições. Mecanismo recomendado para atingir a meta — não imposto por ela: encadeamento de cada registro pelo resumo criptográfico do anterior, com verificação diária da cadeia. Qualquer mecanismo que produza a mesma detectabilidade é aceitável. |
| Rastreio | RF-34, RF-33 · RN-17 · sem caso de teste canônico dedicado |

**Requisito.** O sistema **deve** manter a trilha imutável e verificável por terceiro, e **deve** cobrir
também o acesso de leitura a dados pessoais, com autor, papel, motivo e identificador de
correlação. Observação de calibração: a retenção de 5 anos é menor que a do certificado (10 anos em
RNF-19), de modo que a evidência de emissão ou de revogação de um certificado desaparece antes do
próprio certificado — a divergência precisa ser resolvida na homologação.

## 11. Manutenibilidade

### RNF-24 — Configurabilidade da política e dos limiares

| Campo | Valor |
|---|---|
| Categoria | Manutenibilidade (modificabilidade e modularidade) |
| Prioridade | Deve ter |
| Origem | OB1, OB2, OB3, OB4, OB5, OB6, OB7, OB8 — derivado de RF-19, RF-20 e RF-29 |
| Meta proposta | Os oito parâmetros do Perfil de Política e os limiares de alerta alteráveis por interface administrativa, com efeito observável em até 1 min, zero indisponibilidade, zero retroação sobre inscrições confirmadas e 100 % das alterações com autor, instante e justificativa registrados. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Teste que altera cada um dos oito parâmetros e mede o intervalo até o efeito; verificação de que inscrições confirmadas antes da alteração continuam avaliadas pela cópia congelada de RF-20; inspeção da trilha após cada alteração. |
| Rastreio | RF-19, RF-20, RF-29 · RN-03, RN-14 · CT-17 |

**Requisito.** O sistema **deve** tratar política como dado, não como código: nenhuma das oito
decisões pendentes da elicitação (OB1–OB8) pode exigir nova versão do software para mudar de valor.
Esta é a meta que converte a indefinição da elicitação em risco administrável — se a homologação
alterar o default de 48 h ou de 30 min, a mudança é de configuração, não de projeto.

### 11.1 Restrição de projeto — abstração de canal na central de notificações

Não é um RNF novo (o registro canônico fecha em RNF-24): é uma **restrição de projeto** que o
MVP precisa respeitar para que RF-28 continue barato depois. Fica registrada aqui, sob
manutenibilidade, e não dentro de RF-28, porque não descreve comportamento observável do sistema
entregue — descreve como a solução deve ser estruturada.

| Campo | Valor |
|---|---|
| Enunciado | A emissão de uma notificação não conhece o meio de entrega. O roteamento é resolvido em tempo de execução a partir do parâmetro `canaisNotificacao` da política do evento, e cada canal é um adaptador registrável. |
| Meta verificável | Habilitar um canal adicional exige apenas configuração: zero alteração no contrato de emissão e zero alteração no código que publica os eventos de notificação. |
| Método de verificação | Teste de integração que registra um adaptador de canal fictício, ativa-o em um evento e verifica que as mensagens são roteadas para ele; o teste falha se o contrato de emissão precisar de qualquer novo parâmetro. A conferência é sobre o resultado do teste, não sobre revisão de código. |
| Rastreio | RF-27, RF-28 · RN-04 · LAC-05 · RNF-11 |
| Custo de ignorar | Adicionar WhatsApp em R2 passaria a exigir alteração em todos os pontos de emissão, o que transforma um item "Não terá agora" em reescrita. |

## 12. Portabilidade

O canon não aloca RNF a esta característica, e a decisão é consciente: a elicitação não menciona
migração de plataforma, instalação em ambiente do cliente nem substituição de componente de
infraestrutura, e uma meta de portabilidade sem demanda seria requisito inventado.

As duas metas que absorvem o que há de portabilidade real no sistema:

| Meta | Parcela de portabilidade que cobre |
|---|---|
| RNF-23 | Adaptabilidade ao ambiente do usuário final — quatro navegadores, dois tamanhos de viewport e leitura de QR sem instalação, o que dispensa distribuição de aplicativo por loja. |
| RNF-14 | Substituibilidade do prestador de pagamento: como nenhum dado de cartão é retido e a integração se dá por token e notificação assíncrona idempotente (RNF-07), a troca de prestador não exige migração de dado sensível. |

Se a Eventus decidir hospedar o sistema em ambiente próprio ou multiambiente, a portabilidade
deixa de ser vazia e passa a exigir meta própria — pergunta a levar à homologação da seção 17.

## 13. Conformidade legal e privacidade (LGPD)

### RNF-17 — Minimização na exposição a terceiros

| Campo | Valor |
|---|---|
| Categoria | Conformidade legal (LGPD) — privacidade |
| Prioridade | Deve ter |
| Origem | OB8, OB9, L1 — LGPD art. 6º, III (necessidade) e art. 8º, §5º (revogação); implementa RN-15; sustenta RF-31, RF-32 e RF-25 |
| Meta proposta | Zero campo de contato retornado ao palestrante sem consentimento específico vigente do titular, verificado por teste automatizado a cada versão; revogação propagada a todas as visões e exportações de terceiros em até 60 s; supressão de recorte agregado com menos de 5 pessoas; 100 % dos acessos de terceiros a dados pessoais registrados na trilha. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Teste automatizado por papel sobre todos os campos devolvidos pelas interfaces do palestrante (lista, indicador agregado e exportação), bloqueando a entrega em qualquer vazamento; teste de revogação cronometrado; auditoria mensal por amostragem dos registros de acesso. |
| Rastreio | RF-31, RF-32, RF-03, RF-25, RF-30 · RN-15 · CT-22, CT-23 |

**Requisito.** O sistema **deve** aplicar o perfil mínimo de visibilidade como padrão em toda
interface destinada a terceiros e **deve** tratar a revogação do consentimento como remoção
imediata, inclusive de exportações já geradas, que passam a ser invalidadas para novo download.
A Dra. Helena Prado precisa dimensionar a oficina, não precisa do e-mail de quem se inscreveu.

### RNF-18 — Atendimento aos direitos do titular

| Campo | Valor |
|---|---|
| Categoria | Conformidade legal (LGPD) — direitos do titular |
| Prioridade | Deve ter |
| Origem | OB9 — LGPD art. 18 (direitos) e art. 19, II (prazo de 15 dias); derivado de RF-03 |
| Meta proposta | 100 % das solicitações de confirmação, acesso, correção, portabilidade e eliminação respondidas em até 15 dias corridos, com mediana de 5 dias, e protocolo emitido em até 1 min do pedido; exportação disponibilizada em JSON e CSV acompanhada de dicionário de campos. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Relatório mensal de prazos por tipo de solicitação, com alerta em qualquer pedido acima de 10 dias sem desfecho; auditoria trimestral que executa um pedido real de cada tipo de ponta a ponta; conferência do arquivo exportado contra o modelo de dados. |
| Rastreio | RF-03, RF-34 · RN-15 · sem caso de teste canônico dedicado |

**Requisito.** O sistema **deve** atender aos direitos do titular por autosserviço sempre que possível
e, quando a eliminação encontrar hipótese legal de guarda (art. 16, I), **deve** informar ao titular
qual dado permanece, sob qual fundamento e até quando — negar a exclusão sem explicar o prazo
remanescente é resposta incompleta.

### RNF-19 — Retenção e descarte por categoria

| Campo | Valor |
|---|---|
| Categoria | Conformidade legal (LGPD) — ciclo de vida do dado |
| Prioridade | Deve ter |
| Origem | OB9 — LGPD art. 15 e art. 16 (término do tratamento); sustenta RF-03 e RF-34 |
| Meta proposta | Inscrição e pagamento 5 anos; certificado e respectivo código de verificação 10 anos; registros de notificação 24 meses; dados de navegação 6 meses; necessidades de acessibilidade e restrições alimentares eliminadas em até 30 dias após o encerramento do evento. Descarte executado por rotina automática com tolerância de 7 dias e relatório mensal do volume descartado por categoria. ⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável |
| Método de verificação | Rotina de expurgo testada com relógio deslocado sobre massa sintética envelhecida; auditoria mensal que busca registro fora do prazo em base, cópia de segurança e índice de busca; conferência de que o dado sensível de acessibilidade some das exportações antigas. |
| Rastreio | RF-03, RF-34, RF-30 · RN-17 · sem caso de teste canônico dedicado |

**Requisito.** O sistema **deve** aplicar prazo de retenção por categoria de dado, e não por tabela, e
o descarte **deve** alcançar cópias de segurança, ambientes espelho e índices de busca. Dado
sensível de acessibilidade coletado para o Congresso Eventus de Tecnologia 2026 não tem finalidade
alguma trinta dias depois do encerramento — mantê-lo é risco puro, sem contrapartida.

## 14. Cenários de qualidade

Cinco cenários no formato de seis partes (fonte, estímulo, artefato, ambiente, resposta, medida).
Eles não recebem identificador próprio: a rastreabilidade se faz pelos RNFs e casos de teste
citados em cada um.

### Cenário 1 — Pico de abertura das inscrições do Congresso Eventus de Tecnologia 2026

| Parte | Conteúdo |
|---|---|
| Fonte do estímulo | Público externo alertado pela campanha de abertura, concentrado no minuto anunciado. |
| Estímulo | 3.000 sessões simultâneas e 500 tentativas de inscrição por minuto durante 15 minutos, 60 % delas dirigidas a 3 das 40 atividades. |
| Artefato | Catálogo público, serviço de inscrição em dois níveis, transação de vagas, integração com o prestador de pagamento. |
| Ambiente | Produção, dentro da janela crítica de RNF-09 (99,9 %), com cache de borda frio no primeiro minuto. |
| Resposta esperada | Sistema admite as sessões, mantém os percentis de RNF-01 e RNF-02, não sobrevende nenhuma das 40 atividades, aplica fila de admissão com posição visível ao saturar e não descarta nenhuma reserva já em curso no pagamento. |
| Medida da resposta | Erro ≤ 0,5 % das requisições; p95 do catálogo ≤ 1,5 s e da inscrição ≤ 2 s; invariante de RN-20 íntegra nas 40 atividades ao fim do teste; zero reserva ativa perdida por reinício de instância. |
| Verifica | RNF-04, RNF-01, RNF-02, RNF-06, RNF-09 · CT-06 |

### Cenário 2 — Duzentas pessoas disputando a última vaga do Workshop de Engenharia de Prompt

| Parte | Conteúdo |
|---|---|
| Fonte do estímulo | Participantes que receberam simultaneamente o aviso de "últimas vagas" derivado de RN-26. |
| Estímulo | 200 submissões concorrentes ao mesmo item em janela de 2 s, com 1 vaga disponível. |
| Artefato | Transação de ocupação de vaga, criação da reserva temporária, lista de espera. |
| Ambiente | Produção, carga normal fora da janela crítica. |
| Resposta esperada | Exatamente uma reserva criada (estado E-02, contador de 30 min iniciado); as 199 demais recebem indisponibilidade com oferta imediata de entrada na fila e posição devolvida na mesma resposta; nenhuma delas recebe mensagem de erro técnico. |
| Medida da resposta | 1 reserva e 0 sobrevenda em 50 rodadas; invariante de RN-07 verificada ao fim de cada rodada; p99 da resposta ≤ 4 s, inclusive para as recusadas; 100 % das recusas com opção de fila quando a política habilitar. |
| Verifica | RNF-06, RNF-02, RNF-08 · CT-06, CT-08 |

### Cenário 3 — Prestador de pagamento indisponível durante a venda do Congresso

| Parte | Conteúdo |
|---|---|
| Fonte do estímulo | Prestador externo de pagamento em falha total. |
| Estímulo | 100 % de erro ou tempo esgotado nas chamadas de criação de cobrança por 20 minutos, no meio da janela de inscrições. |
| Artefato | Integração de cobrança, disjuntor, reservas ativas, fila de exceções da conciliação. |
| Ambiente | Produção, janela crítica, 180 reservas ativas no instante da falha. |
| Resposta esperada | Disjuntor abre em até 30 s do início da falha; novas inscrições onerosas não criam reserva nem consomem vaga, e recebem aviso explícito de indisponibilidade do pagamento com sugestão de retorno; inscrições gratuitas, check-in, catálogo e emissão de certificado permanecem operantes; as 180 reservas ativas têm o prazo suspenso e retomado no fechamento do disjuntor, com novo instante-limite comunicado ao participante (⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável); liquidações reconhecidas após o restabelecimento e após a expiração entram na fila de exceções de RF-17. |
| Medida da resposta | Zero vaga consumida por reserva não pagável durante a falha; zero cobrança duplicada no restabelecimento (RNF-07); disponibilidade dos demais fluxos preservada na apuração por fluxo de RNF-09; 100 % das reservas suspensas notificadas com o novo prazo em até 5 min do fechamento do disjuntor (RNF-11). |
| Verifica | RNF-07, RNF-08, RNF-09, RNF-11 · CT-05, CT-07 |

### Cenário 4 — Pedido de eliminação de dados após o Encontro Corporativo Nexa

| Parte | Conteúdo |
|---|---|
| Fonte do estímulo | Marina Alves, titular dos dados, pela central de privacidade de RF-03, três dias após o encerramento do evento, tendo um certificado emitido e um pagamento liquidado de R$ 380,00. |
| Estímulo | Solicitação de eliminação de todos os dados pessoais, com revogação simultânea do consentimento de exibição de contato ao palestrante. |
| Artefato | Central de privacidade, base de inscrições, registros de pagamento, certificado e página pública de verificação, visões do palestrante, trilha de auditoria. |
| Ambiente | Produção, operação normal, com exportação da lista de inscritos já baixada pela Dra. Helena Prado no dia anterior. |
| Resposta esperada | Protocolo emitido imediatamente; contato removido das visões e exportações do palestrante e o arquivo já baixado invalidado para novo download; dados sem hipótese de guarda eliminados; registro fiscal e de pagamento retido por 5 anos e certificado retido por 10 anos, com o fundamento informado ao titular; a titular escolhe entre manter o certificado válido — porque o artefato serve ao próprio interesse dela perante terceiros — ou revogá-lo, passando a página pública a exibir situação "revogado a pedido do titular" sem qualquer outro dado (⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável). |
| Medida da resposta | Protocolo em ≤ 1 min; propagação da revogação às visões de terceiros em ≤ 60 s; desfecho completo em ≤ 15 dias corridos, com mediana de 5 dias; 100 % das operações de eliminação e de acesso registradas na trilha; zero campo de contato remanescente em consulta do palestrante após a revogação. |
| Verifica | RNF-17, RNF-18, RNF-19, RNF-16 · CT-22, CT-23 |

### Cenário 5 — Participante com leitor de tela emite o certificado do Workshop de Engenharia de Prompt

| Parte | Conteúdo |
|---|---|
| Fonte do estímulo | Participante que utiliza leitor de tela (NVDA com Firefox) e navega exclusivamente por teclado. |
| Estímulo | Emissão autosserviço do certificado 40 horas após o encerramento, seguida de verificação do código na página pública. |
| Artefato | Tela de Minhas Inscrições, apuração de elegibilidade, gerador de PDF, página pública de verificação. |
| Ambiente | Homologação com massa real anonimizada, viewport de 320 px com ampliação a 200 %. |
| Resposta esperada | Percurso completo por teclado, com foco visível e ordem previsível; carga horária e critério de presença anunciados como texto, não apenas como gráfico; PDF gerado com marcação PDF/UA, idioma pt-BR declarado e código de verificação em texto além do QR; página pública de verificação operável por teclado e legível pelo leitor de tela. |
| Medida da resposta | Zero violação WCAG 2.2 de nível A ou AA no fluxo; 100 % das ações alcançáveis por teclado; PDF aprovado no verificador PDF/UA com zero erro; tempo de conclusão no máximo 1,5 vez o do mesmo fluxo com mouse (⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável). |
| Verifica | RNF-20, RNF-21, RNF-22 · CT-26 |

## 15. Orçamento de desempenho

Meta não distribuída entre camadas não é gerenciável: quando o p95 estoura, ninguém sabe de quem é
a culpa. As tabelas abaixo repartem os alvos de RNF-02 e RNF-03 em parcelas com dono.

### 15.1 Submissão da inscrição com criação de reserva (alvo de RNF-02)

| Camada | Orçamento p95 | Orçamento p99 | Dono | Observação |
|---|---|---|---|---|
| Rede do cliente até a borda, incluindo negociação TLS | 180 ms | 300 ms | Infraestrutura | Medido a partir da borda; conexão móvel 4G como referência. |
| Balanceador, autenticação de sessão e limite de taxa | 70 ms | 140 ms | Infraestrutura | Inclui verificação de papel e escopo de RF-33. |
| Aplicação — validação de política congelada, agenda e conflito | 150 ms | 300 ms | Desenvolvimento | Avaliação de RN-03, RN-10 e RN-13 sobre a agenda do participante. |
| Banco — transação serializada de ocupação de vaga | 350 ms | 700 ms | Desenvolvimento | Trecho sob contenção; é a parcela que degrada primeiro no cenário 2. |
| Persistência da reserva e da trilha de auditoria | 120 ms | 240 ms | Desenvolvimento | Escrita somente de inclusão de RNF-16, na mesma transação lógica. |
| Prestador de pagamento — criação da intenção de cobrança | 700 ms | 1.400 ms | Terceiro | Tempo esgotado duro em 2,5 s; sem retentativa em caso de tempo esgotado. |
| Publicação da notificação na fila (assíncrono) | 30 ms | 60 ms | Desenvolvimento | Só a publicação entra no caminho crítico; o envio é fora dele (RNF-11). |
| Serialização e envio da resposta | 100 ms | 200 ms | Desenvolvimento | Inclui o corpo com protocolo, prazo e contador. |
| Folga de segurança | 300 ms | 660 ms | Arquitetura | Absorve coleta de lixo, variação de rede e uma retentativa curta de conexão. |
| **Total** | **2.000 ms** | **4.000 ms** | — | Coincide com a meta de RNF-02. |

Três regras de uso deste orçamento:

1. **O trecho de terceiros é isolado.** Disjuntor abre após 5 falhas ou tempos esgotados em 30 s e
   dispara o cenário 3. Sem esse isolamento, o p95 de RNF-02 é uma promessa sobre um sistema que
   não é nosso.
2. **A parcela do banco é a que se defende primeiro.** Em contenção, reduz-se escopo da transação
   antes de aumentar tempo de espera; ampliar o tempo de espera transforma disputa de vaga em fila
   invisível.
3. **Estouro de parcela é alerta antes de ser incidente.** Cada linha tem alerta próprio em 80 % do
   orçamento, o que permite agir antes que a meta agregada seja violada.

### 15.2 Propagação do número de vagas (alvo de 5 s de RNF-03)

| Etapa | Orçamento | Observação |
|---|---|---|
| Commit da transação até a publicação do evento de domínio | 200 ms | Publicação transacional, sem perda de evento. |
| Consumo do evento e recálculo do rótulo de RN-26 | 800 ms | Cálculo de vagas disponíveis por RN-20. |
| Invalidação da chave no cache de borda | 1.000 ms | Invalidação dirigida por item, nunca purga total. |
| Tempo de vida residual na borda | 2.000 ms | Teto de 2 s para a chave de disponibilidade. |
| Revalidação no navegador | 1.000 ms | Cabeçalho de revalidação obrigatória no rótulo. |
| **Total** | **5.000 ms** | Coincide com o rótulo público de RNF-03. |

Para o painel do organizador, o orçamento de 30 s reparte-se em coleta de eventos (5 s), agregação
incremental por evento e atividade (15 s) e atualização da tela com carimbo de instante (10 s).

## 16. Conformidade LGPD

### 16.1 Inventário de dados pessoais

| Dado pessoal coletado | Finalidade declarada | Base legal (LGPD) | Retenção | Quem pode ver |
|---|---|---|---|---|
| Nome completo | Identificação do inscrito, controle de vaga e emissão de certificado | Art. 7º, V — execução de contrato | 5 anos com a inscrição; 10 anos no certificado | Titular, organizador do evento (escopo), financeiro, palestrante (perfil mínimo), operador de credenciamento |
| Nome social | Tratamento respeitoso e identificação em credencial e certificado | Art. 7º, V — execução de contrato | Igual ao nome completo | Mesmos papéis; exibido com precedência sobre o nome completo em toda interface |
| E-mail | Canal oficial de comunicação transacional (RN-04) | Art. 7º, V — execução de contrato | 5 anos com a inscrição | Titular, organizador do evento, financeiro, TI mediante registro; **palestrante somente com consentimento** — ver 16.2 |
| Telefone | Contato alternativo em incidente operacional do evento | Art. 7º, I — consentimento (campo opcional) | 5 anos ou até a revogação | Titular, organizador do evento; nunca palestrante |
| CPF ou documento | Conciliação financeira e comprovação fiscal do pagamento | Art. 7º, II — cumprimento de obrigação legal | 5 anos | Titular, financeiro; nunca palestrante, nunca organizador |
| Organização ou vínculo | Dimensionamento e calibração da atividade | Art. 7º, IX — legítimo interesse, com avaliação de impacto registrada | 5 anos com a inscrição | Titular, organizador do evento, palestrante (perfil mínimo) |
| Dados de pagamento tokenizados (token, bandeira, quatro últimos dígitos, situação) | Cobrança, conciliação e estorno pelo mesmo meio | Art. 7º, V — execução de contrato | 5 anos | Titular (parcial), financeiro; nunca organizador, nunca palestrante |
| Registros de check-in por sessão | Apuração da elegibilidade ao certificado (RN-23) | Art. 7º, V — execução de contrato | 5 anos com a inscrição; consolidado de carga horária 10 anos com o certificado | Titular, organizador do evento, operador de credenciamento; palestrante apenas como situação da inscrição |
| Necessidades de acessibilidade | Provisão de acomodação razoável na atividade | Art. 11, I — consentimento específico e destacado (dado sensível) | Até 30 dias após o encerramento do evento | Titular, organizador do evento, operador de credenciamento designado; **nunca palestrante** (RN-15) |
| Restrições alimentares | Planejamento de coffee break e refeições do evento | Art. 11, I — consentimento específico e destacado (pode revelar convicção religiosa) | Até 30 dias após o encerramento do evento | Titular, organizador do evento; nunca palestrante |
| Consentimento do responsável legal (16 a 18 anos) | Validade da inscrição de adolescente (LAC-12) | Art. 14 — melhor interesse do adolescente | 5 anos com a inscrição | Titular, responsável legal, organizador do evento |
| Registros de consentimento e de revogação | Prova do consentimento e da sua retirada | Art. 8º, §1º — ônus da prova do controlador | 5 anos após a revogação (⚠️ DECISÃO PROPOSTA — requer homologação do stakeholder responsável) | Titular, TI, encarregado |
| Registros de notificação e situação de entrega | Comprovação de comunicação de prazos e transições | Art. 7º, V — execução de contrato | 24 meses | Titular, organizador do evento, TI |
| Trilha de auditoria e registro de acesso a dados pessoais | Responsabilização, resposta a contestação e prestação de contas | Art. 37 e art. 46 — registro das operações e segurança | 5 anos | TI, encarregado, auditoria; nunca palestrante |
| Dados de navegação, endereço IP e registro de sessão | Segurança do acesso e detecção de abuso | Art. 7º, IX — legítimo interesse (segurança) | 6 meses | TI, encarregado |

Nenhum dado desta tabela é coletado sem finalidade declarada na própria coleta (art. 9º), e nenhum
campo sensível é obrigatório para concluir inscrição — recusar informar necessidade de
acessibilidade não impede inscrever-se, apenas impede a acomodação correspondente.

### 16.2 O caso do e-mail do participante e a visibilidade para o palestrante

O e-mail é o dado que mais tensiona a especificação: é canal oficial não recusável para o sistema
(RN-04) e é exatamente o dado que o palestrante mais gostaria de ter (L1, OB8). Separar por
finalidade resolve o conflito sem negar nenhum dos dois interesses.

| Finalidade do uso do e-mail | Base legal | Pode ser recusada pelo titular? | Regra no sistema |
|---|---|---|---|
| Comunicação transacional da inscrição (comprovante, convite de fila, expiração de reserva, cancelamento, liberação de certificado) | Art. 7º, V — execução de contrato | Não. Desativar equivaleria a renunciar ao aviso de perda da própria vaga | Canal obrigatório de RN-04, com espelho in-app e situação de entrega por mensagem (RNF-11) |
| Divulgação de eventos futuros da Eventus | Art. 7º, I — consentimento | Sim, com recusa por um clique e efeito imediato | Consentimento separado, jamais pré-marcado, revogável na central de privacidade (RF-03) |
| Exibição do contato à Dra. Helena Prado ou a qualquer palestrante | Art. 7º, I — consentimento específico por finalidade | Sim, a qualquer tempo (art. 8º, §5º) | Exibição condicionada a consentimento vigente daquele titular para aquele evento; revogação propaga em ≤ 60 s a listas, indicadores e exportações (RNF-17); toda visualização registrada na trilha (RF-34) |
| Exportação da lista de inscritos pelo organizador | Art. 7º, V, limitado pela necessidade (art. 6º, III) | Não se aplica — é operação do controlador | Exige papel autorizado, finalidade declarada, mascaramento por padrão e registro de autor, filtros e volume (RF-30) |

Três consequências que valem como regra de projeto:

1. **Padrão é ausência.** A visão do palestrante nasce sem campo de contato (RF-31); o consentimento
   acrescenta o campo, e não o contrário. Nenhuma configuração de evento pode inverter esse padrão.
2. **Revogação é remoção, não sinalização.** Após revogar, o e-mail some das listas, dos indicadores
   e das exportações já emitidas, que deixam de ser baixáveis.
3. **Agregado também identifica.** Indicador com recorte menor que cinco pessoas é suprimido
   (RF-32): "3 participantes da organização Nexa nesta oficina" é dado pessoal disfarçado de
   estatística.

### 16.3 Decisões automatizadas e direito à revisão

Três decisões do sistema afetam interesses do titular sem intervenção humana e atraem o art. 20 da
LGPD: a promoção — ou a não promoção — na lista de espera (RN-29), a apuração do valor a restituir
(RN-22) e a apuração de elegibilidade ao certificado (RN-23). Em todas, o sistema **deve** expor o
critério aplicado e o caminho de revisão: memória de cálculo antes da confirmação do cancelamento
(RNF-22), motivo e data-limite quando o cancelamento é recusado (RF-21), critério não atendido e
pedido de revisão de presença quando o certificado é negado (RF-24). Explicar a decisão automatizada
não é cortesia de interface; é obrigação legal.

## 17. Metas que exigem validação

Toda a linha de base é proposta. A tabela abaixo é a pauta de homologação: uma linha por meta, com o
número em jogo, quem tem autoridade para fixá-lo e o que acontece se o número estiver errado. Sem
homologação registrada desta pauta, o sistema pode ser construído, mas não pode abrir inscrições com
dados reais (LAC-09).

| ID | Número proposto | Quem homologa | Risco de errar |
|---|---|---|---|
| RNF-01 | p95 1,5 s e p99 3 s com 500 sessões | Téo Miranda com Rafael Nunes | Meta folgada esconde catálogo lento e derruba conversão no dia da abertura; meta apertada custa infraestrutura permanente para tráfego que só existe 15 minutos por evento. |
| RNF-02 | p95 2 s e p99 4 s com 200 concorrentes | Téo Miranda com Cleide Barros | Acima disso o participante duplica a submissão e pressiona a idempotência de RNF-07; abaixo disso o orçamento sobra apenas 1,3 s para o prestador de pagamento, o que pode inviabilizar a integração. |
| RNF-03 | 30 s no painel e 5 s no rótulo público | Rafael Nunes com Téo Miranda | Rótulo defasado gera tentativa de inscrição em item esgotado e reclamação; painel defasado atrasa a decisão de ampliar sala. Resolve AMB-01. |
| RNF-04 | 3.000 sessões, 500 tentativas/min, 15 min, erro ≤ 0,5 % | Rafael Nunes com Téo Miranda | Dimensionamento é o único dado ausente que não se descobre testando: se o maior congresso real for o dobro, o teste de carga aprova um sistema que cai na estreia. |
| RNF-05 | 50 eventos, 500 atividades, 100.000 contas, 200.000 inscrições/ano | Rafael Nunes | Envelope subestimado obriga redesenho no segundo ano; superestimado encarece a base desde o primeiro dia. |
| RNF-06 | Zero sobrevenda em 200 concorrentes × 50 rodadas | Rafael Nunes | Único número não negociável: qualquer tolerância maior que zero é decidir de antemão quantos participantes ficarão de fora com inscrição confirmada. |
| RNF-07 | 1.000 reenvios, chave por 24 h, transação por 30 dias | Cleide Barros com Téo Miranda | Janela curta de retenção da chave deixa passar cobrança duplicada em reenvio tardio do prestador, com estorno manual e desgaste. |
| RNF-08 | 60 s, 2 min, teto de 31 min | Rafael Nunes com Cleide Barros | Liberação lenta segura vaga que ninguém ocupa; convite lento encurta o prazo útil de quem espera. O teto de 31 min cai se boleto for aceito com reserva (INC-05). |
| RNF-09 | 99,5 % mensal e 99,9 % na janela crítica | Téo Miranda com Rafael Nunes | 99,9 % em regime normal multiplica o custo de infraestrutura; 99,5 % na janela crítica admite 22 min de queda exatamente no minuto da abertura. Precisa ser homologada junto com RNF-10. |
| RNF-10 | RPO 15 min e RTO 4 h | Téo Miranda | RTO de 4 h consome mais que o orçamento mensal inteiro de 99,5 %; RPO de 15 min pode significar perder pagamentos liquidados e não registrados. |
| RNF-11 | 99 % em 5 min, 3 retentativas | Rafael Nunes com Téo Miranda | Atraso de e-mail consome o prazo do convite de 24 h e faz o participante perder vaga por falha nossa; por isso o prazo fica suspenso até a entrega. |
| RNF-12 | 4 h sem rede, sincronização em 2 min, 2.000 registros | Téo Miranda com Rafael Nunes | Autonomia curta interrompe o credenciamento e destrói a base do certificado; sincronização frouxa gera presença duplicada e carga horária inflada. |
| RNF-13 | TLS 1.2 mínimo, AES-256, Argon2id, rotação anual | Téo Miranda | Recusar TLS 1.2 exclui dispositivos antigos do público; aceitar menos que isso é vulnerabilidade conhecida. Rotação anual pode ser insuficiente para o cofre de chaves de pagamento. |
| RNF-14 | Zero registro de PAN, validade ou CVV | Cleide Barros com Téo Miranda | Não é meta negociável, e sim condição contratual do prestador; qualquer exceção transfere à Eventus um escopo de conformidade que ela não tem estrutura para sustentar. |
| RNF-15 | 5 falhas/15 min, 30 min, 12 h, limite de 24 h | Téo Miranda | Sessão administrativa longa demais expõe dados de participantes em máquina compartilhada no balcão do evento; curta demais faz o operador de credenciamento reautenticar no meio da fila. |
| RNF-16 | Trilha inalterável, adulteração detectada em 24 h, 5 anos, consulta em 10 s | Téo Miranda com Cleide Barros | Retenção de 5 anos é menor que a do certificado (10 anos), deixando emissão e revogação sem evidência no fim do período — divergência a resolver nesta homologação. |
| RNF-17 | Zero contato sem consentimento, propagação em 60 s, corte de 5 pessoas | Téo Miranda com Rafael Nunes e Dra. Helena Prado | Errar para menos vaza contato sem base legal; errar para mais entrega ao palestrante uma lista inútil para preparar a atividade. Fecha OB8 e LAC-08. |
| RNF-18 | 15 dias corridos, mediana de 5 dias | Téo Miranda | O prazo de 15 dias é legal, não opcional (art. 19); a mediana de 5 dias é operacional e define quanto de equipe o atendimento consome. |
| RNF-19 | 5 anos, 10 anos, 24 meses, 6 meses, 30 dias | Téo Miranda com Cleide Barros | Reter além do necessário é infração e risco; descartar antes destrói prova fiscal e inviabiliza revalidar certificado antigo perante o RH de um empregador. |
| RNF-20 | WCAG 2.2 AA em 5 fluxos, contraste 4,5:1, 200 % em 320 px | Rafael Nunes com Téo Miranda | Reduzir o escopo dos fluxos essenciais é decidir quais participantes não conseguem se inscrever sozinhos; ampliar para AAA sem necessidade paralisa a entrega do MVP. |
| RNF-21 | PDF/UA em 100 % dos documentos | Rafael Nunes | Certificado como imagem é inacessível e ilegível por sistema de RH; exigir PDF/UA completo já no MVP pode atrasar a primeira emissão, daí a prioridade dividida. |
| RNF-22 | 1 clique, 100 % dos eventos, memória de cálculo sempre | Rafael Nunes com Cleide Barros | Regra escondida transforma cada cancelamento em atendimento humano — exatamente o que P3 pediu para eliminar. |
| RNF-23 | 320 px, duas versões de quatro navegadores, UTC e America/Sao_Paulo | Téo Miranda | Matriz ampla encarece o teste de cada versão; matriz estreita quebra o check-in no aparelho de quem está na porta da sala. Erro de fuso corrompe janela de cancelamento e corte do convite. |
| RNF-24 | 8 parâmetros configuráveis com efeito em 1 min | Rafael Nunes com Téo Miranda | Se a política virar código, toda homologação futura de OB1–OB8 vira nova versão do sistema, e o Perfil de Política deixa de cumprir a função para a qual foi criado. |

Duas metas adicionais surgidas nos cenários da seção 14 entram na mesma pauta: a **suspensão do
prazo da reserva enquanto o disjuntor do prestador estiver aberto** (cenário 3, homologa Cleide
Barros) e a **escolha do titular entre manter ou revogar o certificado no pedido de eliminação**
(cenário 4, homologa Téo Miranda com Cleide Barros).
