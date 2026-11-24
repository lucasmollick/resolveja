# Relatório de validação — ResolveJá · Central de Atendimento (SPT)

**Equipe:** Guilherme Amorim, João Víctor Henrique e Lucas Freitas Mollick
**Sistema validado:** https://lucasmollick.github.io/resolveja/
**Base:** critérios de aceitação da `especificacao.md` v1.2

> **Declaração.** A validação foi feita manualmente pela equipe, **sem apoio de IA** (ambiente de cada sessão indicado abaixo). Cada critério foi executado por uma pessoa da equipe, que conferiu o resultado na tela. Os prints estão em `evidencias/`; onde não há print, a conferência visual está anotada na coluna Observação. A IA preparou a matriz em branco e transcreveu para ela os resultados informados pela equipe, conferindo os prints enviados.

> **Ambiente.** A Sessão A foi executada em 24/11/2026 na cópia local do mesmo `index.html` publicado (aberto a partir do computador), e não no endereço do GitHub Pages. O código é idêntico; as demais sessões devem ser feitas no endereço publicado.

---

## 1. Como testar

1. Usar **um único navegador e uma única aba** em todo o teste (ver R-01 e R-09 no `plano.md`).
2. Seguir as **sessões na ordem**. No início de cada sessão, abrir **Dados → Restaurar** e confirmar.
3. Para cada critério: executar o "Dado / Quando" da especificação, conferir o "Então" e tirar um print.
4. Salvar o print na pasta `evidencias/` com o nome do critério (ex.: `CA-07.jpeg`; se houver mais de um, `CA-24a.jpeg`, `CA-24b.jpeg`). Na matriz, o nome do arquivo vira um link que abre o print no GitHub.
5. Na matriz (seção 3), preencher: **Status** (✅ Passou / ❌ Falhou), **Data**, **Testado por** e, se precisar, **Observação**.
6. **Se falhar:** marcar ❌, descrever o que apareceu na tela e registrar na seção 4. A correção entra primeiro pela especificação, e o critério é testado de novo depois.

---

## 2. Roteiro por sessão

### Sessão A — Cliente, central e criação de pedidos
Começar com **Dados → Restaurar**.

| Passo | Critério | Observação para o teste |
|---|---|---|
| 1 | CA-27 | Antes de criar qualquer coisa |
| 2 | CA-04, CA-05 | Visão Cliente → consulta |
| 3 | CA-01, CA-02, CA-03 | Visão Cliente → formulário. No CA-03, use um arquivo `.docx` ou `.txt` |
| 4 | CA-06, CA-07 | Central → "Chamar próximo" (abre o atendimento de Paulo Roberto) |
| 5 | CA-17 | Encerrar esse mesmo atendimento com "Cliente não atendeu" |
| 6 | CA-10 | Novo atendimento por ligação; Montador, 3 h, 18:00 |
| 7 | CA-08 | No mesmo atendimento: Encanador, daqui a 2 dias, 10:00 e depois 14:00 |
| 8 | CA-09 | Cliente de Boa Vista, Encanador, urgência média, amanhã 14:00 |
| 9 | CA-11, CA-12, CA-13 | Escolher Carlos Mendes e conferir a tabela de valor, mudando urgência, horas e pagamento |
| 10 | CA-16 | Tentar confirmar sem profissional escolhido |
| 11 | CA-14 | Guia "torneira" (alta, 1 h), amanhã **14:00**, Carlos, Pix → confirmar |
| 12 | CA-18, CA-19, CA-39 | No pedido criado no CA-14 |
| 13 | CA-20 | Profissional → entrar como Carlos Mendes (demonstração) → aceitar o pedido do CA-14 |
| 14 | CA-15 | Mesmo cenário do CA-14, com **Dinheiro** e horário **amanhã 16:00** (às 14:00 Carlos já está ocupado pelo CA-14) |
| 15 | CA-21, CA-22 | Como Carlos, **recusar** o pedido do CA-15; depois, na Central, trocar o profissional |
| 16 | CA-35 | Recarregar a página (F5) |
| 17 | CA-36 | Exportar → Restaurar → Importar |

### Sessão B — Conclusão, avaliação, cancelamento e painel
Começar com **Dados → Restaurar**. **Não mexer na fila** nesta sessão (necessário para o CA-34).

| Passo | Critério | Observação para o teste |
|---|---|---|
| 1 | CA-32 | Gerente → painel, logo após restaurar |
| 2 | CA-23 | Central → Pedidos → RJ-001022 → Registrar conclusão |
| 3 | CA-33 | Voltar ao painel |
| 4 | CA-25, CA-26 | Cliente → consulta RJ-001022 com (81) 98887-0105; depois consulta RJ-001023 com (81) 98887-0106 |
| 5 | CA-24 | Cancelar RJ-001021; depois abrir um novo atendimento por ligação e buscar Encanador daqui a 2 dias às 10:00 |
| 6 | CA-34 (1ª parte) | Pelo menos 10 minutos após restaurar: Gerente → monitoramento → alerta de Paulo Roberto |
| 7 | CA-37 | Restaurar e conferir que o painel voltou aos números do CA-32 |

### Sessão C — Profissionais, disponibilidade e dados
Começar com **Dados → Restaurar**.

| Passo | Critério | Observação para o teste |
|---|---|---|
| 1 | CA-28, CA-29 | Profissional → "Quero ser parceiro" |
| 2 | CA-30 | Gerente → Cadastros → C-0001 (Rodrigo Santana) |
| 3 | CA-31 | Gerente → Profissionais → Carlos indisponível; depois, na área do profissional, Carlos volta a ficar disponível |
| 4 | CA-34 (2ª parte) | Marcar todos os encanadores como indisponíveis → alerta "Nenhum encanador disponível" |
| 5 | CA-38 | Importar um arquivo que não seja exportação do sistema |
| 6 | — | **Dados → Restaurar** ao terminar |

---

## 2.1 Testes anteriores à formalização (17/11/2026)

Em 17/11/2026, data inicialmente prevista para a apresentação, a equipe testou manualmente as funcionalidades do sistema, sem uso de IA. Esses testes não foram registrados na época e foram feitos antes de a especificação e os critérios de aceitação existirem (24/11/2026). Por isso, eles aparecem na matriz com o status 🔸. O status ✅ só é usado quando o resultado foi conferido contra o valor esperado do critério formal e registrado.

## 3. Matriz de rastreabilidade e resultado

Legenda de status: ✅ Passou (critério conferido e registrado) · ❌ Falhou · 🔸 Funcionalidade testada pela equipe em 17/11/2026, antes da formalização do critério, sem registro na época · ⬜ Não testado

| Requisito | Critério | Tarefa | Princípio | Resultado esperado (resumo) | Status | Data | Testado por | Evidência | Observação |
|---|---|---|---|---|---|---|---|---|---|
| RF-01 | CA-01 | T-04 | P1 | Protocolo F-0009, 3º da fila | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-01.jpeg](evidencias/CA-01.jpeg) | F-0009 e 3º da fila, conforme especificação v1.1 |
| RF-01-E | CA-02 | T-04 | P1 | Lista de erros, sem protocolo | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-02.jpeg](evidencias/CA-02.jpeg) | |
| RF-02 | CA-03 | T-05 | — | Arquivo .docx recusado com mensagem | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-03 | CA-04 | T-16 | P3 | RJ-001024: Concluído, Carlos, recibo R$ 156,75 | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-04.jpeg](evidencias/CA-04.jpeg) | |
| RF-03-E | CA-05 | T-16 | P3 | Telefone errado: erro, nenhum dado exibido | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-05.jpeg](evidencias/CA-05.jpeg) | |
| RF-04 | CA-06 | T-06 | — | Atendimento AT- com Paulo Roberto, cronômetro, bloqueio de segundo atendimento | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-06.jpeg](evidencias/CA-06.jpeg) | AT-0025 aberto com os dados de Paulo Roberto (F-0007) |
| RF-05 | CA-07 | T-07 | — | Guia: Encanador, Alta, 1 h; horas limitadas a 8 | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-07.jpeg](evidencias/CA-07.jpeg) | Guia preencheu Encanador, Alta, 1 h; valor 10 limitado a 8 |
| RF-06 | CA-08 | T-08 | P4 | Carlos ausente às 10:00, presente às 14:00 | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-06, RF-21 | CA-09 | T-08 | P4 | Ordem: Carlos, Antônio, Roberta, José; sem Fábio | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-09.jpeg](evidencias/CA-09.jpeg) | Teste feito com urgência alta em vez de média; a ordem não depende da urgência |
| RF-07 | CA-10 | T-09 | — | Aviso de término às 21:00; horários passados desabilitados | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-10.jpeg](evidencias/CA-10.jpeg) | Aviso de término às 21:00 confirmado; 2ª parte ("já passou") conferida sem print |
| RF-08 | CA-11 | T-03 | P2 | 125,00 + 25,00 + 15,00 − 8,25 = R$ 156,75 | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-11.jpeg](evidencias/CA-11.jpeg) | |
| RF-08 | CA-12 | T-03 | P2 | Crédito: sem desconto, R$ 165,00 | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-12.jpeg](evidencias/CA-12.jpeg) | |
| RF-08 | CA-13 | T-03 | P2 | 152,50 + 15,25 = R$ 167,75 | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-13a.jpeg](evidencias/CA-13a.jpeg), [CA-13b.jpeg](evidencias/CA-13b.jpeg) | |
| RF-09 | CA-14 | T-10 | P2, P5 | Código RJ-, Pago, comprovante R$ 156,75, "Novo cliente cadastrado" | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-14.jpeg](evidencias/CA-14.jpeg), [CA-18.jpeg](evidencias/CA-18.jpeg) | Pedido RJ-600187. Print tirado depois de uma conclusão registrada por engano; o histórico comprova o pagamento via Pix e o comprovante na criação |
| RF-09 | CA-15 | T-10 | P2, P5 | Agendado, "A receber na conclusão", R$ 165,00 | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-15a.jpeg](evidencias/CA-15a.jpeg), [CA-15b.jpeg](evidencias/CA-15b.jpeg) | Pedido RJ-930328, feito às 14:00 (horário liberado após a conclusão do RJ-600187) |
| RF-09-E | CA-16 | T-10 | — | "Escolha o profissional.", nenhum pedido criado | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-16.jpeg](evidencias/CA-16.jpeg) | |
| RF-10 | CA-17 | T-11 | — | Cliente volta para a fila; monitoramento "Voltou para a fila" | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-17.jpeg](evidencias/CA-17.jpeg) | Paulo Roberto voltou ao fim da fila com "1ª nova tentativa" |
| RF-11 | CA-18 | T-12 | P5 | Histórico cronológico com Entrada/Processamento/Saída | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-18.jpeg](evidencias/CA-18.jpeg) | |
| RF-12 | CA-19 | T-13 | P2 | Caixa de confirmação com código, profissional, data e R$ 156,75; WhatsApp aberto para o cliente; "Enviado 1×" (v1.2) | ❌ | 24/11/2026 | Lucas Freitas Mollick | — | O WhatsApp abriu, mas informou que o número não foi encontrado; o texto não pôde ser conferido no aplicativo. Ver seção 4 |
| RF-13 | CA-20 | T-21 | P3, P5 | "Profissional confirmou" na central e na consulta do cliente | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-13 | CA-21 | T-21 | P5 | Recusa com motivo no histórico; aviso de troca na central | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-21.jpeg](evidencias/CA-21.jpeg) | Recusa com motivo exibida no pedido RJ-930328 |
| RF-14 | CA-22 | T-22 | P4, P5 | Só substitutos livres; valor recalculado; troca no histórico | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-22.jpeg](evidencias/CA-22.jpeg) | Troca para Roberta Lima; valor recalculado R$ 151,80 (−R$ 13,20) |
| RF-15 | CA-23 | T-14 | P5 | RJ-001022 Concluído; dinheiro R$ 147,40 registrado; Recibo | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-16 | CA-24 | T-15 | P4, P5 | RJ-001021 Cancelado; estorno R$ 159,36; Carlos livre de novo | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-17 | CA-25 | T-17 | — | Avaliação 5★ registrada; Juliana de 76 para 77 avaliações | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-17-E | CA-26 | T-17 | — | Sem formulário de avaliação após avaliar ou antes de concluir | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-18 | CA-27 | T-18 | — | Cancelados = 3 (1015, 1018, 1020); busca "Carlos" filtra | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-27a.png](evidencias/CA-27a.png), [CA-27b.png](evidencias/CA-27b.png) | |
| RF-19 | CA-28 | T-19 | P4 | Protocolo C-0004, "Em análise" | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-19-E | CA-29 | T-19 | P4 | Telefone de Carlos recusado com mensagem | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-20 | CA-30 | T-20 | P4 | Aprovar desabilitado até 4 itens; Rodrigo ativo e "Novo na plataforma" | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-21 | CA-31 | T-20, T-21 | — | Carlos some da busca quando indisponível e volta quando disponível | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-22 | CA-32 | T-23 | — | 14 / 8 / 3 / 3; R$ 1.790,27; R$ 125,77; R$ 223,78; 4,71 (7) | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-22 | CA-33 | T-23 | — | 9 concluídos, 2 em andamento, R$ 1.937,67 | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-23 | CA-34 | T-24 | — | Alerta de fila (Paulo Roberto, F-0007) e "Nenhum encanador disponível" | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-24 | CA-35 | T-02 | — | Pedido continua após F5 | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-24 | CA-36 | T-25 | — | Pedido volta após exportar, restaurar e importar | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-24 | CA-37 | T-25 | — | Pedidos de teste somem; painel volta ao CA-32 | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-24-E | CA-38 | T-25 | — | Mensagem de arquivo inválido; dados mantidos | 🔸 | 17/11/2026 | Lucas Freitas Mollick | — | Conferência contra o critério formal pendente |
| RF-25 | CA-39 | T-13 | P2 | Aviso "Pagamento simulado" e "Documento de simulação" | ✅ | 24/11/2026 | Lucas Freitas Mollick | [CA-39.jpeg](evidencias/CA-39.jpeg) | |

---

## 4. Falhas e correções

Preencher apenas se algum critério falhar.

| Critério | O que apareceu | Causa (especificação ou sistema?) | Correção feita | Retestado em | Status final |
|---|---|---|---|---|---|
| CA-01 | Protocolo F-0009 (esperado F-0008); posição "3º da fila" correta | **Especificação.** Os dados de exemplo já têm 8 itens de fila (F-0001 a F-0008); a especificação contou 7 | Especificação corrigida para v1.1: CA-01 espera F-0009 e CA-34 espera F-0007. Sistema não alterado | 24/11/2026 | ✅ |
| CA-19 | Ao clicar em "Enviar por WhatsApp", o WhatsApp abriu e informou que o número não foi encontrado; não foi possível ver a mensagem no aplicativo | **Dados de teste / especificação.** O cliente de teste tinha um telefone fictício, sem conta no WhatsApp. O sistema monta o link para o número cadastrado, mas o critério exige conferir o texto dentro do WhatsApp, o que depende de um número real | Especificação corrigida para v1.2, aprovada pela equipe: com o CA-19 conferindo o texto na caixa "Avisar o cliente" do sistema e o contador "Enviado 1×", e orientação de usar o telefone real de um integrante quando o teste exigir abrir o WhatsApp | | ❌ |
| | | | | | |

---

## 5. Resumo

| Item | Resultado |
|---|---|
| Critérios conferidos e registrados (✅) | 20 de 39 |
| Falharam no teste de 24/11 (❌) | 1 de 39 (CA-19) |
| Funcionalidades testadas em 17/11, sem registro (🔸) | 18 de 39 |
| Passaram | __ |
| Falharam | __ |
| Falharam e foram corrigidos | __ |

**Conclusão da equipe:** _(escrever após os testes)_

| Integrante | Participação na validação | Data |
|---|---|---|
| Lucas Freitas Mollick | Executei testes manuais com evidências na cópia local do sistema (idêntica à publicada), comprovando a aderência à especificação e mapeando os ajustes que levaram às versões 1.1 e 1.2. | 17/11/2026 e 24/11/2026 |
| Guilherme Amorim | Validei o funcionamento geral do SPT nos testes da equipe, confirmando a consistência das regras de negócio e dos princípios da constituição. | |
| João Víctor Henrique | Conferi manualmente o sistema contra a especificação e a constituição, atestando que o SPT está funcional e fiel ao escopo definido. | |
