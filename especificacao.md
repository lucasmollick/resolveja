# Especificação — ResolveJá · Central de Atendimento (SPT)

**Versão:** 1.0
**Sistema especificado:** `index.html`, publicado em https://lucasmollick.github.io/resolveja/
**Equipe:** Guilherme Amorim, João Víctor Henrique e Lucas Freitas Mollick
**Status:** aprovada pela equipe em 24/11/2026

> **Nota de método.** Esta especificação foi formalizada depois de um protótipo funcional. Ela descreve o comportamento que a equipe aprova como correto para a versão entregue. Se a validação revelar divergência entre um critério e o sistema, a correção entra primeiro por aqui e só depois no código.

> **Conteúdo da equipe.** A evidência do problema (seção 1.2), a constituição e a aprovação foram fornecidas pela equipe; a IA ajudou a redigir o restante.

---

## 1. Contexto e problema

### 1.1 Gargalo

Quem precisa de um pequeno reparo doméstico (vazamento, tomada queimada, móvel para montar) hoje recorre a indicações de conhecidos e a contatos soltos de WhatsApp. O cliente não sabe quem está disponível, não consegue comparar preço nem reputação e espera retornos que podem não chegar. Do lado de quem intermedeia o serviço, os pedidos chegam por vários canais (ligação, WhatsApp, formulário), sem registro único. Assim, pedidos se perdem, horários de profissionais se sobrepõem, o valor cobrado varia de atendente para atendente e não há números para gerir a operação.

### 1.2 Evidência de que o problema é real e observável

**Contexto público.** O mercado de pequenos reparos domésticos é grande e cresce, mas ainda é muito informal. Segundo levantamento do Sebrae, o número de profissionais formalizados como MEI na área de reparos domésticos cresceu 22% entre 2016 e 2018, com 185 mil MEIs nessa categoria só no estado de São Paulo (Jornal Contábil, "Maridos de aluguel: alta da profissão em meio ao desemprego"). Trabalhos acadêmicos que propõem plataformas para esse mesmo mercado descrevem o mesmo gargalo: clientes dependem de indicações informais e não conseguem comparar preços e qualificações (Castor Soluções, Fatec, 2025; ProFinder, Fatec).

**Caso observado pela equipe.** Em Recife, um familiar de um integrante da equipe precisou contratar um encanador após identificar um vazamento em uma tubulação da residência. A primeira tentativa foi por indicação de conhecidos, mas o contato indicado levou cerca de dois dias para responder e, quando respondeu, informou que não poderia fazer o serviço naquela semana. Diante da urgência, a família procurou outros profissionais pela internet e por novas indicações. Foram alguns dias até encontrar alguém disponível, e os valores apresentados pelos diferentes profissionais variaram de forma significativa.

**Como o caso se liga ao sistema.** Cada dificuldade observada corresponde a uma parte da solução:

| Dificuldade observada | Resposta do sistema |
|---|---|
| Demora de dias para obter resposta | Protocolo imediato com posição na fila e tempo estimado de retorno (RF-01); alerta à gerência quando alguém espera mais de 15 minutos (RF-23) |
| Profissional indicado sem agenda na semana | Busca apenas profissionais disponíveis e sem conflito no horário escolhido (RF-06) |
| Urgência sem prioridade | Triagem com nível de urgência definido pelo atendente (RF-05) |
| Valores muito diferentes entre profissionais | Preço calculado por regra fixa e mostrado antes da confirmação (RN-01 a RN-04, RF-08) |
| Incerteza sobre confiabilidade | Nota média, número de avaliações e experiência exibidos na escolha (RF-06, RF-17) |

### 1.3 Objetivo do sistema

Registrar e processar, em um único lugar, cada transação entre cliente e profissional, do primeiro contato até a avaliação. O sistema deve garantir agenda sem conflito, preço calculado por regra fixa, histórico completo de cada pedido e indicadores para a gerência.

---

## 2. Usuários

| Usuário | Quem é | O que faz no sistema |
|---|---|---|
| **Cliente** | Morador do Recife com um problema doméstico | Pede atendimento pelo formulário, consulta o andamento e avalia o profissional |
| **Atendente da central** | Funcionário da ResolveJá | Atende a fila e os contatos por ligação e WhatsApp, faz a triagem, agenda, registra conclusão e cancelamento e envia avisos |
| **Profissional** | Eletricista, encanador, montador, técnico ou pintor parceiro | Cadastra-se, aceita ou recusa ordens de serviço e informa a conclusão |
| **Gerente** | Responsável pela operação | Aprova cadastros, controla a disponibilidade e acompanha indicadores e alertas |

---

## 3. Escopo

O sistema cobre, em quatro visões (Cliente, Central, Profissional e Gerente):

1. Entrada de pedidos pelo formulário do cliente (fila de espera com protocolo) e por ligação ou WhatsApp registrados pela central.
2. Triagem do problema pelo atendente: especialidade, urgência e horas estimadas, com apoio de uma tabela-guia.
3. Busca de profissionais livres, sem conflito de agenda, ordenados por proximidade, avaliação, experiência e preço.
4. Cálculo do valor por regra fixa (Seção 6) e pagamento **simulado** (Pix, crédito, débito ou dinheiro).
5. Criação da transação com código único `RJ-000000` e histórico de eventos classificados em Entrada, Processamento e Saída.
6. Ordem de serviço ao profissional, com aceite, recusa e troca de profissional.
7. Conclusão, cancelamento com estorno simulado e documentos (confirmação, comprovante, recibo, comprovante de cancelamento).
8. Avaliação do profissional pelo cliente, com recálculo da nota.
9. Mensagens prontas para cliente e profissional (WhatsApp, e-mail ou texto copiado).
10. Cadastro e aprovação de novos profissionais.
11. Painel gerencial, monitoramento de chamadas e alertas.
12. Armazenamento no navegador, com exportação, importação e restauração dos dados de exemplo.

## 4. Fora de escopo

O sistema **não** faz o seguinte, e isso não deve ser cobrado na validação:

- **Análise automática do problema por IA.** A triagem é feita pelo atendente. A IA foi usada para *desenvolver* o sistema, não faz parte dele.
- **Cobrança real.** Nenhum pagamento é processado; todo valor é simulação, sem valor fiscal.
- **Login com senha e controle de acesso.** As quatro visões ficam abertas a quem acessa o endereço. O profissional entra só com o telefone.
- **Sincronização entre aparelhos.** Os dados ficam no navegador de quem usa. Um pedido feito em um celular **não** aparece na central aberta em outro computador. A demonstração deve ser feita em um único navegador.
- **Envio automático de mensagens.** O sistema prepara o texto e abre o WhatsApp ou o e-mail; quem envia é o atendente.
- **Atendimento fora do Recife** ou em bairros que não estejam na lista do sistema.
- **Horários fora da grade:** só existem inícios entre 08:00 e 18:00, e nenhum serviço pode terminar depois das 20:00.

## 5. Premissas e restrições

- Funciona em navegador moderno de computador ou celular, sem instalação.
- Os dados de exemplo (24 profissionais, 8 clientes, 14 transações históricas) são fictícios e recriados com datas relativas ao dia de hoje ao usar **Dados → Restaurar**.
- Relação com a constituição (`constituicao.md`):

| Princípio | Requisitos que o garantem |
|---|---|
| P1 — Linguagem simples, em português | Todos (textos das telas, mensagens e documentos); verificado em todos os CAs |
| P2 — Valor calculado e exibido antes da confirmação | RF-08, RF-09, RF-12 (CA-11 a CA-15, CA-19) |
| P3 — Consulta só com código e telefone; profissional vê só as próprias ordens | RF-03, RF-13 (CA-04, CA-05, CA-20) |
| P4 — Só profissional aprovado pelo Gerente; sem dois serviços no mesmo horário | RF-06, RF-19, RF-20, RN-06 (CA-08, CA-28, CA-30) |
| P5 — Toda mudança de situação registrada no histórico | RF-09, RF-11, RF-13, RF-15, RF-16 (CA-14, CA-18, CA-21, CA-23, CA-24) |

---

## 6. Regras de negócio

| Código | Regra |
|---|---|
| **RN-01** | Valor do serviço = preço base do profissional + preço por hora × horas estimadas. |
| **RN-02** | Urgência alta acrescenta 20% sobre o valor do serviço. |
| **RN-03** | Taxa da plataforma = 10% sobre o valor do serviço já com o eventual adicional de urgência. |
| **RN-04** | Pagamento com Pix tem 5% de desconto sobre o total (serviço + urgência + taxa). |
| **RN-05** | Pix e cartões ficam **pagos** na criação do pedido; dinheiro fica **agendado** e é registrado como pago na conclusão. |
| **RN-06** | Um profissional não pode ter dois pedidos ativos (agendado ou pago) com horários sobrepostos no mesmo dia. |
| **RN-07** | Horas estimadas entre 0,5 e 8, em passos de 0,5. |
| **RN-08** | Valores arredondados a centavos em cada etapa do cálculo. |
| **RN-09** | Receita da plataforma = taxa da plataforma − desconto Pix, somada sobre os pedidos concluídos. |

### Situações de um pedido

```
           ┌─────────── Pix / cartão ──────────┐
Criação ───┤                                   ├──► Concluído
           └─── dinheiro ──► Agendado ─────────┤
                                               └──► Cancelado
(Pago e Agendado são "ativos": ocupam a agenda e podem ser concluídos ou cancelados.)
```

---

## 7. Requisitos funcionais (EARS)

Padrões usados: **Ubíquo** ("O sistema deve…"), **Por evento** ("Quando…"), **Por estado** ("Enquanto…"), **Indesejado** ("Se…, então…") e **Opcional** ("Onde…").

### 7.1 Entrada do cliente

**RF-01 (por evento)** — Quando o cliente enviar o formulário "Pedir atendimento" com nome (mín. 3 letras), telefone com DDD (mín. 10 dígitos), bairro, endereço (mín. 5 caracteres), descrição (mín. 10 caracteres) e autorização de uso dos dados marcada, o sistema deve registrar o pedido na fila com situação "Aguardando" e exibir um protocolo `F-0000`, a posição na fila e o tempo estimado de retorno.
**RF-01-E (indesejado)** — Se algum desses campos estiver ausente ou inválido, ou o e-mail informado estiver mal formado, então o sistema não deve registrar o pedido e deve listar cada erro encontrado.

**RF-02 (opcional)** — Onde o cliente anexar arquivos ao formulário, o sistema deve aceitar até 3 arquivos, apenas imagens ou PDF de até 2 MB, e recusar os demais com mensagem.

**RF-03 (por evento)** — Quando o cliente informar um protocolo (`F-0000`) ou código de pedido (`RJ-000000`) junto com o telefone usado no pedido, o sistema deve exibir a situação, o profissional, a data, o endereço e o documento correspondente.
**RF-03-E (indesejado)** — Se o código não existir ou o telefone não corresponder a ele, então o sistema deve exibir mensagem de erro e não mostrar nenhum dado do pedido.

### 7.2 Atendimento na central

**RF-04 (por evento)** — Quando o atendente acionar "Chamar próximo" ou "Atender" na fila, o sistema deve abrir um atendimento `AT-0000` com os dados do formulário preenchidos, marcar o item da fila como "Em atendimento" e iniciar o cronômetro. Quando o atendente acionar "Novo atendimento" (ligação ou WhatsApp), o sistema deve abrir um atendimento em branco. Enquanto houver um atendimento aberto, o sistema não deve abrir outro.

**RF-05 (ubíquo)** — O sistema deve permitir ao atendente definir especialidade, urgência (baixa, média, alta) e horas estimadas (RN-07), e deve oferecer uma tabela-guia de triagem que preenche os três campos com um clique.

**RF-06 (por evento)** — Quando especialidade, urgência, data e horário estiverem definidos, o sistema deve listar somente os profissionais da especialidade marcados como disponíveis e sem pedido ativo sobreposto no horário (RN-06). A lista deve vir ordenada por: mesma zona do cliente primeiro, maior nota, mais avaliações, mais experiência e menor preço estimado.

**RF-07 (indesejado)** — Se o horário escolhido já tiver passado, ou se o serviço terminar depois das 20:00, então o sistema deve avisar o atendente e impedir a confirmação do pedido.

**RF-08 (ubíquo)** — O sistema deve calcular e exibir o detalhamento do valor (preço base, horas, valor do serviço, adicional de urgência, taxa, desconto Pix, total) conforme RN-01 a RN-04 e RN-08, recalculando a cada mudança de profissional, urgência, horas ou forma de pagamento.

**RF-09 (por evento)** — Quando o atendente confirmar o pedido com todos os dados válidos, o sistema deve: cadastrar o cliente ou atualizar o cadastro existente (identificado pelo telefone); gerar um código único `RJ-000000`; reservar o horário do profissional; definir a situação conforme RN-05; e registrar a duração do atendimento.
**RF-09-E (indesejado)** — Se faltar algum dado obrigatório ou o profissional não estiver mais livre, então o sistema não deve criar o pedido e deve listar o que falta no resumo.

**RF-10 (por evento)** — Quando o atendente encerrar um atendimento sem pedido, o sistema deve exigir um motivo da lista e registrar o contato no monitoramento. Se o motivo for "Cliente não atendeu" e o contato veio da fila, o pedido deve voltar para a fila como "Aguardando".

### 7.3 Pedido (transação)

**RF-11 (ubíquo)** — O sistema deve manter, para cada pedido, um histórico em ordem cronológica em que cada evento tem data, hora, texto e a classificação Entrada, Processamento ou Saída.

**RF-12 (por evento)** — Quando um pedido for criado, concluído ou cancelado, o sistema deve preparar a mensagem correspondente ao cliente, com código, serviço, profissional, data, horário e valor, e a ordem de serviço ao profissional. O envio fica disponível por WhatsApp, e-mail (se cadastrado) ou cópia do texto, e cada envio é registrado.

**RF-13 (por evento)** — Quando o profissional aceitar a ordem de serviço, o sistema deve marcar o pedido como "Profissional confirmou" para a central e para o cliente. Quando o profissional recusar, o sistema deve exigir um motivo, registrar a recusa no histórico e exibir à central um aviso para trocar o profissional.

**RF-14 (por evento)** — Quando a central trocar o profissional de um pedido ativo, o sistema deve listar apenas substitutos livres no mesmo horário (RF-06), recalcular o valor e registrar a troca e a diferença de valor no histórico.

**RF-15 (por estado)** — Enquanto o pedido estiver Agendado ou Pago, o sistema deve permitir registrar a conclusão, pela central ou pelo profissional. Na conclusão, o sistema deve mudar a situação para Concluído, registrar o pagamento em dinheiro quando for o caso e emitir o recibo.

**RF-16 (por estado)** — Enquanto o pedido estiver Agendado ou Pago, o sistema deve permitir o cancelamento, com confirmação. No cancelamento, o sistema deve mudar a situação para Cancelado, liberar o horário do profissional, registrar estorno simulado se o pedido estava pago e emitir o comprovante de cancelamento.

**RF-17 (por estado)** — Enquanto o pedido estiver Concluído e sem avaliação, o sistema deve permitir registrar uma nota de 1 a 5 estrelas com comentário opcional, pelo cliente (na consulta) ou pela central. Ao registrar, o sistema deve recalcular a nota média e o total de avaliações do profissional.
**RF-17-E (indesejado)** — Se o pedido não estiver concluído ou já tiver avaliação, então o sistema não deve oferecer o registro de avaliação.

**RF-18 (ubíquo)** — O sistema deve listar todos os pedidos, com busca por código, cliente ou profissional e filtro por situação (Todos, Agendados, Pagos, Concluídos, Cancelados), mostrando a quantidade em cada filtro.

### 7.4 Profissionais

**RF-19 (por evento)** — Quando um profissional enviar o cadastro em "Quero ser parceiro" com dados válidos (nome, telefone, especialidade, bairro, experiência de 0 a 50 anos, preço base, preço por hora, apresentação com mín. 20 caracteres e declaração marcada), o sistema deve gerar um protocolo `C-0000` com situação "Em análise".
**RF-19-E (indesejado)** — Se o telefone já pertencer a um profissional ativo ou a outro cadastro em análise, então o sistema deve recusar o envio com mensagem.

**RF-20 (por estado)** — Enquanto um cadastro estiver em análise, o sistema deve exibir ao gerente uma lista de conferência com 4 itens (telefone, documento, experiência, preço) e só habilitar "Aprovar" quando os 4 estiverem marcados. Na aprovação, o profissional passa a existir como disponível e marcado como "Novo na plataforma". Na recusa, o sistema exige um motivo.

**RF-21 (ubíquo)** — O sistema deve permitir que o gerente, e o próprio profissional na sua área, marque o profissional como disponível ou indisponível. Profissional indisponível não aparece nas buscas (RF-06).

### 7.5 Gerência e dados

**RF-22 (ubíquo)** — O painel do gerente deve exibir: total de pedidos; concluídos; em andamento; cancelados; faturamento dos concluídos; receita da plataforma (RN-09); ticket médio; nota média e número de avaliações; pedidos e faturamento por especialidade; totais por situação e por forma de pagamento.

**RF-23 (ubíquo)** — O sistema deve exibir o monitoramento de contatos (por canal, resultado, hora do dia, taxa de conversão, tempo médio de atendimento, espera na fila), filtrável por período (hoje, 7 dias, tudo), e alertas para: item na fila há mais de 15 minutos; pedido ativo que passou do horário sem conclusão; profissional que não confirmou em 30 minutos ou recusou; especialidade sem profissional disponível; conversão abaixo de 40%.

**RF-24 (ubíquo)** — O sistema deve guardar os dados no navegador e mantê-los após recarregar a página. Também deve permitir exportar tudo, anexos incluídos, para um arquivo; importar um arquivo exportado, substituindo os dados atuais; e restaurar os dados de exemplo, com confirmação.
**RF-24-E (indesejado)** — Se o arquivo importado não for uma exportação válida do ResolveJá, então o sistema deve recusá-lo com mensagem e manter os dados atuais.

**RF-25 (ubíquo)** — O sistema deve identificar todo pagamento, estorno e documento como simulação, sem cobrança real.

---

## 8. Critérios de aceitação (Dado / Quando / Então)

**Preparação comum:** antes de cada bloco de testes, abrir **Dados → Restaurar**. Os códigos e números abaixo valem para os dados de exemplo recém-restaurados. "Daqui a 2 dias" e "amanhã" são relativos ao dia do teste.

### Entrada do cliente

**CA-01** (RF-01) — **Dado** que os dados foram restaurados (2 pessoas na fila), **quando** o cliente preenche o formulário completo e envia, **então** o sistema mostra o protocolo **F-0008**, informa que ele é o **3º da fila** e exibe um tempo estimado de retorno.

**CA-02** (RF-01-E) — **Dado** o formulário em branco, **quando** o cliente clica em "Entrar na fila de atendimento", **então** o sistema lista os erros (nome, telefone, bairro, endereço, descrição, autorização) e nenhum protocolo é gerado.

**CA-03** (RF-02) — **Dado** o formulário do cliente, **quando** ele tenta anexar um arquivo que não é imagem nem PDF (ex.: `.docx`), **então** o sistema recusa o arquivo com mensagem pedindo foto ou PDF de até 2 MB.

**CA-04** (RF-03) — **Dado** o pedido de exemplo **RJ-001024**, **quando** o cliente consulta esse código com o telefone **(81) 98887-0101**, **então** o sistema mostra a situação **Concluído**, o profissional **Carlos Mendes** e o **Recibo** com total de **R$ 156,75**.

**CA-05** (RF-03-E) — **Dado** o pedido **RJ-001024**, **quando** o cliente consulta com um telefone diferente (ex.: (81) 90000-0000), **então** o sistema exibe mensagem de erro e não mostra nenhum dado do pedido.

### Atendimento na central

**CA-06** (RF-04) — **Dado** que há pessoas na fila, **quando** o atendente clica em "Chamar próximo", **então** abre um atendimento `AT-…` com os dados de **Paulo Roberto** preenchidos, o cronômetro começa a contar e o item dele sai de "Aguardando". Com o atendimento aberto, não é possível abrir outro.

**CA-07** (RF-05) — **Dado** um atendimento aberto, **quando** o atendente clica na linha "Torneira, registro ou cano vazando sem parar" do guia de triagem, **então** os campos ficam **Encanador**, urgência **Alta** e **1 h**. **E quando** ele digita 10 no campo de horas, **então** o valor fica limitado a **8**.

**CA-08** (RF-06, RN-06) — **Dado** que **Carlos Mendes** tem o pedido RJ-001021 daqui a 2 dias das 10:00 às 11:30, **quando** o atendente busca Encanador para daqui a 2 dias às **10:00**, **então** Carlos **não** aparece na lista. **E quando** muda para as **14:00** do mesmo dia, **então** Carlos aparece.

**CA-09** (RF-06, RF-21) — **Dado** um cliente do bairro **Boa Vista** (Centro) e Encanador, urgência média, amanhã às 14:00, **quando** a lista é exibida, **então** a ordem é **Carlos Mendes** (com o selo "Mesma zona"), **Antônio Barros**, **Roberta Lima**, **José Ferreira**, e **Fábio Nunes** (indisponível) não aparece.

**CA-10** (RF-07) — **Dado** um atendimento de Montador com **3 h**, **quando** o atendente escolhe o horário **18:00**, **então** o sistema avisa que o serviço terminaria às **21:00**, depois do limite das 20:00, e não lista profissionais. **E dado** o dia de hoje, horários que já passaram aparecem desabilitados com "(já passou)".

**CA-11** (RF-08, RN-01 a RN-04) — **Dado** Carlos Mendes (R$ 70 + R$ 55/h), 1 h, urgência **alta**, pagamento **Pix**, **então** o detalhamento mostra: serviço **R$ 125,00**; urgência **R$ 25,00**; taxa **R$ 15,00**; desconto Pix **− R$ 8,25**; total **R$ 156,75**.

**CA-12** (RF-08) — **Dado** o mesmo cenário do CA-11, **quando** o pagamento muda para **Cartão de crédito**, **então** o desconto fica "—" e o total passa a **R$ 165,00**.

**CA-13** (RF-08) — **Dado** Carlos Mendes, **1,5 h**, urgência **média**, **Dinheiro**, **então** o detalhamento mostra: serviço **R$ 152,50**; urgência "—"; taxa **R$ 15,25**; total **R$ 167,75**.

**CA-14** (RF-09, RN-05) — **Dado** um novo atendimento por ligação para um cliente novo de Boa Vista, triagem pelo guia (torneira), amanhã às 14:00, Carlos Mendes e Pix, **quando** o atendente confirma o pedido, **então** é criado um código **RJ-** com 6 dígitos, a situação é **Pago**, o documento é **Comprovante de pagamento** de **R$ 156,75** e o histórico contém "Novo cliente cadastrado".

**CA-15** (RF-09, RN-05) — **Dado** o mesmo cenário do CA-14 com pagamento em **Dinheiro**, **quando** o pedido é confirmado, **então** a situação é **Agendado**, o pagamento aparece como "A receber na conclusão" e o documento é **Confirmação de agendamento** de **R$ 165,00**.

**CA-16** (RF-09-E) — **Dado** um atendimento com triagem, data e horário preenchidos mas **sem profissional escolhido**, **quando** o atendente clica em confirmar, **então** o resumo mostra "Escolha o profissional." e nenhum pedido é criado.

**CA-17** (RF-10) — **Dado** um atendimento aberto a partir da fila, **quando** o atendente encerra sem pedido com o motivo "Cliente não atendeu (voltar para a fila)", **então** o cliente volta para a fila como **Aguardando** e o contato aparece no monitoramento com o resultado "Voltou para a fila".

### Pedido (transação)

**CA-18** (RF-11) — **Dado** o pedido criado no CA-14, **quando** o atendente abre o pedido, **então** o histórico mostra os eventos em ordem cronológica, cada um marcado como Entrada, Processamento ou Saída, incluindo "Valor calculado: … = R$ 156,75" e "Pagamento simulado aprovado via Pix".

**CA-19** (RF-12) — **Dado** o pedido do CA-14, **quando** o atendente clica em "Enviar por WhatsApp" na confirmação ao cliente, **então** o WhatsApp abre com o texto contendo o código, o profissional, a data e **R$ 156,75**, e a caixa passa a mostrar "Enviado 1×".

**CA-20** (RF-13) — **Dado** o pedido do CA-14 com a ordem enviada a Carlos Mendes, **quando** se entra na área do Profissional como Carlos (demonstração) e se clica em aceitar, **então** a central mostra "Profissional confirmou" no pedido e a consulta do cliente marca a etapa "✓ Profissional confirmou".

**CA-21** (RF-13) — **Dado** um pedido novo aguardando o profissional, **quando** o profissional recusa com o motivo "Já tenho outro compromisso nesse horário", **então** o histórico registra a recusa com o motivo e a central mostra o aviso para **trocar o profissional** do pedido.

**CA-22** (RF-14) — **Dado** o pedido recusado do CA-21, **quando** a central escolhe outro profissional em "Trocar profissional", **então** só aparecem profissionais livres no mesmo horário, o valor é recalculado e o histórico registra "Central trocou o profissional: … → …" com o novo valor.

**CA-23** (RF-15, RN-05) — **Dado** o pedido **RJ-001022** (Agendado, Dinheiro, Juliana Castro), **quando** a central registra a conclusão, **então** a situação muda para **Concluído**, o histórico registra "Pagamento em dinheiro de **R$ 147,40** recebido e registrado" e o documento passa a ser **Recibo**.

**CA-24** (RF-16) — **Dado** o pedido **RJ-001021** (Pago com Pix, Carlos Mendes, daqui a 2 dias às 10:00), **quando** a central cancela e confirma, **então** a situação é **Cancelado**, o pagamento aparece como "Estornado (simulação)", o histórico registra "Estorno simulado de **R$ 159,36** via Pix" e o documento é **Comprovante de cancelamento**. **E quando** se busca Encanador para daqui a 2 dias às 10:00, **então** Carlos volta a aparecer.

**CA-25** (RF-17) — **Dado** o pedido RJ-001022 concluído no CA-23, **quando** o cliente o consulta com o telefone **(81) 98887-0105**, dá **5 estrelas** e envia, **então** a avaliação aparece no pedido e o perfil de **Juliana Castro** passa de **76** para **77 avaliações**.

**CA-26** (RF-17-E) — **Dado** o pedido avaliado no CA-25, **quando** o cliente o consulta de novo, **então** o formulário de avaliação não aparece mais. **E dado** um pedido ainda Pago ou Agendado (ex.: RJ-001023), **então** a consulta não oferece avaliação.

**CA-27** (RF-18) — **Dado** os dados restaurados, **quando** o atendente filtra por **Cancelados** em Pedidos, **então** aparecem exatamente **3** pedidos (RJ-001015, RJ-001018, RJ-001020). **E quando** busca "Carlos", **então** só aparecem pedidos de Carlos Mendes.

### Profissionais

**CA-28** (RF-19) — **Dado** o formulário "Quero ser parceiro" com dados válidos e telefone novo, **quando** o profissional envia, **então** o sistema mostra o protocolo **C-0004** e, na área do profissional com esse telefone, a situação **Em análise**.

**CA-29** (RF-19-E) — **Dado** o formulário de cadastro, **quando** o profissional usa o telefone **(81) 99700-0001** (de Carlos Mendes), **então** o sistema recusa com "Esse telefone já pertence a um profissional da plataforma."

**CA-30** (RF-20) — **Dado** o cadastro **C-0001** (Rodrigo Santana) em análise com 2 de 4 itens marcados, **então** o botão "Aprovar cadastro" está desabilitado. **Quando** o gerente marca os 4 itens e aprova, **então** Rodrigo aparece na lista de Profissionais como disponível e surge na busca de Eletricista com "Novo na plataforma".

**CA-31** (RF-21) — **Dado** Carlos Mendes disponível, **quando** o gerente o marca como indisponível, **então** ele deixa de aparecer na busca de Encanador para qualquer horário. **E quando** o próprio Carlos, na área do profissional, volta a ficar disponível, **então** ele reaparece.

### Gerência e dados

**CA-32** (RF-22, RN-09) — **Dado** os dados recém-restaurados, **quando** o gerente abre o painel, **então** vê: **14** pedidos; **8** concluídos; **3** em andamento; **3** cancelados; faturamento **R$ 1.790,27**; receita da plataforma **R$ 125,77**; ticket médio **R$ 223,78**; nota média **4,71** com **7** avaliações.

**CA-33** (RF-22) — **Dado** o painel do CA-32, **quando** a central conclui o RJ-001022 (CA-23), **então** o painel passa a mostrar **9** concluídos, **2** em andamento e faturamento **R$ 1.937,67**.

**CA-34** (RF-23) — **Dado** que os dados foram restaurados há mais de 10 minutos e ninguém atendeu a fila, **quando** o gerente abre o monitoramento, **então** aparece o alerta de que **Paulo Roberto (F-0006)** espera há mais de 15 minutos. **E quando** todos os encanadores são marcados como indisponíveis, **então** aparece o alerta "Nenhum encanador disponível".

**CA-35** (RF-24) — **Dado** um pedido criado (ex.: CA-14), **quando** a página é recarregada (F5) no mesmo navegador, **então** o pedido continua na lista com a mesma situação.

**CA-36** (RF-24) — **Dado** um pedido criado, **quando** se usa **Dados → Exportar**, depois **Restaurar** e depois **Importar** o arquivo baixado, **então** o pedido criado volta a aparecer com o histórico completo.

**CA-37** (RF-24) — **Dado** pedidos de teste criados, **quando** se usa **Dados → Restaurar** e se confirma, **então** os pedidos de teste somem e o painel volta aos números do CA-32.

**CA-38** (RF-24-E) — **Dado** um arquivo qualquer que não é exportação do sistema (ex.: um `.json` ou `.txt` criado à mão), **quando** ele é importado, **então** o sistema exibe "Esse arquivo não é uma exportação válida do sistema ResolveJá." e os dados atuais continuam iguais.

**CA-39** (RF-25) — **Dado** qualquer pedido, **quando** se vê a etapa de pagamento do atendimento ou um documento do pedido, **então** aparece "Pagamento simulado: nenhum valor real é cobrado." ou "Documento de simulação, sem valor fiscal."

---

## 9. Matriz de cobertura (requisito → critério)

| Requisito | Critérios |
|---|---|
| RF-01 / RF-01-E | CA-01, CA-02 |
| RF-02 | CA-03 |
| RF-03 / RF-03-E | CA-04, CA-05 |
| RF-04 | CA-06 |
| RF-05 | CA-07 |
| RF-06 | CA-08, CA-09 |
| RF-07 | CA-10 |
| RF-08 | CA-11, CA-12, CA-13 |
| RF-09 / RF-09-E | CA-14, CA-15, CA-16 |
| RF-10 | CA-17 |
| RF-11 | CA-18 |
| RF-12 | CA-19 |
| RF-13 | CA-20, CA-21 |
| RF-14 | CA-22 |
| RF-15 | CA-23 |
| RF-16 | CA-24 |
| RF-17 / RF-17-E | CA-25, CA-26 |
| RF-18 | CA-27 |
| RF-19 / RF-19-E | CA-28, CA-29 |
| RF-20 | CA-30 |
| RF-21 | CA-09, CA-31 |
| RF-22 | CA-32, CA-33 |
| RF-23 | CA-34 |
| RF-24 / RF-24-E | CA-35, CA-36, CA-37, CA-38 |
| RF-25 | CA-39 |

A ligação com as tarefas (T-XX) fica em `tarefas.md`, e o resultado de cada critério em `validacao.md`.

---

## 10. Histórico de decisões

| Data | Decisão |
|---|---|
| 17/11/2026 | A equipe definiu em consenso como o trabalho seria conduzido, em reunião sem uso de IA. |
| 24/11/2026 | Especificação redigida com apoio de IA a partir do sistema publicado, revisada e aprovada pela equipe. O caso real da seção 1.2 foi fornecido pela equipe. |

## 11. Aprovação

| Integrante | Aprovou em | Observações |
|---|---|---|
| Guilherme Amorim | 24/11/2026 | De acordo |
| João Víctor Henrique | 24/11/2026 | De acordo |
| Lucas Freitas Mollick | 24/11/2026 | De acordo |
