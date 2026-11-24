# Plano técnico — ResolveJá · Central de Atendimento (SPT)

**Equipe:** Guilherme Amorim, João Víctor Henrique e Lucas Freitas Mollick
**Base:** `especificacao.md` v1.0 e `constituicao.md`
**Papéis:** a IA propôs as decisões abaixo a partir da especificação; a equipe revisou e aprovou.

---

## 1. Ferramenta e arquitetura

### Decisão

Uma aplicação web em **um único arquivo `index.html`** (HTML, CSS e JavaScript puro, sem bibliotecas externas; apenas as fontes vêm do Google Fonts), publicada gratuitamente no **GitHub Pages**: https://lucasmollick.github.io/resolveja/

### Justificativa

| Critério | Por que esta escolha atende |
|---|---|
| Equipe que não programa profissionalmente | Um arquivo só, sem instalação, sem servidor e sem etapa de compilação. Para publicar, basta enviar o arquivo ao GitHub. |
| Custo zero | GitHub Pages é gratuito e não exige banco de dados pago nem chave de API. |
| Avaliação pelos critérios de aceitação | O slide 9 deixa o caminho livre e avalia o atendimento aos critérios, não a sofisticação técnica. |
| Demonstração em sala | Abre em qualquer navegador de computador ou celular, sem instalar nada. |

### Alternativa considerada (iteração anterior)

Antes desta versão, foi construído um protótipo na plataforma **Lovable** (`resolveja-home-heroes.lovable.app`), com banco de dados online (Supabase) e análise do problema por IA. A equipe adotou o `index.html` como versão oficial. A tabela registra as diferenças:

| Aspecto | Protótipo (Lovable) | Versão oficial (`index.html`) |
|---|---|---|
| Hospedagem | Plataforma Lovable | GitHub Pages |
| Dados | Banco online (Supabase) | Navegador (`localStorage` + `IndexedDB`) |
| Triagem do problema | Automática, por IA | Atendente, com tabela-guia |
| Visões | Cliente e painel | Cliente, Central, Profissional e Gerente |
| Fluxo | Autoatendimento do cliente | Fila + central de atendimento + ordem de serviço ao profissional |

### Organização interna do arquivo

| Parte | Responsabilidade |
|---|---|
| Constantes | Especialidades, bairros e zonas do Recife, urgências, formas de pagamento, situações, grade de horários (08:00 a 18:00, fim até 20:00) e guia de triagem |
| `calcular()` | Única função de preço (RN-01 a RN-04, RN-08). Todas as telas usam esta mesma função. |
| `salvar()` / `carregar()` | Única porta de gravação e leitura dos dados no navegador |
| `ANEXOS` | Guarda fotos e PDFs no `IndexedDB` |
| `montarTransacao()`, `aplicarConclusao()`, `aplicarCancelamento()`, `aplicarAvaliacao()` | Mudanças de situação do pedido, cada uma registrando seus eventos no histórico (P5) |
| `sobrepoe()` / `profsLivres()` | Verificação de conflito de agenda e busca ordenada (RN-06, P4) |
| Visões (`vCliente`, `vCentral`, `vAtendimento`, `vPedido`, `vProfOrdens`, `vGerente`…) | Telas, uma por rota (`#cliente`, `#central`, `#gerente`, `#profissional`…) |

Concentrar o preço em uma única função e as gravações em uma única porta é a principal decisão de projeto: garante que o valor exibido, o valor salvo e o valor da mensagem sejam sempre o mesmo (P2). Também permite, no futuro, trocar o armazenamento local por um banco online mexendo em um só lugar.

---

## 2. Estrutura de dados

Todos os dados ficam em um único objeto, salvo no navegador com a chave `resolveja-central-v1`. Os anexos ficam no banco `resolveja-anexos` do `IndexedDB`.

| Entidade | Campos principais | Identificador |
|---|---|---|
| **Profissional** | nome, especialidade, bairro, zona, preço base, preço por hora, nota inicial, total de avaliações inicial, anos de experiência, apresentação, disponível, telefone | `p01`… |
| **Cliente** | nome, telefone, e-mail, endereço, bairro, zona | telefone (só dígitos) |
| **Item da fila** | nome, telefone, e-mail, bairro, endereço, descrição, preferência de horário, anexos, situação (aguardando, em atendimento, virou pedido, encerrada), tentativas | `F-0000` |
| **Atendimento** | canal (ligação, WhatsApp, formulário), início, fim, duração, espera na fila, resultado (pedido, sem pedido, voltou para a fila), motivo | `AT-0000` |
| **Pedido (transação)** | cliente, endereço, descrição, especialidade, urgência, horas, data, horário, início e fim em minutos, profissional, forma de pagamento, valores, situação, situação do pagamento, ordem de serviço (enviada, confirmada, recusada), avaliação, avisos enviados, anexos, histórico | `RJ-000000` (único) |
| **Evento do histórico** | data e hora, fase (entrada, processamento, saída), texto | dentro do pedido |
| **Avaliação** | nota (1 a 5), comentário (até 400 caracteres), data, origem (cliente ou central) | dentro do pedido |
| **Cadastro de profissional** | dados do candidato, anexos, situação (em análise, aprovado, recusado), lista de conferência (telefone, documento, experiência, preço), motivo, histórico | `C-0000` |
| **Anexo** | nome, tipo, tamanho, a quem pertence | `AX-…` |

**Valores de um pedido:** preço base, preço por hora, horas, valor do serviço, adicional de urgência, subtotal, taxa da plataforma, bruto, desconto Pix e total. Ficam guardados no momento da confirmação, então uma mudança posterior no preço do profissional não altera pedidos já feitos.

**Nota do profissional:** (nota inicial × total inicial + soma das notas recebidas no sistema) ÷ (total inicial + avaliações recebidas).

**Dados de exemplo:** 24 profissionais (5 especialidades, 4 zonas, 2 indisponíveis), 8 clientes, 14 pedidos com datas relativas ao dia atual (8 concluídos, 3 ativos, 3 cancelados), 24 atendimentos, 2 pessoas na fila e 3 cadastros de profissionais.

---

## 3. Riscos identificados

| # | Risco | Impacto | Mitigação adotada |
|---|---|---|---|
| R-01 | Dados ficam só no navegador onde foram criados | Pedido feito no celular não aparece na central do notebook | Declarado fora de escopo; demonstração em um único navegador; exportar e importar para levar os dados a outro aparelho (RF-24) |
| R-02 | Sem login nas visões Central e Gerente | Qualquer pessoa com o link vê os dados gerais | Declarado fora de escopo; dados apenas fictícios; a consulta do cliente exige código e telefone (P3) |
| R-03 | Limite de espaço do navegador (cerca de 5 MB) | Falha ao salvar com muitos anexos | Imagens reduzidas a 1280 px em JPEG; PDF limitado a 2 MB; anexos no `IndexedDB`, separados dos registros; aviso na tela se não for possível salvar |
| R-04 | Dois pedidos para o mesmo profissional no mesmo horário | Conflito de agenda (viola P4) | Checagem de sobreposição na busca **e de novo** no momento da confirmação (RF-06, RF-09-E) |
| R-05 | Agendamento em horário passado ou que termina após 20:00 | Pedido impossível de cumprir | Horários passados desabilitados e confirmação bloqueada (RF-07) |
| R-06 | Erro de arredondamento em centavos | Total diferente entre tela, recibo e mensagem | Arredondamento a centavos em cada etapa, dentro de uma única função de cálculo (RN-08) |
| R-07 | Dados de teste misturados aos de exemplo na apresentação | Painel com números inesperados | **Dados → Restaurar** antes de apresentar (RF-24) |
| R-08 | Perda dos dados ao limpar o navegador | Histórico perdido | Exportação para arquivo como cópia de segurança (RF-24) |
| R-09 | Mesmo sistema aberto em duas abas | A última aba a salvar sobrescreve a outra | **Não mitigado** nesta versão; orientação de usar uma aba só |

---

## 4. Aprovação

| Integrante | Aprovou em |
|---|---|
| Guilherme Amorim | 24/11/2026 |
| João Víctor Henrique | 24/11/2026 |
| Lucas Freitas Mollick | 24/11/2026 |
