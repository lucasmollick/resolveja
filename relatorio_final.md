# Relatório final — ResolveJá · Central de Atendimento (SPT)

**Disciplina:** Inteligência Empresarial — Profª Drª Jananda Pinto
**Equipe:** Guilherme Amorim, João Víctor Henrique e Lucas Freitas Mollick
**Sistema:** https://lucasmollick.github.io/resolveja/
**Repositório:** https://github.com/lucasmollick/resolveja

> Este relatório foi redigido com apoio de IA a partir dos artefatos e registros do repositório e revisado pela equipe. As decisões e os aprendizados aqui descritos são da equipe.

---

## 1. Problema

Quem precisa de um pequeno reparo doméstico depende de indicações informais, não sabe quem está disponível e não consegue comparar preços. O caso que motivou o trabalho aconteceu em Recife: um familiar de um integrante precisou de um encanador para um vazamento. O contato indicado levou dois dias para responder e não tinha agenda na semana, e foram necessários vários dias e vários orçamentos, com valores bem diferentes, até encontrar alguém (detalhes na seção 1.2 da `especificacao.md`).

O ResolveJá é um **Sistema de Processamento de Transações** que registra cada pedido do primeiro contato até a avaliação, com agenda sem conflito, preço por regra fixa e histórico completo.

## 2. Método aplicado (Spec-Driven Development)

| Passo | O que a equipe fez | Artefato |
|---|---|---|
| 1. Constituir | Cinco princípios redigidos pela equipe, sem IA, e reescritos após a comparação com o sistema | `constituicao.md` |
| 2. Especificar | 25 requisitos em EARS, 39 critérios Dado/Quando/Então, escopo e fora de escopo, evidência do problema | `especificacao.md` (v1.2) |
| 3. Planejar | Ferramenta, estrutura de dados e 9 riscos com mitigação | `plano.md` |
| 4. Decompor | 26 tarefas ligadas a requisitos, critérios e princípios | `tarefas.md` |
| 5. Implementar | Código gerado por IA, uma versão de protótipo e uma versão oficial | `index.html` |
| 6. Validar | Testes manuais da equipe, sem IA, registrados em matriz | `validacao.md` + `evidencias/` |

### Ordem real dos passos

A equipe registra com transparência que **a implementação veio antes da especificação**. O sistema foi construído e publicado primeiro; a especificação, o plano e as tarefas foram formalizados depois, a partir do sistema existente, e então usados como referência para a validação. Na sequência:

| Data | Acontecimento |
|---|---|
| 05/11 a 10/11/2026 | Protótipo construído com IA na plataforma Lovable |
| Novembro/2026 | Versão oficial (`index.html`) gerada com o Claude e publicada no GitHub Pages com orientação do Gemini |
| 17/11/2026 | A equipe definiu em consenso a condução do trabalho e testou as funcionalidades do sistema, sem IA e sem registro formal |
| 24/11/2026 | Constituição, especificação, plano e tarefas formalizados; validação registrada contra os critérios |

Essa inversão é uma limitação do processo (ver seção 7), mas teve um efeito útil: a especificação descreve o que o sistema **de fato** faz, e não o que se imaginava que ele faria.

## 3. Iterações (design science)

O artefato passou por avaliações contra critérios objetivos, e cada uma gerou uma mudança documentada:

| # | O que foi avaliado | O que se descobriu | O que mudou |
|---|---|---|---|
| 1 | Protótipo no Lovable | Dependia de banco online e de serviço de IA externo; o fluxo era de autoatendimento | Versão oficial em arquivo único, com central de atendimento, ordem de serviço ao profissional e quatro visões |
| 2 | Constituição v1 × sistema | Quatro princípios prometiam algo que o sistema não garante (aprovação do valor pelo cliente, controle de acesso, quem aprova o cadastro, situação "execução") | A equipe reescreveu os princípios para que todos fossem verificáveis (v2) |
| 3 | CA-01 na validação | O sistema gerou o protocolo F-0009; a especificação esperava F-0008. A causa era um erro de contagem na especificação | Especificação v1.1 (CA-01 e CA-34 corrigidos); sistema mantido |
| 4 | CA-19 na validação | Com telefone fictício, o WhatsApp não abre a conversa, e o critério não podia ser conferido | Especificação v1.2 (CA-19 reescrito para conferir o texto e o registro de envio dentro do sistema) |

Nas iterações 3 e 4, a correção entrou **pela especificação, e não direto no código**, como o método determina.

## 4. Principais decisões

| Decisão | Motivo |
|---|---|
| Arquivo único em HTML/JavaScript, publicado no GitHub Pages | Sem custo, sem servidor e sem instalação; adequado a uma equipe que não programa profissionalmente |
| Triagem feita pelo atendente, com tabela-guia, e não por IA dentro do sistema | Funciona sem serviço externo; a IA foi usada para **desenvolver** o sistema, e isso ficou declarado como fora de escopo |
| Dados guardados no navegador, com exportar, importar e restaurar | Dispensa banco de dados; a limitação (dados por aparelho) foi declarada e mitigada |
| Uma única função de cálculo de preço | Garante que tela, recibo e mensagem mostrem sempre o mesmo valor (P2) |
| Verificação de conflito de agenda na busca e de novo na confirmação | Garante que nenhum profissional tenha dois serviços no mesmo horário (P4) |
| Todo evento do pedido classificado em Entrada, Processamento e Saída | Torna visível o ciclo do SPT e garante o histórico completo (P5) |

## 5. Resultado da validação

Detalhes em `validacao.md`.

| Situação | Critérios |
|---|---|
| ✅ Conferidos contra o critério e registrados (24/11) | 20 de 39 |
| ❌ Falhou e foi corrigido pela especificação v1.2, aguardando reteste | 1 de 39 (CA-19) |
| 🔸 Funcionalidade testada em 17/11, antes da formalização, sem registro | 18 de 39 |

Os 20 critérios conferidos cobrem o caminho principal da transação: entrada pela fila, triagem, conflito de agenda, as três regras de preço, criação do pedido, histórico, troca de profissional após recusa e documentos de simulação. Nenhum critério conferido revelou defeito no sistema; as duas falhas encontradas eram da especificação.

## 6. Uso responsável da IA

O registro completo está na seção 4 do `tarefas.md`. Em resumo:

- **A IA não decidiu escopo nem aprovou critérios.** Os princípios foram escritos pela equipe; a evidência do problema veio da equipe; a especificação foi aprovada pela equipe.
- **A IA gerou o código** (Lovable no protótipo, Claude na versão oficial), **orientou a publicação** (Gemini) e **redigiu a documentação** a partir do sistema, para revisão da equipe.
- **Nenhum critério foi validado pela IA.** Os testes foram feitos manualmente pela equipe. A IA apenas preparou a matriz em branco e transcreveu os resultados informados, conferindo os prints enviados.

## 7. Limitações

- **Ordem do método:** a especificação foi escrita depois do código, o que reduz o papel dela como guia da implementação.
- **Validação incompleta no registro formal:** 18 critérios foram testados antes de existirem como critérios e não têm registro da época.
- **Ambiente de teste:** a sessão registrada foi feita na cópia local do `index.html`, idêntica à publicada, e não no endereço do GitHub Pages.
- **Sistema:** sem login nas visões Central e Gerente, dados apenas no navegador de cada aparelho, pagamentos simulados e risco de sobrescrita com duas abas abertas (R-09 do `plano.md`).

## 8. Aprendizados

1. **Critério de aceitação precisa ser verificável na prática.** O CA-19 parecia claro no papel, mas dependia de algo fora do sistema (um número real no WhatsApp). Escrever o critério sobre o que o sistema controla evita testes impossíveis.
2. **A especificação também erra, e o teste humano é o que encontra o erro.** O F-0009 do CA-01 só apareceu quando uma pessoa executou o teste e comparou com o esperado.
3. **Princípio inegociável só tem valor se o sistema o garante.** Comparar a constituição com o sistema mostrou que princípios bem-intencionados podiam ser promessas vazias.
4. **Registrar na hora custa menos do que reconstruir depois.** Os testes de 17/11 aconteceram, mas, sem registro, valem menos como evidência do que os de 24/11.
5. **A IA acelera a construção, mas não substitui as decisões.** Ela gerou o código e redigiu os documentos em pouco tempo; o que deu consistência ao trabalho foram as decisões da equipe sobre escopo, princípios e validação.

## 9. Artefatos entregues

| Arquivo | Conteúdo |
|---|---|
| `index.html` | O sistema (SPT) |
| `constituicao.md` | Princípios inegociáveis |
| `especificacao.md` | Contexto, usuários, escopo, requisitos e critérios (v1.2) |
| `plano.md` | Decisões técnicas, dados e riscos |
| `tarefas.md` | Tarefas rastreáveis e registro de uso de IA |
| `validacao.md` | Matriz requisito → critério → tarefa → status |
| `evidencias/` | Prints dos testes |
| `relatorio_final.md` | Este relatório |
| `README.md` | Apresentação do repositório |
