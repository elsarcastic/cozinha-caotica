# 🍳 Cozinha Caótica — Simulador de Card Game

**Um laboratório de balanceamento para um card game cooperativo de cozinha, inspirado no caos de *Overcooked* e em receitas do cotidiano brasileiro.**

No jogo, os participantes precisam reunir ingredientes e concluir pedidos antes que expirem. O simulador permite testar cardápios, distribuição de cartas, número de jogadores e pressão por rodada sem precisar imprimir um novo protótipo a cada ajuste.

> **Status:** protótipo em desenvolvimento. As regras do jogo físico ainda estão sendo definidas. Os resultados do simulador são estimativas de um modelo automatizado, não previsões de partidas humanas.

## 🚀 Como executar

1. Baixe o arquivo [`simulador_cozinha.html`](./simulador_cozinha.html).
2. Abra o arquivo em um navegador moderno (Chrome, Firefox ou Edge, por exemplo).
3. Configure as receitas, os ingredientes e as regras.
4. Vá até **📈 Simulações** e clique em **▶ Simular**.

O projeto usa **HTML, CSS e JavaScript** em um único arquivo. Não precisa instalar dependências, iniciar servidor ou criar uma conta. O simulador funciona localmente, inclusive sem internet.

## 🧩 Como usar o simulador

### 1. 🍽️ Receitas e cartas

- Escolha o cardápio inicial com **12** ou **18 receitas**.
- Adicione, remova ou renomeie receitas e ingredientes.
- Na tabela, informe quantas unidades de cada ingrediente a receita exige: `0` significa que não usa; `1`, `2` etc. indicam unidades exigidas.
- Configure o **prazo individual** de cada receita na coluna **Prazo (rod.)**. Se ficar vazio, vale o prazo padrão da aba *Regras*.
- Escolha entre **distribuição automática proporcional** de cartas e **distribuição manual**.
- Ajuste a razão entre cartas de ingredientes disponíveis e unidades de ingredientes exigidas pelas receitas (por padrão, `1,5`).

A tabela de ingredientes mostra demanda, frequência nas receitas, quantidade de cartas e participação no baralho. No modo automático, a quantidade total de cartas é arredondada para o inteiro mais próximo.

**Exemplo:** se o cardápio exige 44 unidades de ingredientes, uma razão de `1,5` resulta em 66 cartas de ingredientes.

### 2. ⚙️ Regras

Você pode alterar:

| Parâmetro | O que muda |
| --- | --- |
| Cartas iniciais por jogador | Quantos ingredientes cada jogador recebe no começo |
| Pedidos iniciais | Quantos pedidos começam na mesa |
| Novos pedidos por rodada | Quantos novos pedidos entram a cada rodada |
| Penalidade por pedido perdido | Ativa ou desativa **+1 pedido na rodada seguinte por pedido perdido** |
| Turnos iniciais e crescimento | Quantos turnos há na primeira rodada e quanto aumentam |
| Limite de turnos | Teto para o crescimento dos turnos |
| Prazo padrão | Quantas rodadas um pedido pode permanecer em jogo, salvo prazo individual |
| Limite de pedidos perdidos | Quantos pedidos perdidos provocam derrota |
| Limite de rodadas | Quando uma partida é classificada como inconclusiva |
| Estratégia automatizada | Priorizar urgência, facilidade de conclusão ou combinação das duas |
| Ampulheta | Liga/desliga a restrição de tempo |
| Duração e segundos por ação | Aproximam quantas ações cabem no tempo disponível |
| Reciclagem | Define se os ingredientes descartados voltam ao baralho quando ele acaba |

**Importante:** a ampulheta é simulada como um *orçamento de ações sequenciais*, calculado pela duração dividida pelo tempo médio por ação. O site não mede o tempo de pessoas jogando nem reproduz conversas e movimentos simultâneos.

### 3. 📈 Simulações e relatórios

1. Defina o **número de partidas por configuração**.
2. Selecione o **menor** e o **maior** número de jogadores a testar.
3. Se quiser reproduzir um experimento, mantenha a mesma **semente aleatória** e as mesmas configurações.
4. Clique em **▶ Simular**.

O site mostra as taxas de **vitória, derrota e partidas inconclusivas**, a duração média, os pedidos concluídos e perdidos e o uso dos turnos. Também gera um **relatório automático** que pode ser baixado em `.txt`, além de uma tabela exportável em `.csv`.

**Para investigar partidas:** clique em uma quantidade de jogadores no gráfico ou na tabela de resultados. O painel de analítico permite consultar a evolução média por rodada e abrir uma amostra de **até 40 partidas detalhadas por tamanho de equipe**.

### 4. ⚖️ Comparação A/B: vale adicionar um ingrediente?

1. Monte o cardápio atual e abra **⚖️ Comparar cenários**.
2. Clique em **📌 Salvar cenário A**.
3. Volte a **🍽️ Receitas e cartas** e, por exemplo, adicione *queijo* e inclua-o em algumas receitas.
4. Retorne à comparação, escolha o número de jogadores e de simulações e clique em **⚖️ Comparar A × B atual**.
5. Compare as diferenças de taxa de vitória, derrotas, rodadas e pedidos perdidos.

A comparação mantém **as mesmas regras de partida** e o mesmo número de jogadores nos dois cenários. Para entender o efeito de um ingrediente novo, procure mudar **somente uma variável por vez**.

### 5. 💾 Salvar e compartilhar

- As configurações são salvas automaticamente no armazenamento local do navegador, quando disponível.
- Use **Exportar configuração** para baixar um arquivo `.json`.
- Use **Importar JSON** para carregar uma configuração salva.
- Resultados gerais podem ser exportados em `.csv`, e o relatório automático em `.txt`.

> A persistência local pode variar conforme o navegador, especialmente quando um arquivo HTML é aberto diretamente do computador. Para preservar configurações importantes, exporte o JSON.

## 🎲 Regras do card game

### Ideia central

Um jogo **cooperativo de gerenciamento de pedidos**. Os jogadores trabalham na mesma cozinha, recebem cartas de ingredientes e tentam completar receitas antes de seus prazos acabarem.

### Componentes

- **Baralho de pedidos:** cada carta representa uma receita e os ingredientes necessários para concluí-la.
- **Baralho de ingredientes:** contém as cartas que os jogadores usam para atender aos pedidos.
- **Marcador de rodadas/turnos:** acompanha a progressão do jogo.
- **Ampulheta ou cronômetro (opcional):** cria pressão de tempo durante a rodada.

O cardápio padrão tem **10 tipos de ingrediente**: carne, ovo, bacon, arroz, feijão, pão, batata, tomate, alface e cebola. A carne é genérica para representar diferentes preparações.

### Estrutura desejada do jogo físico

1. Revele uma quantidade inicial de pedidos na mesa.
2. Cada pedido indica seus ingredientes necessários e o prazo para entrega.
3. Os jogadores usam cartas de ingredientes para completar os pedidos coletivamente.
4. A rodada começa com **3 turnos**. Nas rodadas seguintes, a quantidade de turnos pode aumentar em **+1**, conforme a regra definida para a partida.
5. Ao fim de cada rodada, os pedidos não entregues avançam no prazo e podem expirar.
6. Uma nova rodada traz pedidos adicionais; **opcionalmente**, cada pedido perdido também adiciona **+1 novo pedido na rodada seguinte**.
7. Se a ampulheta estiver ativa, os turnos que não couberem no tempo são perdidos.

**Ainda em definição para o jogo físico:** limite de cartas na mão, ações permitidas por turno, possibilidade de trocar ingredientes, destino das cartas usadas, condição exata de vitória/derrota e prazo padrão de cada pedido. O simulador usa valores ajustáveis e convenções provisórias para permitir testes agora.

### Regras provisórias implementadas na simulação

Para que o sistema consiga jogar partidas automaticamente, o modelo atual considera:

- Cada jogador realiza **uma ação por turno**: **entregar um ingrediente da própria mão a um pedido** **ou comprar uma carta**.
- Os pedidos são compartilhados, mas as mãos são individuais; **não há troca de cartas** entre jogadores.
- Os ingredientes entregues são descartados. Conforme a configuração, o descarte pode ser reembaralhado quando o monte de compras acaba.
- Cada receita aparece **uma vez** no baralho de pedidos padrão; o baralho é embaralhado no início da partida.
- O prazo é contado em **rodadas** e os pedidos expirados são removidos da mesa.
- Quando a penalidade está ativa, cada pedido perdido gera **um sorteio adicional de pedido na rodada seguinte**, limitado às cartas ainda existentes no baralho.
- A partida termina em **derrota** ao atingir o limite configurado de pedidos perdidos; em **vitória** quando todos os pedidos do baralho foram resolvidos e não há pedidos ativos antes desse limite; ou como **inconclusiva** ao atingir o limite de rodadas.
- Uma partida pode ser considerada **vencida mesmo tendo perdido alguns pedidos**, desde que não atinja o limite de derrota.

Esse comportamento é **um modelo de teste**, não um regulamento definitivo publicado do jogo físico.

## 🧮 Como funciona o balanceamento

A principal proporção é:

```text
razão = total de cartas do baralho de ingredientes
        ÷ total de unidades de ingredientes exigidas pelas receitas
```

Na configuração inicial, a razão é **1,5**. A distribuição automática reparte as cartas entre os ingredientes proporcionalmente à demanda no cardápio.

Outra métrica útil é a chance de um pedido sorteado pedir determinado ingrediente:

```text
P(ingrediente ser exigido) = receitas que usam o ingrediente
                             ÷ total de receitas
```

Essa conta considera receitas equiprováveis no baralho. Ela **não** representa a chance de comprar a carta, que depende do número de cópias do ingrediente no baralho.

### Dois cardápios de referência

| Configuração | Receitas | Tipos de ingrediente | Demanda total | Cartas com razão 1,5 |
| --- | ---: | ---: | ---: | ---: |
| Protótipo A | 12 | 10 | 44 | 66 |
| Protótipo B | 18 | 10 | 66 | 99 |

O número de receitas **não determina sozinho** o número ideal de jogadores. A pressão depende também das ações disponíveis, dos prazos, do ritmo de novos pedidos, das mãos e da ampulheta.

## 📉 Como interpretar os resultados

- **Vitória alta demais:** talvez existam ações ou prazos demais para a quantidade de pedidos.
- **Muitos pedidos perdidos:** talvez o prazo esteja curto, faltem ingredientes ou a entrada de novos pedidos esteja acelerada.
- **Mais receitas, mesma dificuldade:** aumentar o cardápio pode apenas prolongar a partida se o ritmo dos pedidos permanecer igual.
- **Ingrediente novo piora os resultados:** confira se ele aparece em receitas suficientes e se a oferta acompanha a demanda.

Simulações automatizadas **não substituem playtests**. O modelo não representa perfeitamente negociação, comunicação, erro humano, agilidade e improvisação — justamente aspectos importantes de um jogo de cozinha caótico.

## 🛠️ Tecnologias

- HTML5
- CSS3
- JavaScript (sem frameworks ou dependências externas)
- `localStorage` para configurações locais, quando disponível

## 🗺️ Próximos passos possíveis

- Importação direta de planilhas de receitas (`.csv` / `.xlsx`).
- Regras alternativas de compra, troca e descarte de ingredientes.
- Simulação de jogadores com decisões e estratégias mais variadas.
- Exportação do histórico completo das partidas e novos gráficos de demanda por ingrediente.
- Validação das probabilidades e das regras com playtests físicos.

## 📌 Sobre o projeto

**Cozinha Caótica** é um protótipo experimental de jogo de cartas e de sua ferramenta de balanceamento. A proposta é desenvolver um jogo divertido, colaborativo e progressivamente caótico, usando receitas familiares da culinária brasileira.

Se você quiser contribuir, teste configurações diferentes e compartilhe as regras utilizadas, a semente da simulação e os resultados observados.

----

**Feito para experimentar, analisar e melhorar cada rodada.** 🍳
