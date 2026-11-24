# ResolveJá · Central de Atendimento (SPT)

Sistema de Processamento de Transações para pedidos de pequenos serviços domésticos (encanador, eletricista, montador de móveis, técnico de manutenção e pintor) no Recife.

**▶ Acessar o sistema:** https://lucasmollick.github.io/resolveja/

Trabalho da disciplina de **Inteligência Empresarial** (Profª Drª Jananda Pinto), desenvolvido com a abordagem **Spec-Driven Development**.

**Equipe:** Guilherme Amorim, João Víctor Henrique e Lucas Freitas Mollick

---

## Artefatos

| Arquivo | Conteúdo |
|---|---|
| [`constituicao.md`](constituicao.md) | Princípios inegociáveis do projeto |
| [`especificacao.md`](especificacao.md) | Problema, usuários, escopo, requisitos (EARS) e critérios de aceitação |
| [`plano.md`](plano.md) | Decisões técnicas, estrutura de dados e riscos |
| [`tarefas.md`](tarefas.md) | Tarefas rastreáveis e registro de uso de IA |
| [`validacao.md`](validacao.md) | Relatório de validação (matriz requisito → critério → tarefa → status) |
| [`evidencias/`](evidencias/) | Prints dos testes |
| [`relatorio_final.md`](relatorio_final.md) | Método, decisões, iterações e aprendizados |
| [`index.html`](index.html) | O sistema |

## Como usar

O sistema tem quatro visões, acessadas pelo menu do topo:

- **Cliente:** pedir atendimento pelo formulário e consultar o pedido com código e telefone.
- **Central:** atender a fila e os contatos, fazer a triagem, agendar o profissional e registrar conclusão ou cancelamento.
- **Profissional:** cadastrar-se como parceiro e aceitar, recusar ou concluir as ordens de serviço.
- **Gerente:** aprovar cadastros, controlar a disponibilidade e acompanhar indicadores e alertas.

Antes de testar ou apresentar, use **Dados → Restaurar** para começar com os dados de exemplo.

## Avisos

- **Pagamentos simulados:** nenhum valor real é cobrado.
- **Dados fictícios**, guardados apenas no navegador de quem usa. Um pedido feito em um aparelho não aparece em outro.
- **Sem login:** as visões ficam abertas a quem acessa o endereço.
