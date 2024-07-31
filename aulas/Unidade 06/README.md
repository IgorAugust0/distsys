# Unidade 06 - Sincronização

Em sistemas centralizados, o tempo é unívoco, facilitando a sincronização, já em SD, alcançar acordo sobre o tempo é desafiador: cada nó possui seu próprio dispositivo de relógio para manter a noção de tempo. Problemas de sincronização podem causar complicações em aplicações distribuídas sensíveis ao tempo.

**Relógios Físicos:** todos os computadores possuem um circuito para acompanhar o tempo, geralmente um cristal de quartzo, que oscilam em frequências bem definidas, mas podem variar ligeiramente entre diferentes computadores.

- Cada oscilação do cristal decrementa um contador associado, gerando interrupções regulares (clock ticks). Relógios são mantidos em memória CMOS, atualizados a cada interrupção do relógio.

**Sincronização:** devido à disparidade de relógios nos nós, problemas de sincronização surgem em DS, em alguns casos, é necessário sincronizar com relógios físicos externos.

- Referências de relógio físico incluem: Tempo Atômico Internacional (TAI), baseado em relógios de césio 133; Estações de rádio e satélites que transmitem sinais precisos de tempo, como UTC.

**Algoritmos de Sincronização:** têm o objetivo de manter a precisão interna (sincronização entre os nós) e externa (sincronização com relógios físicos).

- Modelos subjacentes incluem: um relógio de software é derivado do relógio de hardware do computador. Uma contagem de ticks de relógio é mantida e usada para sincronização.

Algoritmo de Christian: clientes contatam um servidor de tempo, estimando o atraso de mensagens para calcular a sincronização.

Algoritmo de Berkeley: Um servidor de tempo ativo consulta periodicamente os nós para calcular uma média e ajustar os relógios dos outros nós.

Protocolo de Tempo de Rede (NTP): Pares de servidores estimam o atraso e a sincronização entre si, dividindo-os em estratos para evitar sincronização desnecessária.

→ baseado em uma herarquia de servidores, onde cada nível da hierarquia é denominado de Stratum. Normalmente, são implementados 4 Stratums, onde os servidores do Stratum 1 (de mais alto nível) obtêm sua hora por meio de sinais provenientes de UTC receivers

→ cada par de servidores NTP, seja no mesmo Stratum ou em Stratums diferentes, trocam informações de sincronização. Inicialmente, eles computam o offset entre os nós por meio do Algoritmo de Christian, resultando no valor de teta, o qual serve para indicar a defasagem entre os relógios de ambos os servidores.

→ eles calculam o delta que indica o delay médio entre os servidores, o que serve para ajustar os timestamps durante a sincronização entre eles. A posição na hierarquia determinará qual servidor ajustará o seu relógio ao do outro, onde o servidor de Stratum maior sempre ajusta seu relógio ao servidor de Stratum menor.

---

**RELÓGIOS LÓGICOS:** a sincronização de relógios está relacionada ao tempo, mas pode não ser necessário ter uma conta precisa do tempo real. O que geralmente importa é a ordem em que os eventos ocorrem, não necessariamente o tempo absoluto.

**Relógios Lógicos de Lamport:** propôs relógios lógicos como uma maneira de capturar a ordem dos eventos em sistemas distribuídos. São essencialmente contadores de eventos

- Introduziu a relação **_happens-before_**, indicando a ordem entre eventos. Ela pode ser observada diretamente em eventos internos de um processo e na troca de mensagens entre processos.

**Algoritmo de Lamport:** cada processo mantém um contador local que é incrementado antes de executar um evento. Ao enviar uma mensagem, o emissor anexa o valor atual (**_timestamp_**) do seu contador à mensagem.

- A cada troca de mensagem, caso haja uma defasagem entre os relógios, de forma que o relógio do processo destinatário tenha um “tempo lógico” anterior ao “tempo lógico” do emissor, então o relógio lógico do destinatário é atualizado para o valor do **_timestamp + 1_**.

**Relógios Vetoriais:** com os relógios de Lamport, não podemos determinar a relação causal entre dois eventos apenas comparando seus tempos (relógios lógicos).

- São uma extensão dos relógios lógicos de Lamport que capturam a relação causal entre eventos. Cada processo mantém um vetor de contadores, onde cada entrada representa a contagem de eventos em um processo específico.
- Os relógios vetoriais permitem discernir entre precedência e dependência causal entre eventos.

**Clocks: Hardware x Software x Logical**

**Hardware →** conjunto de componentes físicos, mecânicos e eletrônicos que geram, contam e armazenam clock ticks na forma de um temporizador (timer)

**Software →** são as rotinas e estruturas de dados que, por meio das interrupções (IRQs) geradas pelo hardware clock, mantêm a informações de data e hora atualizadas e disponíveis para os processos de aplicação; sua operação ocorre no contexto do SO.

**Logical** → também implementados via software pelo middleware para suporte aos processos distribuídos, em especial para se controlar a ordenação global de eventos distribuídos.

---

**EXCLUSÃO MÚTUA:** fundamental em SD para garantir que múltiplos processos não acessem simultaneamente os mesmos recursos, com abordagens baseadas em permissão e baseadas em token para alcança-la.

**Permission-based (centralized):** um processo é eleito como coordenador. Quando um processo deseja acessar um recurso compartilhado, envia uma mensagem de solicitação ao coordenador. O coordenador concede permissão se nenhum outro processo estiver acessando o recurso. Após o término do uso do recurso, o processo informa ao coordenador para liberá-lo.

- _Shortcoming_: coordenador é SPOF, e pode se tornar um gargalo de desempenho em sistemas grandes.

**Ricart & Agrawala algorithm (distributed):** usa os relógios lógicos de Lamport para ordenar os eventos. Quando um processo deseja acessar um recurso, envia solicitações para todos os outros processos. Os outros processos respondem com "OK" se não estiverem acessando o recurso ou comparam os carimbos de data/hora para determinar qual solicitação tem prioridade, garantindo exclusão mútua sem deadlock ou starvation.

- _Shortcoming_: pode ser afetado por N pontos únicos de falha (SPOF) e pode ser ineficiente em grandes grupos de processos

**Algoritmo do Anel de Tokens:** os processos formam um anel e um token circula entre eles. Apenas o processo com o token pode acessar o recurso compartilhado. Após o uso do recurso, o processo passa o token para o próximo na ordem do anel.

- _Shortcoming_: a perda do token se torna um problema → é preciso um mecanismo para detecção de token perdido, tal como timeout, contudo suceptível a falsos-positivo

Fully decentralized (distributed): envolve um algoritmo de votação. Cada recurso compartilhado é replicado N vezes, com cada réplica tendo seu próprio coordenador de exclusão mútua. Um processo precisa obter a maioria dos votos dos coordenadores para acessar o recurso.

- _Shortcoming_: se uma quantidade f de coordenadores reinicializam, haverá uma violação na votação quando um número m = >N/2 coordenadores concordam em liberar o recurso solicitado

---

**ALGORITMOS DE ELEIÇÃO:** muitos algoritmos distribuídos requerem que um processo atue como coordenador, iniciador ou realize algum papel especial. Em sistemas descentralizados, é essencial ter um coordenador para coordenar certas atividades, como visto em algoritmos de exclusão mútua.

Visão Geral**:** são usados para selecionar um processo especial, como o coordenador, em um grupo de processos. Cada processo tem um identificador único.

**Objetivo**: é garantir que todos os processos concordem sobre quem será o próximo coordenador. Os algoritmos diferem na maneira como localizam o coordenador, mas todos assumem que cada processo tem conhecimento completo do grupo de processos.

**Bully Algorithm:**

1. Quando um processo percebe que o coordenador não está mais respondendo, ele inicia uma eleição enviando uma mensagem _ELECTION_ para todos os processos com identificadores mais altos.
2. Se nenhum processo com identificador maior responder, o processo iniciador vence a eleição e se torna o coordenador.
3. Se um dos processos com identificador mais alto responder, ele assume o papel de coordenador e a eleição termina.

- Múltiplas rodadas → menos eficiente

**Ring Algorithm:** baseado em um anel lógico (rede overlay). Mais eficiente que Bully → uma rodada

- Quando um processo detecta a falha do coordenador, ele inicia uma eleição enviando uma mensagem _ELECTION_ para seu sucessor no anel.
- Se o sucessor estiver inativo, o processo remetente envia a mensagem para o próximo membro ativo do anel. Em cada passo, o remetente adiciona seu próprio identificador à mensagem, tornando-se um candidato à eleição.
- Quando a mensagem retorna ao processo iniciador, ele a reconhece e a transforma em uma mensagem COORDINATOR, informando a todos quem é o novo coordenador. Após a circulação dessa mensagem, todos voltam ao trabalho.
