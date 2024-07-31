# Unidade 05 - Naming

**1. Introdução:**

- Nomes, endereços e identificadores desempenham papéis essenciais em sistemas de computador, usados para compartilhar recursos, identificar entidades de forma única e referenciar localizações.
- A resolução de nomes permite que um processo acesse a entidade nomeada, sendo fundamental para o funcionamento dos SD.
- Objetivo em SD: permitir que entidades sejam conhecidas e acessadas independente dos seus endereços.

**2. Nomes, Endereços e Identificadores:**

- **Nomes:** Sequências de caracteres usadas para referir-se a entidades.
- **Endereços:** Pontos de acesso das entidades.
- **Identificadores:** Strings de bits que identificam de forma única uma entidade.
- Identificadores verdadeiros são independentes de localização e garantem a referência única a uma entidade.
- IP (192.168.1.1) -> **MAC** (00-1B-63-84-45-E6)
  Host name ([serv01.company.com](http://serv01.company.com/)) -> **IP** (23.214.165.132)
  Service (ftp) -> **Port** (21)

**3. Nomeação Simples (Flat):**

- Para resolução de identificadores (nomes simples, não estruturados, não contendo nenhuma info sobre como localizar endereço da entidade) para endereços.
- **EX:** Nome de Serviço (www, ftp, rpc)
- **Broad/Multicasting:** tipicamente usada em redes do tipo LAN. O host envia uma mensagem de broadcast (para todos os hosts da rede) com a consulta (query) para resolver um dado nome. O host que tem esse nome responde diretamente para o requisitante com o seu endereço Ex: resolução IP->MAC, via protocolo ARP, em que um host deseja obter o MAC address para um dado endereço IP
- **Forwarding Pointers:** quando uma entidade move de um host A para um host B, esta deixa uma referência no host A para o seu novo endereço (B), de forma que todas as consultas de resolução destinando a entidade no host A retornarão o ponteiro para seu novo destino. Nesse método, a resolução de nome ocorre por meio de um processo recursivo.
- **Native Location:** cada host móvel usa um endereço IP fixo. A comunicação é inicialmente direcionada ao home agent (HA) do host móvel. O HA encaminha o tráfego para o endereço temporário (care-of address) do host móvel.
- **Distributed Hash Table (DHT):** Um sistema distribuído que fornece um serviço de lookup. As entidades são mapeadas para chaves em um espaço de identificadores, e cada nó na rede é responsável por uma faixa de identificadores. Exemplo: Chord, onde cada entidade com chave k é gerenciada pelo nó com a menor identificação id ≥ k.
- **Chord:** usa um espaço de identificação de m bits para atribuir identificadores aleatórios aos nós e chaves de entidade. Cada nó mantém uma tabela de dedos (finger table) para rotear consultas. A consulta por uma chave k é direcionada ao nó q com o índice j na finger table do nó p. Ele oferece uma resolução de chave eficiente em O(log N), onde N é o número de nós no sistema.

**4. Nomeação Estruturada:**

- Organização hierárquica de nomes em namespaces. Usa-se um grafo direcionado com nós de diretório e folhas para representar entidades e diretórios. Resolução de nomes inicia-se a partir de um ponto de fechamento conhecido. Aliases e montagem de namespaces permitem a combinação de diferentes namespaces.
- **EX:** Path de um arquivo no file server ([ftp://serv01.company.com/pub/docs/report.pdf](ftp://serv01.company.com/pub/docs/report.pdf))
- **Namespaces:** grafo direcionado com nós de diretório e folhas. Cada caminho no grafo é referido por uma sequência de rótulos de arestas. Resolução de nomes inicia-se a partir de um ponto de fechamento conhecido.
- **Resolução de Nomes:** processo de lookup de informações armazenadas em um nó referido por um nome. Mecanismo de fechamento é necessário para iniciar a resolução.
  - **Método Recursivo:** o cliente solicita a consulta para o servidor de nomes raiz, que resolve o nome consultando os servidores de nomes em sua hierarquia de forma recursiva. O processo continua até que o nome seja resolvido ou uma mensagem de "desconhecido" seja retornada.
  - _Vantagens_: remove a carga de processo das consultas do resolver, pois o servidor de nomes se encarrega disso; simplifica o processo para o resolver, que recebe a resposta do servidor de nomes raiz consultado.
  - **Método Iterativo:** o cliente consulta o servidor de nomes raiz, que pode não resolver diretamente o nome, mas indica um dos seus servidores de nomes imediatamente inferiores ou informa que o nome é desconhecido. O resolver então consulta este servidor, e o processo continua até que o nome seja resolvido ou uma msg de "desconhecido" seja retornada.
  - _Vantagens_: reduz a carga do servidor de nomes, distribuindo-a entre os diferentes clientes (resolvers).
- **Linking:** aliases permitem referenciar uma entidade por vários nomes. Hard link: múltiplos caminhos absolutos para referir o mesmo nó num gráfico de nomes; Symbolic Link: representam uma entidade através de um nó folha, mas em vez de em vez de armazenar o endereço ou o estado dessa entidade, o nó armazena um nome de um caminho absoluto para outro nó.
- **Mounting:** permite a combinação de diferentes namespaces de forma transparente.
- Name resolver nos sistemas de nomeação baseados no protocolo DNS: papel → de middleware de resolução de nomes no lado do cliente. Funcionamento → o resolver realiza as consultas ao servidor DNS, por meio de mensagens nesse protocolo, bem como implementa cache dos nomes resolvidos para fins de otimização das futuras consultas. Cabe ao resolver acordar com o servidor de nomes se as consultas ocorrerão de forma iterativa ou recursiva

**5. Nomeação Baseada em Atributos:**

Os sistemas de nomeação baseados em atributos, também conhecidos como **serviços de diretório**, complementam estruturas de nomeação tradicionais descrevendo e buscando entidades com base em atributos associados. Entidades são vinculadas a atributos como localidade, organização e nome comum, possibilitando gerenciamento de recursos mais flexível e eficiente.

**Atributos Associados:** entidades são descritas com atributos como país, organização e nome comum.

**Busca:** usuários podem buscar entidades com base em valores específicos de atributos, retornando resultados correspondentes.

**Estrutura de Descrição de Recursos (RDF):** atributos podem ser descritos usando RDF, representando recursos como tríplices sujeito-predicado-objeto.

**Armazenamento e Consulta:** descrições de recursos são armazenadas e consultadas de forma eficiente, embora o armazenamento distribuído apresente desafios, muitas vezes exigindo buscas exaustivas por descritores.

**Combinação com Nomeação Estruturada:** combinar a nomeação estruturada com sistemas baseados em atributos, exemplificado pelo LDAP (Lightweight Directory Access Protocol), melhora o gerenciamento de recursos em ambientes distribuídos.

- **EX:** Objeto LDAP (C=BR, O=UFU, OU=FACOM, CN=LaserPrinter01)
