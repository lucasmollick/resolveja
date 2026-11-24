# Tarefas — ResolveJá · Central de Atendimento (SPT)

**Equipe:** Guilherme Amorim, João Víctor Henrique e Lucas Freitas Mollick
**Base:** `especificacao.md` v1.0, `plano.md` e `constituicao.md`
**Papéis:** a IA sugeriu a quebra e a ordem; a equipe revisou e aprovou.

> **Nota de método.** Como o sistema foi construído antes de a especificação ser formalizada (ver `especificacao.md`), esta decomposição foi feita a partir do código já implementado. Cada tarefa corresponde a uma parte real do `index.html` e está ligada aos requisitos (RF), aos critérios de aceitação (CA) e aos princípios (P) que atende. O resultado de cada critério fica em `validacao.md`.

---

## 1. Ordem e dependências

```
T-01 Base ──► T-02 Dados ──► T-03 Preço
                 │
                 ├─► T-04 Formulário ─► T-05 Anexos
                 │
                 ├─► T-06 Atendimento ─► T-07 Triagem ─► T-08 Agenda ─► T-09 Horários ─► T-10 Confirmação ─► T-11 Sem pedido
                 │                                                                          │
                 │                       ┌──────────────────────────────────────────────────┘
                 │                       ├─► T-12 Histórico ─► T-13 Documentos e mensagens
                 │                       ├─► T-14 Conclusão ─► T-15 Cancelamento
                 │                       ├─► T-16 Consulta do cliente ─► T-17 Avaliação
                 │                       └─► T-18 Lista de pedidos
                 │
                 ├─► T-19 Cadastro de profissional ─► T-20 Análise pelo Gerente ─► T-21 Área do profissional ─► T-22 Troca
                 │
                 └─► T-23 Painel ─► T-24 Monitoramento e alertas ─► T-25 Exportar, importar e restaurar ─► T-26 Publicação
```

## 2. Lista de tarefas

| Tarefa | Descrição | Requisitos | Critérios | Princípios | Depende de | Situação |
|---|---|---|---|---|---|---|
| **T-01** | Estrutura base: arquivo único, navegação por rotas entre as visões Cliente, Central, Profissional e Gerente, layout para computador e celular, textos em português, valores em reais e datas no formato brasileiro | Todos | Todos | P1 | — | Implementada |
| **T-02** | Armazenamento no navegador (`salvar`/`carregar`) e geração dos dados de exemplo (24 profissionais, 8 clientes, 14 pedidos, fila e cadastros) | RF-24 | CA-32, CA-35 | — | T-01 | Implementada |
| **T-03** | Função única de cálculo de valor: serviço, urgência, taxa, desconto Pix e arredondamento | RF-08, RN-01 a RN-04, RN-08 | CA-11, CA-12, CA-13 | P2 | T-02 | Implementada |
| **T-04** | Formulário "Pedir atendimento" com validações, entrada na fila, protocolo `F-0000`, posição e tempo estimado | RF-01, RF-01-E | CA-01, CA-02 | P1 | T-02 | Implementada |
| **T-05** | Anexos: até 3 arquivos, imagens reduzidas, PDF até 2 MB, recusa de outros tipos, gravação no `IndexedDB` | RF-02 | CA-03 | — | T-04 | Implementada |
| **T-06** | Abertura de atendimento pela fila ou por ligação/WhatsApp, com cronômetro e bloqueio de um segundo atendimento | RF-04 | CA-06 | — | T-02 | Implementada |
| **T-07** | Triagem: especialidade, urgência e horas (0,5 a 8), com guia de triagem clicável | RF-05, RN-07 | CA-07 | — | T-06 | Implementada |
| **T-08** | Verificação de conflito de agenda e busca de profissionais livres, ordenada por zona, nota, avaliações, experiência e preço | RF-06, RN-06 | CA-08, CA-09 | P4 | T-07 | Implementada |
| **T-09** | Validação de data e horário: horários passados desabilitados e bloqueio de serviço que termina após 20:00 | RF-07 | CA-10 | — | T-08 | Implementada |
| **T-10** | Confirmação do pedido: validações, cadastro ou atualização do cliente, código `RJ-000000` único, reserva de horário, situação Pago ou Agendado | RF-09, RF-09-E, RN-05 | CA-14, CA-15, CA-16 | P2, P4, P5 | T-03, T-09 | Implementada |
| **T-11** | Encerrar atendimento sem pedido, com motivo e retorno à fila quando o cliente não atendeu | RF-10 | CA-17 | — | T-06 | Implementada |
| **T-12** | Histórico do pedido com eventos classificados em Entrada, Processamento e Saída | RF-11 | CA-18 | P5 | T-10 | Implementada |
| **T-13** | Documentos conforme a situação (confirmação, comprovante, recibo, cancelamento) e mensagens prontas ao cliente e ao profissional, com registro de envio e aviso de simulação | RF-12, RF-25 | CA-19, CA-39 | P1, P2 | T-12 | Implementada |
| **T-14** | Registro de conclusão pela central, com pagamento em dinheiro registrado e recibo | RF-15, RN-05 | CA-23 | P5 | T-10 | Implementada |
| **T-15** | Cancelamento com confirmação, liberação do horário e estorno simulado | RF-16 | CA-24 | P4, P5 | T-14 | Implementada |
| **T-16** | Consulta do cliente por código e telefone, com etapas do pedido e documento | RF-03, RF-03-E | CA-04, CA-05 | P3 | T-10 | Implementada |
| **T-17** | Avaliação de 1 a 5 estrelas pelo cliente ou pela central, só após conclusão, sem duplicidade, com recálculo da nota | RF-17, RF-17-E | CA-25, CA-26 | — | T-14, T-16 | Implementada |
| **T-18** | Lista de pedidos com busca e filtro por situação | RF-18 | CA-27 | — | T-10 | Implementada |
| **T-19** | Cadastro "Quero ser parceiro" com validações, protocolo `C-0000` e bloqueio de telefone repetido | RF-19, RF-19-E | CA-28, CA-29 | P4 | T-02 | Implementada |
| **T-20** | Análise de cadastros pelo Gerente (lista de conferência, aprovação e recusa) e controle de disponibilidade dos profissionais | RF-20, RF-21 | CA-30, CA-31 | P4 | T-19 | Implementada |
| **T-21** | Área do profissional: entrada pelo telefone, apenas as próprias ordens, aceite, recusa com motivo, conclusão e disponibilidade própria | RF-13, RF-15, RF-21 | CA-20, CA-21, CA-31 | P3, P5 | T-20, T-10 | Implementada |
| **T-22** | Troca de profissional com busca de substitutos livres e recálculo do valor | RF-14 | CA-22 | P4, P5 | T-21 | Implementada |
| **T-23** | Painel do Gerente: totais, faturamento, receita, ticket médio, nota média, gráficos por especialidade, situação e forma de pagamento | RF-22, RN-09 | CA-32, CA-33 | — | T-14 | Implementada |
| **T-24** | Monitoramento de contatos por período e alertas da operação | RF-23 | CA-34 | — | T-11, T-23 | Implementada |
| **T-25** | Exportar, importar (com recusa de arquivo inválido) e restaurar dados de exemplo | RF-24, RF-24-E | CA-35, CA-36, CA-37, CA-38 | — | T-02 | Implementada |
| **T-26** | Publicação no GitHub Pages | Entrega | — | — | T-01 a T-25 | Concluída |

"Implementada" indica que a tarefa existe no código. Se ela **atende** ao critério, isso é decidido pela equipe na validação (`validacao.md`).

## 3. Verificação de rastreabilidade

- Todos os 25 requisitos (RF-01 a RF-25) estão ligados a pelo menos uma tarefa.
- Todos os 39 critérios (CA-01 a CA-39) estão ligados a pelo menos uma tarefa.
- Os cinco princípios (P1 a P5) estão ligados a pelo menos uma tarefa.
- A única tarefa sem requisito é a T-26 (publicação), que é a entrega do sistema.

---

## 4. Registro de uso de IA

Este registro informa qual assistente de IA foi usado, em quais etapas e o que foi gerado pela IA versus decidido, revisado ou corrigido pela equipe.

### 4.1 Linha do tempo

| Data | Etapa | Ferramenta | O que aconteceu |
|---|---|---|---|
| 05/11 a 10/11/2026 | Protótipo (iteração 1) | Lovable (IA da plataforma) | A partir do enunciado escrito pela equipe, a IA construiu um protótipo com banco de dados online e análise do problema por IA. A equipe enviou o enunciado e pediu a retomada e a continuação do trabalho. A IA testou o próprio protótipo, mas esse teste **não** foi considerado validação da equipe. |
| Novembro/2026 | Implementação (iteração 2) | Claude (Anthropic) | A IA gerou o código do `index.html`: Central de Atendimento com quatro visões, armazenamento no navegador e as mesmas regras de preço. Conversa registrada em https://claude.ai/share/2b91c6fb-d337-4081-ac62-1ca8005aefef |
| 17/11/2026 | Decisão de condução | Nenhuma | A equipe definiu em consenso como o trabalho seria conduzido, sem uso de IA. |
| Novembro/2026 | Publicação | Gemini (Google) | A IA orientou a publicação: renomear o arquivo para `index.html`, criar o repositório público `resolveja`, enviar o código e ativar o GitHub Pages (branch `main`, pasta raiz). Também conferiu a entrega dos links no Google Classroom, gerou um guia de publicação e orientou o uso de **Dados → Restaurar** para limpar dados de teste. Quem executou cada ação no GitHub e no Classroom foi a equipe. |
| 24/11/2026 | Constituição | Claude (apenas revisão) | A equipe redigiu os princípios. A IA comparou a primeira versão com o sistema e apontou quatro divergências; a equipe decidiu e reescreveu. A IA não redigiu nenhum princípio. |
| 24/11/2026 | Especificação | Claude | A IA leu o código e redigiu requisitos (EARS), critérios de aceitação e fora de escopo. A equipe forneceu o caso real da seção 1.2, confirmou o fora de escopo e aprovou. |
| 24/11/2026 | Plano e tarefas | Claude | A IA propôs as decisões técnicas, os riscos e a quebra em tarefas a partir da especificação e do código. A equipe revisou e aprovou. |
| A preencher | Validação | **Nenhuma** | A equipe executa cada critério manualmente no sistema publicado e registra o resultado em `validacao.md`, com prints. |

### 4.2 Divisão de papéis

| Passo do método | Quem decidiu | Papel da IA |
|---|---|---|
| Constituir | Equipe | Nenhum na redação; apontou divergências com o sistema |
| Especificar | Equipe | Redigiu a partir do sistema; a equipe aprovou cada parte e forneceu a evidência do problema |
| Planejar | Equipe, com sugestão da IA | Propôs ferramenta, estrutura de dados e riscos |
| Decompor | Equipe, com apoio da IA | Sugeriu tarefas, ordem e ligações com os requisitos |
| Implementar | IA executou, equipe acompanhou | Gerou o código (Lovable no protótipo, Claude na versão oficial); Gemini orientou a publicação |
| Validar | Equipe | **Nenhum.** Nenhum critério é considerado aprovado sem o teste manual da equipe. |

### 4.3 Limites reconhecidos

- A especificação foi formalizada depois do código. A equipe registrou isso abertamente, em vez de apresentar uma ordem que não aconteceu.
- O código foi integralmente gerado por IA. A garantia de que ele atende à especificação vem da validação manual da equipe, critério por critério.
