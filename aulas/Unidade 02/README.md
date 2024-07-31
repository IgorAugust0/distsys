# Unidade 02 - Arquitetura

**Estilos Arquiteturais:** organização lógica dos componentes de software em um sistema

- Implementar efetivamente SDs implica posicionar componentes de software em diferentes localidades físicas (computadores).
- Um estilo arquitetural é formulado em termos de componentes, conexões entre eles, dados trocados e configuração.
- Componentes: unidades modulares com interfaces bem definidas; comunicam-se por conectores, mecanismos para mediação entre componentes (e.g., RPC, passagem de mensagens, streaming de dados).

**Principais Estilos Arquiteturais para SD:**

1. **Arquiteturas em Camadas:** organiza componentes em camadas, permitindo chamadas apenas para camadas subjacentes. Amplamente usado em sistemas cliente-servidor e aplicações distribuídas como motores de busca, corretoras de ações e pacotes de escritório.
2. **Baseadas em Objetos:** cada objeto é um componente, conectado por chamadas de procedimento remoto (método). Facilita a encapsulação de dados e operações, fundamentando arquiteturas orientadas a serviços (SOAs).
   - Proxy e Skeleton: realizam a interceptação de mensagens, serialização do seu conteúdo (marshalling) e envio/recepção desses dados entre objetos remotos em um SD
3. **Baseadas em Eventos:** processos comunicam-se por eventos propagados, geralmente associados a sistemas de publish/subscribe. Vantagem na solução de acoplamento frouxo entre processos.
   - Event bus: propaga os eventos gerados por publishers para todos os subscribers inscritos para aqueles eventos.
4. **Baseadas em Recursos/Dados:** processos comunicam-se através de um repositório comum. Desacoplamento no tempo; visualização do SD como uma coleção de recursos gerenciados por componentes.

**Arquiteturas de Sistema:** localização física dos componentes de software (onde são colocados)

- **Centralizadas: o**rganização em termos de clientes que solicitam serviços de servidores, gerenciando a complexidade dos SDs. Modelos incluem arquiteturas cliente-servidor e em camadas.
- **Descentralizadas:** sistemas P2P:
  - **Estruturadas**: organiza nós em uma rede overlay com uma estrutura específica. Usa operações como LOOKUP(key) para rotear solicitações para o nó associado. Ex.: CDNs.
  - **Não estruturadas:** organiza nós aleatoriamente (random overlay), sem estrutura predefinida. Buscas podem ser não deterministicas, recorrendo a técnicas como flooding (envio de consulta lookup a todos os vizinhos) ou random walk (seleção aleatória de vizinho).
  - **Híbrida**: combinação de client-superpeer e P2P.
  - Arquiteturas de sobreposição (overlay networks) predominam, roteando dados entre nós, com enlaces lógicos em nível de aplicação, são a base para a implementação de sistemas colaborativos distribuídos e VPNs.
- **Híbridas:** combinação de características cliente-servidor e arquiteturas descentralizadas.
  - Exemplos: sistemas de borda (edge-server) para redes de entrega de conteúdo (CDN) e sistemas colaborativos distribuídos como BitTorrent.

**Middleware vs. Arquiteturas:**

- O middleware atua entre aplicações e plataformas distribuídas, buscando transparência de distribuição.
- Desafio: adaptação dinâmica do middleware para estilos arquiteturais específicos, frequentemente facilitada por interceptadores.
