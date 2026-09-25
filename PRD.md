# PRD · Simulador de Faturamento (Simulador de ganho)

## 0. Metadados

| Campo | Valor |
|---|---|
| Projeto | Simulador de Faturamento (repositório: `simulador-de-ganho`) |
| Cliente | Uso próprio da Cris, apresentado às pessoas clientes nas propostas comerciais |
| Responsável | Cris Barros Wanzeler · crisbarroswanzeler@gmail.com |
| Tipo | Web app estático (interface visual, arquivo único) |
| URL pública | https://crisbarroswanzelergithub.github.io/simulador-de-ganho/ |
| Versão no Claude | https://claude.ai/artifact/QxMxruHEutoG6SkMKihQtn |
| Repositório | github.com/CrisBarrosWanzelerGitHub/simulador-de-ganho |
| Tags | simulador, ROI, proposta comercial, automação, UX, HTML, sem dependências |
| Status | Em produção |
| Última atualização | 25 de setembro de 2026 (nova simulação, escala dos deslizantes e textos editáveis) |

---

## 1. Objetivo

Permitir que a pessoa cliente visualize, de forma simples e transparente, quanto a solução proposta aumenta o seu faturamento e quanto custa, gerando uma proposta em PDF pronta para envio.

---

## 2. Problema e contexto

### 2.1 Situação atual (antes do projeto)

- O cálculo do ganho e da proposta era feito manualmente, a partir de um exemplo fixo (conversão de 10% para 15%, ticket de R$ 300, plano anual com implementação de 2x o ganho mensal e mensalidade de 20%).
- A pessoa cliente não conseguia testar os próprios números nem enxergar o antes e o depois com clareza.
- A proposta não tinha um formato padronizado para envio.

### 2.2 Limitações que o projeto precisava respeitar

- Nenhuma dependência externa (bibliotecas, fontes, APIs ou servidores).
- Funcionamento 100% no navegador, no computador e no celular.
- Interface minimalista, moderna e em cores neutras, com controles deslizantes.
- Fatores de precificação fora da visão da pessoa cliente (multiplicador e percentual).

### 2.3 O que já foi tentado e descartado durante o projeto

| Tentativa | Motivo do descarte |
|---|---|
| Aba "Do contato ao atendimento" (funil com agendamento, comparecimento e capacidade) | As abas "Mais vendas" e "Agenda cheia" já cumpriam o papel; a terceira aumentava a complexidade |
| PDF pela impressão do navegador | Resultado variava entre navegadores (faixa escura no Chrome do computador e necessidade de reduzir a escala para 80% no celular) |
| Abertura do arquivo pela pré-visualização de arquivos do celular | A pré-visualização não executa JavaScript, então a simulação aparecia zerada; a solução foi publicar a página (Claude e GitHub Pages) e exibir um aviso quando o script não roda |
| Nome e solução editáveis no topo da página | Os campos zeravam a cada carga; o nome passou a ser pedido apenas no momento de gerar o PDF |

---

## 3. Usuários e permissões

| Perfil | O que faz | O que vê | O que edita |
|---|---|---|---|
| Cris (autora e consultora) | Apresenta a simulação, ajusta a proposta e gera o PDF | Toda a página e o painel oculto de ajustes | Todos os controles, preços, quantidades, textos de apoio e o bloco `CONFIG` do código |
| Pessoa cliente | Simula os próprios números e recebe a proposta | Toda a página, exceto o painel de ajustes | Os mesmos controles visíveis da página |

**Observação importante:** não há login nem perfis técnicos. O painel de ajustes é apenas ocultado (atalho **Alt + Shift + C** ou endereço terminado em `#ajustes`), não protegido. Os preços da proposta também são editáveis por qualquer pessoa com o link. Por isso, a proposta oficial é sempre o PDF gerado pela Cris.

---

## 4. Requisitos funcionais

Cada requisito tem um critério de aceitação verificável.

### RF-01 · Seleção do tipo de resultado
Abas "Mais vendas" e "Agenda cheia", com controles próprios e cálculo específico.
**Aceitação:** ao trocar de aba, apenas os controles do modelo escolhido aparecem, e os resultados são recalculados em menos de 100 ms. Valores compartilhados (ticket médio) permanecem iguais nas duas abas.

### RF-02 · Controles deslizantes com edição exata
Cada controle tem deslizante, valor clicável para digitação e limites mínimo e máximo também clicáveis.
**Aceitação:** digitar 5 em "Oportunidades por mês" (mínimo 10) exibe o valor 5, move o início da faixa para 5 e calcula sem erro. Enter confirma e Esc cancela a edição.

### RF-03 · Cenário "com a solução" vinculado
Ao mover o valor atual (conversão ou horários preenchidos), o valor com a solução acompanha com o acréscimo automático.
**Aceitação:** com acréscimo de 3, mover a conversão atual para 20% posiciona a conversão com a solução em 23%. O valor com a solução continua ajustável isoladamente.

### RF-03b · Quantidades nos textos de apoio dos controles
Abaixo dos controles percentuais, o texto de apoio mostra a quantidade mensal resultante, sempre em número inteiro: "Hoje: X" no valor atual e "Estimativa: Y" no valor com a solução.
**Aceitação:** com os valores iniciais, "Conversão atual" mostra "Hoje: 30 fechamentos por mês" e "Conversão com a solução" mostra "Estimativa: 39 fechamentos por mês". Na aba Agenda cheia, os textos mostram "Hoje: 140 atendimentos por mês" e "Estimativa: 146 atendimentos por mês". Os textos se atualizam a cada mudança e também aparecem no PDF.

### RF-03c · Piso do cenário com a solução
O valor com a solução nunca fica abaixo do valor atual, nem pelo deslizante nem por digitação.
**Aceitação:** com "Horários preenchidos hoje" em 65%, tentar definir 50% em "Horários preenchidos com a solução" resulta em 65%. Os dois deslizantes mantêm a mesma escala, para que o cenário com a solução apareça sempre à frente do atual.

### RF-04 · Resultado principal e comparação
Exibe o faturamento adicional no período, o ganho mensal e barras comparando "Hoje" e "Com a solução".
**Aceitação:** com os valores iniciais (300 oportunidades, 10% para 13%, ticket de R$ 300), a tela mostra R$ 9.000 e R$ 11.700 nas barras, "Ganho de + R$ 2.700 por mês" e R$ 32.400 em 12 meses.

### RF-05 · Indicadores
Três caixas: ROI no período, prazo de retorno e crescimento do faturamento mensal, com rótulo à esquerda e número à direita, valores alinhados entre si.
**Aceitação:** com os valores iniciais no modo mensalidade, a tela mostra 186%, 2,5 meses e +30%. Os três números ficam na mesma altura no computador e no PDF.

### RF-06 · Explicação do ROI
A etiqueta "ROI" abre uma explicação com a fórmula e a conta do cenário atual.
**Aceitação:** a explicação abre ao passar o mouse (computador) ou tocar (celular), fica inteira dentro da tela e se atualiza a cada mudança nos controles.

### RF-07 · Proposta com dois formatos de acompanhamento
Seletor entre "Mensalidade" e "Suporte eventual".
**Aceitação:** em "Mensalidade", a proposta mostra implementação e "11 × R$ 540"; em "Suporte eventual", mostra implementação e "11 × R$ 200". O total, o ganho, o ROI e o prazo de retorno mudam conforme o formato.

### RF-08 · Preços, quantidades e contas editáveis
Implementação, mensalidade, valor da hora de suporte, quantidade de mensalidades e quantidade de horas são editáveis com um clique. Os campos de preço aceitam contas.
**Aceitação:** digitar `500*1,20` na implementação resulta em R$ 600; uma conta inválida mantém o valor anterior. Nos controles percentuais, digitar `56/80` resulta em 70%.

### RF-09 · Descrições e textos de apoio editáveis
As descrições dos serviços ("Implementação", "Mensalidade" e "Suporte eventual"), os hints da implementação, da mensalidade e do suporte, a observação da mensalidade, o texto abaixo de "Investimento" e a validade da proposta são editáveis na página.
**Aceitação:** trocar "Implementação" por "Setup e integração com o CRM" atualiza a linha na tela e no PDF daquela proposta. Apagar todo o texto de uma descrição ou de um hint restaura o padrão.

### RF-09b · Nova simulação
Ícone no rodapé do painel "Personalização total", com a dica "Começar nova simulação", que zera apenas os controles da simulação, após confirmação.
**Aceitação:** o clique abre a confirmação "Começar uma nova simulação?"; ao confirmar, voltam ao padrão os controles das duas abas e as faixas editadas. Preços, quantidades e textos da proposta permanecem como estavam. Cancelar mantém tudo como está.

### RF-10 · Restauração do cálculo padrão
Ícone circular com a dica "Restaurar cálculo padrão".
**Aceitação:** o ícone só aparece quando alguma descrição, preço, quantidade ou texto do investimento foi alterado; o clique devolve apenas esses itens aos valores do `CONFIG`, sem alterar os controles deslizantes nem a aba.

### RF-11 · Valores mínimos sem aviso
Quando o preço proporcional fica abaixo de um mínimo, a proposta usa o valor mínimo, sem exibir aviso, para deixar a negociação livre (ver RN-07).
**Aceitação:** na aba Agenda cheia, com os valores iniciais (ganho mensal de R$ 1.800), a mensalidade aparece como R$ 400 e nenhum aviso é exibido na tela ou no PDF.

### RF-12 · Geração do PDF
O botão "Salvar em PDF" pede o nome da pessoa cliente e gera a proposta.
**Aceitação:**
- o botão "Gerar PDF" fica desativado enquanto o nome está vazio;
- o arquivo tem uma única página A4, sempre no tema claro;
- o nome do arquivo segue o padrão "Nome - Simulador Venda Mais.pdf" ou "Nome - Simulador Agenda Cheia.pdf";
- no computador, o arquivo é baixado; no celular, abre a tela de compartilhamento do aparelho; na versão do Claude, usa o download da própria plataforma.

### RF-13 · Validade da proposta
**Aceitação:** a proposta exibe "Proposta válida por 30 dias, até [data]", com a data calculada a partir do dia de abertura da página.

### RF-14 · Tema claro e escuro
**Aceitação:** a página segue o tema do aparelho; o botão de sol ou lua alterna o tema e a escolha é lembrada na próxima visita.

### RF-15 · Aviso quando a página não executa
**Aceitação:** se o JavaScript não rodar (por exemplo, na pré-visualização de arquivos do celular), aparece a orientação para abrir a página em um navegador.

---

## 5. Regras de negócio

| Código | Regra |
|---|---|
| RN-01 | **Mais vendas:** vendas = oportunidades × conversão; faturamento = vendas × ticket médio |
| RN-02 | **Agenda cheia:** atendimentos = horários disponíveis × horários preenchidos; faturamento = atendimentos × valor médio por atendimento |
| RN-03 | **Ganho mensal** = faturamento com a solução − faturamento de hoje. **Ganho no período** = ganho mensal × `mesesDoPlano` |
| RN-04 | **Implementação proporcional** = ganho mensal × `multiplicadorImplementacao`, com piso em `implementacaoMinima` |
| RN-05 | **Mensalidade proporcional** = ganho mensal × `percentualMensalidade` ÷ 100, com piso em `mensalidadeMinima` |
| RN-06 | Preço digitado manualmente é respeitado, mesmo abaixo do mínimo |
| RN-07 | **Sem aviso de inviabilidade:** a aplicação dos mínimos não gera mensagem; a avaliação da viabilidade fica a cargo da negociação |
| RN-08 | **Investimento (mensalidade)** = implementação + mensalidade × `mensalidadesPrevistas` |
| RN-09 | **Investimento (suporte eventual)** = implementação + horas × valor da hora |
| RN-10 | **Ganho no faturamento** = ganho no período − investimento |
| RN-11 | **ROI** = (ganho no período − investimento) ÷ investimento × 100 |
| RN-12 | **Prazo de retorno (mensalidade)** = implementação ÷ (ganho mensal − mensalidade) |
| RN-13 | **Prazo de retorno (suporte)** = (implementação + total de suporte) ÷ ganho mensal |
| RN-14 | **Crescimento do faturamento mensal** = ganho mensal ÷ faturamento de hoje × 100 |
| RN-15 | Se o ganho mensal for zero ou negativo, os resultados ficam esmaecidos e aparece o pedido para ajustar os valores |
| RN-16 | Ao mover o valor atual, o valor com a solução passa a ser o atual + `acrescimoAutomatico`, limitado a 100% |
| RN-16b | O valor com a solução nunca é menor que o valor atual: ao tentar reduzir além disso, ele permanece igual ao atual. Os dois controles mantêm a mesma escala visual |
| RN-17 | Percentuais ficam sempre entre 0% e 100% |
| RN-18 | Valor digitado fora da faixa amplia a faixa: abaixo do mínimo vira o novo mínimo; acima do máximo vira o novo máximo |
| RN-19b | Nos controles percentuais, uma conta cujo resultado fica entre 0 e 1 é lida como fração e convertida em percentual (`56/80` vira 70%); valores acima de 1 são usados como estão (`56/80*100` também vira 70%) |
| RN-19 | Contas nos preços e nos controles aceitam `+`, `-`, `*` (ou `x`, `×`), `/` e parênteses, com números no formato brasileiro; o resultado é arredondado para 2 casas decimais; divisão por zero é recusada |
| RN-20 | Prazo de retorno abaixo de 1 mês aparece como "menos de 1 mês"; sem retorno possível, aparece "Não se aplica" |
| RN-21 | **Nome do PDF:** "[nome da pessoa cliente] - Simulador [Venda Mais ou Agenda Cheia].pdf", sem os caracteres `\ / : * ? " < > \|` |
| RN-22 | **Subtítulo do PDF:** "Soluções Venda Mais" ou "Soluções Agenda Cheia", conforme a aba ativa |
| RN-23 | **Quantidade nos textos de apoio e nas barras:** arredondada para o número inteiro mais próximo; "fechamento" na aba Mais vendas e "atendimento" na aba Agenda cheia, no singular apenas quando o valor é exatamente 1 |

---

## 6. Variáveis e limites

### 6.1 Configurações editáveis (bloco `CONFIG` no código)

Ficam no início do script, entre as marcações **"CONFIGURAÇÕES EDITÁVEIS"** e **"FIM DAS CONFIGURAÇÕES"**. Números sem "R$" e sem ponto de milhar; textos entre aspas.

| Variável | Valor atual | Função |
|---|---|---|
| `multiplicadorImplementacao` | 2 | Implementação = ganho mensal × este número |
| `percentualMensalidade` | 20 | Mensalidade = este percentual do ganho mensal |
| `implementacaoMinima` | 1000 | Piso da implementação proporcional (R$) |
| `mensalidadeMinima` | 400 | Piso da mensalidade proporcional (R$) |
| `valorHoraSuporte` | 200 | Valor de cada hora de suporte eventual (R$) |
| `horasSuportePrevistas` | 11 | Horas de suporte sugeridas ao abrir a página |
| `descricaoImplementacao` | "Implementação" | Descrição do serviço na primeira linha da proposta |
| `descricaoMensalidade` | "Mensalidade" | Descrição do serviço no modo mensalidade |
| `descricaoSuporte` | "Suporte eventual" | Descrição do serviço no modo suporte |
| `hintImplementacao` | "Valor da implementação com período de garantia" | Texto abaixo da implementação; vazio mostra "2x do ganho mensal" |
| `hintMensalidade` | "" | Texto abaixo da mensalidade; vazio não exibe nada |
| `hintSuporte` | "Valor cobrado por hora de suporte" | Texto abaixo do suporte eventual |
| `textoRegraMensalidade` | "Suporte e consultoria conforme contrato" | Observação da mensalidade; vazio oculta |
| `acompanhamentoPadrao` | "mensalidade" | Formato ao abrir: "mensalidade" ou "suporte" |
| `mesesDoPlano` | 12 | Período da simulação e da proposta (meses) |
| `mensalidadesPrevistas` | 11 | Mensalidades cobradas no período |
| `acrescimoAutomatico` | 3 | Pontos percentuais somados ao cenário com a solução |
| `validadeDias` | 30 | Prazo de validade da proposta (dias) |
| `emailContato` | "crisbarroswanzeler@gmail.com" | E-mail do rodapé da página e do PDF |

### 6.2 Controles da simulação

| Controle | Aba | Padrão | Mínimo | Máximo | Passo | Texto de apoio |
|---|---|---|---|---|---|---|
| Oportunidades por mês | Mais vendas | 300 | 10 | 5.000 | 10 | Contatos, leads ou orçamentos |
| Conversão atual | Mais vendas | 10% | 1% | 100% | 0,5 | Hoje: [vendas atuais] fechamentos por mês |
| Conversão com a solução | Mais vendas | 13% | 1% | 100% | 0,5 | Estimativa: [vendas com a solução] fechamentos por mês |
| Horários disponíveis por mês | Agenda cheia | 200 | 10 | 5.000 | 10 | Capacidade total da agenda |
| Horários preenchidos hoje | Agenda cheia | 70% | 1% | 100% | 1 | Hoje: [atendimentos atuais] atendimentos por mês |
| Horários preenchidos com a solução | Agenda cheia | 73% | 1% | 100% | 1 | Estimativa: [atendimentos com a solução] atendimentos por mês |
| Ticket médio / Valor médio por atendimento | Ambas | R$ 300 | R$ 50 | R$ 5.000 | 10 | Sem texto |

**Comportamento dos limites:**
- os limites exibidos podem ser editados com um clique (o mínimo precisa ser menor que o máximo);
- se a faixa passar a começar em um valor fora do passo (por exemplo, 5 com passo 10), o deslizante passa a andar de 1 em 1 nos valores inteiros e em decimais nos percentuais;
- os valores digitados podem ultrapassar a faixa (RN-18), exceto percentuais (RN-17).

### 6.3 Ajustes ocultos (painel Alt + Shift + C)

| Ajuste | Padrão | Mínimo | Máximo | Passo |
|---|---|---|---|---|
| Acréscimo automático | +3 p.p. | 0 | 20 | 0,5 |
| Multiplicador da implementação | x2 | 1 | 3 | 0,5 |
| Mensalidade (% do ganho) | 20% | 10% | 30% | 1 |

Os valores iniciais do painel vêm do `CONFIG`. Alterações no painel valem apenas para a sessão aberta.

### 6.4 Campos editáveis da proposta

| Campo | Padrão | Limites | Aceita contas |
|---|---|---|---|
| Implementação | Proporcional (RN-04) | A partir de 0 | Sim |
| Mensalidade | Proporcional (RN-05) | A partir de 0 | Sim |
| Quantidade de mensalidades | 11 | 1 a 120 (inteiro) | Não |
| Valor da hora de suporte | R$ 200 | A partir de 0 | Sim |
| Quantidade de horas de suporte | 11 | 0 a 99 (inteiro) | Não |
| Descrição da implementação | "Implementação" | Uma linha de texto simples | Não |
| Descrição da mensalidade | "Mensalidade" | Uma linha de texto simples | Não |
| Descrição do suporte eventual | "Suporte eventual" | Uma linha de texto simples | Não |
| Texto abaixo de "Investimento" | Gerado conforme o modo e o período | Uma linha de texto simples | Não |
| Validade da proposta | "Proposta válida por 30 dias, até [data]" | Uma linha de texto simples | Não |
| Hints e observação | Textos do `CONFIG` | Uma linha de texto simples | Não |

### 6.5 Estado interno da página (não persistido)

| Variável | Descrição |
|---|---|
| `model` | Aba ativa: `conv` (Mais vendas) ou `agenda` (Agenda cheia) |
| `leads`, `convA`, `convB` | Oportunidades e conversões |
| `slots`, `occA`, `occB` | Horários disponíveis e preenchimentos |
| `ticket` | Ticket médio ou valor por atendimento |
| `months` | Período da simulação |
| `parcelas` | Quantidade de mensalidades |
| `offset`, `mult`, `rec` | Ajustes do painel oculto |
| `plan` | `mensalidade` ou `apoio` (suporte eventual) |
| `apoios`, `apoioValor` | Horas e valor da hora de suporte |
| `implManual`, `recManual` | Preços digitados (`null` = proporcional) |
| `implLabel`, `recLabel`, `supLabel` | Descrições dos serviços editadas (`null` = padrão) |
| `proposalHint`, `validityText` | Texto do investimento e validade editados (`null` = padrão) |
| `implHint`, `recHint`, `supHint`, `recNote` | Textos editados (`null` = padrão) |

### 6.6 Quando os valores mínimos entram em ação (com o `CONFIG` atual)

| Item | Mínimo aplicado com ganho mensal abaixo de | Origem |
|---|---|---|
| Mensalidade | R$ 2.000 | 20% × R$ 2.000 = R$ 400 |
| Implementação | R$ 500 | 2 × R$ 500 = R$ 1.000 |

Nenhum aviso é exibido nesses casos.

---

## 7. Requisitos não funcionais

| Tema | Requisito |
|---|---|
| Dependências | Nenhuma: HTML, CSS e JavaScript puros em um único `index.html` (cerca de 68 KB) |
| Fontes | Fontes do sistema na tela; no PDF, Helvetica e Times (fontes padrão do formato) |
| Desempenho | Recalcular a tela em menos de 100 ms a cada interação |
| Responsividade | Layout em coluna única até 860px; caixa de ganho empilhada até 430px; margens laterais de 32px no celular; áreas de toque ampliadas em telas sensíveis ao toque |
| Compatibilidade | Navegadores atuais (Chrome, Safari, Edge, Firefox) no computador, iPhone e Android |
| PDF | Uma página A4, gerado pelo próprio simulador, idêntico em qualquer aparelho |
| Tema | Claro e escuro com tokens de cor; o PDF é sempre claro |

---

## 8. Permissões, privacidade e LGPD

- **Sem coleta de dados.** Não há servidor, banco de dados, formulário enviado, cookies ou analytics.
- **Nome da pessoa cliente.** Usado apenas para montar o PDF no próprio aparelho; não é salvo nem transmitido.
- **Armazenamento local.** O navegador guarda somente a preferência de tema (`simulador-tema`).
- **Dados expostos publicamente.** O repositório é público; o único dado pessoal no código é o e-mail de contato da Cris, que já aparece na página.
- **Base legal.** Como não há tratamento de dados pessoais de terceiros pela página, não há necessidade de consentimento ou política de cookies.

---

## 9. Acessibilidade (meta: WCAG 2.1 AA)

**Já implementado:**
- rótulos acessíveis em botões, campos editáveis e controles;
- foco visível em todos os elementos interativos;
- navegação por teclado (Enter e Esc nas edições);
- respeito à preferência de movimento reduzido;
- textos de estado anunciados (`role="status"`).

**Ponto em aberto:**
- a cor dos textos de apoio no tema claro (`#9BA1A6` sobre branco) tem contraste de 2,6:1, abaixo dos 4,5:1 exigidos pelo nível AA para textos pequenos. A cor secundária (`#6A7076`) atinge 5,0:1. Ajuste recomendado: escurecer a cor dos hints para atingir no mínimo 4,5:1.

---

## 10. Segurança

| Item | Situação |
|---|---|
| Autenticação, força bruta, CSRF | Não se aplica: não há login, sessão nem servidor |
| Execução de código | As contas nos preços usam um interpretador próprio, sem `eval` |
| Campos de texto | Colagem convertida em texto simples; nenhum conteúdo é enviado a terceiros |
| Segredos | Nenhuma chave, token ou senha no código |
| Versão no Claude | Página isolada pela plataforma; usa apenas a capacidade de download |
| Painel de ajustes | Apenas oculto, não protegido (ver seção 3) |

---

## 11. Stack técnico e justificativa

| Escolha | Justificativa |
|---|---|
| HTML, CSS e JavaScript puros | Atende ao requisito de zero dependências e roda em qualquer navegador sem instalação |
| Arquivo único | Facilita backup, envio e atualização (um único upload no GitHub) |
| Gerador de PDF próprio | Elimina a variação da impressão do navegador e garante página única A4 |
| GitHub Pages | Hospedagem gratuita, link direto para clientes e histórico de versões como backup |
| Artifact no Claude | Link alternativo com download integrado à plataforma |

---

## 12. Custos

| Item | Custo |
|---|---|
| Hospedagem (GitHub Pages) | R$ 0 |
| APIs e serviços externos | R$ 0 (não utiliza) |
| Domínio próprio | Opcional; custo do registro, se adotado |

---

## 13. Fora do escopo

- Login, contas de usuário e permissões por perfil.
- Salvamento de simulações ou histórico de propostas.
- Envio automático do PDF por e-mail ou WhatsApp.
- Assinatura digital ou aceite da proposta.
- Integração com CRM, planilhas ou meios de pagamento.
- Terceira aba de funil ("Do contato ao atendimento").
- Proteção por senha do painel de ajustes.

---

## 14. Métricas de sucesso (metas propostas, a validar)

| Métrica | Meta |
|---|---|
| Tempo para montar uma proposta durante a reunião | Até 5 minutos |
| Propostas enviadas com o PDF do simulador | 100% das propostas das soluções Venda Mais e Agenda Cheia |
| PDFs com problema de layout | 0 (uma página A4 em computador e celular) |
| Taxa de fechamento das propostas apresentadas com o simulador | Acompanhar mensalmente e comparar com o período anterior |
| Retrabalho de cálculo após a reunião | 0 casos |

---

## 15. Riscos e pontos em aberto

| Risco ou pendência | Impacto | Encaminhamento |
|---|---|---|
| Pessoa cliente alterar preços pelo link | Proposta divergente | Tratar o PDF gerado pela Cris como proposta oficial |
| Contraste dos hints abaixo do AA | Acessibilidade | Ajustar a cor (seção 9) |
| Upload com nome errado no GitHub (ex.: `index (1).html`) | Site não atualiza | Conferir o nome antes do commit |
| Compartilhamento de arquivos indisponível em navegadores antigos | Celular baixa em vez de compartilhar | Comportamento de reserva já implementado |

---

## 16. Manutenção

1. Para mudar preços, mínimos, textos, prazos ou e-mail, edite o bloco `CONFIG` no `index.html`.
2. Para novas melhorias, baixe o `index.html` do repositório e envie ao Claude, garantindo que as mudanças partam da versão no ar.
3. Envie o novo arquivo pelo **Add file > Upload files**, com o nome exato `index.html`.
4. Use mensagens de commit iniciadas por verbo (Adiciona, Ajusta, Corrige, Remove), com até cerca de 70 caracteres.
5. A cada mudança visual, confira o computador, o celular (360px a 430px) e o PDF.

---

## 17. Histórico de decisões

| Decisão | Motivo |
|---|---|
| Duas abas: Mais vendas e Agenda cheia | Cobrem os dois tipos de solução sem redundância |
| Fatores de precificação ocultos, regra visível nos hints | Transparência sem expor a negociação |
| Mínimos de R$ 1.000 e R$ 400, com aviso discreto | Evitar propostas inviáveis |
| Remoção do aviso de inviabilidade, mantendo os mínimos | Deixar a plataforma mais livre para negociação |
| Descrições dos serviços editáveis na proposta | Permitir apresentar qualquer serviço, sem alterar o código |
| Contas nos controles percentuais e quantidades inteiras | Refletir a realidade da operação (56 de 80 horários, sem meio atendimento) |
| Mesma escala nos dois deslizantes do par | Evitar a leitura equivocada de que o cenário com a solução seria menor |
| Dois resets independentes: simulação e investimento | Cada card controla apenas os próprios valores, evitando perder ajustes de preço ao zerar a simulação |
| Suporte eventual cobrado por hora (11 × R$ 200) | Alternativa à mensalidade |
| 11 mensalidades em um período de 12 meses | Período de análise separado da quantidade cobrada |
| PDF próprio, com compartilhamento no celular | Resultado igual em qualquer aparelho e envio direto pelo canal preferido |
| Ícone de restauração no lugar de link | Interface mais limpa, com dica ao passar o mouse |
| Quantidades mensais nos textos de apoio dos controles | A pessoa cliente enxerga, em números concretos, o efeito de cada percentual |
