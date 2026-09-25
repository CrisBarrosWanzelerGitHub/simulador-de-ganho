# Simulador de Faturamento

Página interativa que mostra, de forma simples e transparente, o ganho de faturamento que uma solução pode gerar para um negócio, comparando o cenário atual com o cenário após a implementação, e gera a proposta em PDF.

**Acesse:** https://crisbarroswanzelergithub.github.io/simulador-de-ganho/

## O que a pessoa cliente consegue fazer

- Simular dois tipos de resultado: **Mais vendas** (aumento de conversão) e **Agenda cheia** (mais horários preenchidos).
- Ajustar os próprios números com controles deslizantes, digitando valores exatos ou fazendo contas, inclusive nos percentuais (`56/80` vira 70%).
- Ver a quantidade de fechamentos ou atendimentos de hoje e a estimativa com a solução.
- Comparar o faturamento antes e depois, com o ganho mensal e no período, sabendo que o cenário com a solução nunca fica abaixo do atual.
- Acompanhar três indicadores: ROI, prazo de retorno e crescimento do faturamento mensal.
- Escolher entre dois formatos de investimento: mensalidade ou suporte eventual por hora.
- Gerar a proposta em PDF, em uma única página A4, com o próprio nome no topo.

## Personalização da proposta

Descrições dos serviços, textos de apoio, preços, quantidades e a validade podem ser editados com um clique na própria página, sem mexer no código.

Cada card tem o próprio botão de reset, no canto inferior direito:

| Botão | O que zera |
|---|---|
| Começar nova simulação | Controles das duas abas e as faixas editadas |
| Restaurar cálculo padrão | Preços, quantidades, descrições e textos da proposta |

Para mudar os padrões que aparecem sempre que a página abre (preços, mínimos, prazos, descrições, textos e e-mail de contato), edite o bloco **CONFIGURAÇÕES EDITÁVEIS**, no início do script em `index.html`.

## Tecnologia

- HTML, CSS e JavaScript puros, em um único arquivo.
- Nenhuma dependência externa: roda 100% no navegador.
- PDF gerado pela própria página, sem bibliotecas, com layout fixo em A4.
- No celular, o PDF abre na tela de compartilhamento do aparelho.
- Layout responsivo, com modo claro e escuro.
- Nenhum dado é coletado, enviado ou armazenado: o navegador guarda apenas a preferência de tema.

## Documentação

O PRD, com requisitos, regras de negócio, variáveis e limites, está em [`docs/PRD.md`](docs/PRD.md).

## Como usar

Acesse a página publicada pelo GitHub Pages ou abra o arquivo `index.html` em qualquer navegador.

## Contato

Cris Barros Wanzeler · crisbarroswanzeler@gmail.com
