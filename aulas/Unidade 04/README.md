# Unidade 04 - Comunicação

**Camada de Middleware:**

- Middleware fornece serviços e protocolos comuns para várias aplicações.
- Serviços geralmente encontrados nas camadas 5, 6 e 7 do modelo OSI.
- Exemplos: chamadas de procedimento remoto (RPC) (L7), marshaling/demarshaling de dados (L6), protocolos de nomenclatura (L7) e sincronização (L5).

**Tipos de Comunicação:**

- **Transientes:**
  - Mensagem armazenada pelo middleware apenas se ambos remetente e destinatário estiverem em execução (exige acoplamento no tempo).
  - Descartada se a entrega não for possível devido a uma interrupção na transmissão ou destinatário inativo.
- **Persistentes:**
  - Mensagem armazenada até ser entregue ao destinatário. (acoplamento no tempo ñ necessário.
  - Middleware armazena a mensagem em instalações de armazenamento.
- **Síncronas vs. Assíncronas:**
  - **Síncronas:** remetente bloqueado até que a solicitação seja aceita. Três pontos de sincronização: no início, durante a entrega e após o processamento.
  - **Assíncronas:** remetente continua imediatamente após enviar a mensagem. Mensagem temporariamente armazenada pelo middleware.
- **Combinações de Persistência e Sincronização:**
  - Persistência com sincronização na submissão da solicitação e comunicação transitória com sincronização após o processamento da solicitação (RPC).
- **Computação Cliente/Servidor:**
  - Geralmente baseada em comunicação síncrona transitória.
  - Desvantagens: cliente esperando por uma resposta, tratamento imediato de falhas e inadequação para certas aplicações.
- **Middleware Orientado a Mensagens:**
  - Objetivo: alcançar comunicação assíncrona persistente de alto nível.
  - Processos enviam mensagens, que são enfileiradas, permitindo que o remetente realize outras tarefas.
  - O middleware frequentemente garante tolerância a falhas.

**Mecanismos de Comunicação:**

**Chamada de Procedimento Remoto (RPC):**

- Protocolo e mecanismo para comunicação cliente-servidor (comunicação síncrona).
- Objetivo: fazer com que uma chamada de procedimento remoto pareça o máximo possível com uma chamada local.
- Ocorre em etapas envolvendo stubs do cliente e do servidor, troca de mensagens e passagem de parâmetros (wrapping de parâmetros em uma mensagem), considerando encoding/decoding dos valores dos parâmetros dos procedimentos durante uma chamada remota (serialization/marshaling).
- Desafios: garantir a compatibilidade das diferentes representações concretas de dados entre os vários nós de computação envolvidos (máquina de clientes e servidores), como considerando a ordenação de bytes (endianness) de uma palavra na memória do computador (little e big endian) e serialização (valor em uma sequência de bytes).
- **Stub:** código que implementa a interface de todos os procedimentos processados remotamente.
- Intercepta a chamada local, converte os valores dos argumentos de entrada em uma representação abstrata, empacota esses valores e os envia para o servidor, desempacota no servidor, converte a representação abstrata em uma concreta, chama o procedimento local no servidor, recebe a resposta e segue as mesmas etapas na direção oposta (do servidor para o cliente)

**Comunicação Orientada a Mensagens:**

- **Mensagens Transitórias:**
  - Sockets (síncrona e assíncrona) e MPI.
- **Mensagens Persistentes:**
  - Alcançadas por meio de filas em nível de middleware → Brokers, Roteadores, Filas de entrada e saída.
  - Suporta comunicação assíncrona persistente.

**Comunicação Multicast:**

Enviar a mesma mensagem para vários nós.

**Multicast em Nível de Aplicação:**

- Técnicas de multicast em nível de aplicação introduzidas com a tecnologia **P2P**. Redes de sobreposição, multicast baseado em árvore e redes de malha.

**Questões de Desempenho em Redes de Sobreposição:**

- Construção de uma árvore eficiente em redes de sobreposição considerando métricas de desempenho. Métricas: estresse de link, penalidade de atraso relativo e custo de árvore.

**Multicast Baseado em Flooding:**

- Rede de overlay por grupo de multicast para evitar ineficiências. Equivalente à transmissão de uma mensagem para um grupo.

**Multicast Baseado em Gossip:**

- Baseado no comportamento epidêmico e informações locais. Nós classificados como infectados, suscetíveis ou removidos. Modelo de propagação popular: anti-entropia. Modelos de disseminação de informações, incluindo abordagens de push, pull e push-pull.
