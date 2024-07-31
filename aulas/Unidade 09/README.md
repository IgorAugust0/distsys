# Unidade 09 - Transações Distribuídas

**Transações FLAT:** os clientes interagem com vários servidores, acessando objetos sequencialmente. cada request é concluída antes de prosseguir para a próxima, permitindo acesso sequencial a objetos de servidor. A concorrência é limitada; uma transação espera por um objeto de cada vez.

Abort se um dos servidores não puder confirmar operação.

**Transações NESTED:** as transações de nível superior podem gerar subtransações (”filhos”), que são executadas em paralelo (concorrentes), em máquinas diferentes, p/ obter ganho de desempenho ou flexibilidade. Subtransações no mesmo nível podem ser executadas concorrentemente, proporcionando maior flexibilidade e concorrência do que as transações planas.

Não é Abortada se um dos servidores não puder confirmar operação (ou em abort de subtransção).

**COMMIT Protocols:** garante que todas as operações de uma transação sejam realizadas ou nenhuma; coordenador solicita a confirmação ou aborto a todos os participantes até que todos respondam.

**Atômico (One-phase):** inadequado → não permite que um servidor aborte unilateralmente uma transação. Problemas de controle de concorrência podem levar ao aborto sem que o cliente saiba.

**Atômico (two-phase):** permite que qualquer participante aborte. 1 — Os participantes votam para confirmar ou abortar. 2 — Se todos votarem para confirmar, a transação é confirmada; se qualquer um votar para abortar, a transação é abortada. Devido à necessidade de atomicidade, se uma parte da transação for abortada, toda a transação deve ser abortada, exemplo:

nodo X retorna resposta negativa na primeira fase → na segunda fase, coord. deve avisar a todos os nodos participante para não completarem a transação.

**CONCURRENCY CONTROL:** cada servidor gerencia um conjunto de objetos e é responsável por garantir sua consistência durante transações concorrentes.

**Lock:** mantidos localmente (serv.) ; somente liberada → confirmada ou abortada; ordem ñ importa.

**DISTRIBUTED DEADLOCKS:** local — deadlocks quando o lock p/ concurrency control. SD — _global wait-for_ pode ser construído a partir dos grafos locais, ñ está em nenhum → deadlock distribuído. **deadlock → ciclo no _wait-for graph_.**

**Detecção:** detecção centralizada de deadlocks → serv. c/ role de detector global de deadlocks → de tempos em tempos cada serv. envia p/ o processo global detecta sua cópia mais recente do _wait-for graph_, o qual combina com demais cópias recebidas a fim de se encontrar esperas cíclicas.

**Phantom deadlocks:** falso positivo, por causa da desatualização das info de grafos _wait-for_ no global detector.

**Edge chasing:** ñ existe papel de global detector, detecção por meio dos local lock managers. Iniciação: serv. envia _probe_ para o serv. de um objeto onde uma transação está bloqueada. Detecção: as sondas são processadas para identificar **ciclos (deadlock)** no wait-for graph. Resolução: uma transação é abortada para quebrar o deadlock, com base em priori. de trans.

**TRANSACTION RECOVERY:** prop. atômica das transações → todos os efeitos das transações confirmadas e nenhum dos efeitos das transações incompletas/abortadas reflitam nos objetos acessados por elas.

**Durabilidade:** requer que os objetos sejam salvos em armazenamento permanente e estejam disponíveis indefinidamente. Portanto, uma confirmação (ack) da solicitação de commit de um cliente implica que todos os efeitos da transação foram registrados em armazenamento permanente, assim como nos objetos voláteis do servidor.

**Atomicidade de falhas:** requer que os efeitos das transações sejam atômicos mesmo quando o servidor falha. A recuperação está preocupada em garantir que os objetos de um servidor sejam duráveis e que o serviço forneça atomicidade de falha.

**Recovery file:** tds obj. na transac. armaz. em memória volátil e recovery file. shadow version.

**Recovery manager:** garantir durabilidade e atomicidade de falhas. Tarefas:

- Salvar objetos em armazenamento permanente (em um recovery file) para transações confirmadas. Restaurar os objetos do servidor após uma falha. Reorganizar o arquivo de recuperação para melhorar o desempenho da recuperação. Recuperar espaço de armazenamento (no recovery file).

**Intentions list:** garante q uma trans. transite do estado prepared para committed ou aborted, consistentemente.

No nível de implementação, a durabilidade é garantida por meio de diversos mecanismos:

1. **Armazenamento Permanente**: A durabilidade garante que objetos sejam salvos em armazenamento permanente. Quando uma solicitação de confirmação de um cliente é reconhecida, isso implica que todos os efeitos da transação foram registrados em armazenamento permanente, juntamente com os objetos voláteis do servidor. Isso significa que mesmo se o servidor falhar, as alterações confirmadas persistirão.
2. **Arquivo de Recuperação**: Servidores mantêm um arquivo ou arquivos de recuperação para registrar objetos confirmados. Durante a operação normal, os servidores mantêm seus objetos na memória volátil. No entanto, objetos confirmados também são registrados no arquivo de recuperação. Em caso de falha, o servidor pode restaurar seus objetos a partir das versões confirmadas mais recentes armazenadas em armazenamento permanente. Este arquivo de recuperação serve como um backup, garantindo durabilidade mesmo diante de falhas do servidor.
3. **Gerenciador de Recuperação**: O gerenciador de recuperação é responsável por garantir a durabilidade e a atomicidade de falhas. Ele executa tarefas como salvar objetos em armazenamento permanente para transações confirmadas, restaurar os objetos do servidor após uma falha, reorganizar o arquivo de recuperação para melhor desempenho de recuperação e recuperar espaço de armazenamento. O gerenciador de recuperação é projetado para ser resiliente a falhas de mídia, garantindo que mesmo se o arquivo de recuperação for corrompido ou falhar, outra cópia possa ser usada para recuperação.
4. **Lista de Intenções**: Os servidores mantêm uma lista de intenções para todas as transações atualmente ativas. Esta lista contém referências e valores de objetos alterados por cada transação. Quando uma transação é confirmada, o gerenciador de recuperação usa a lista de intenções para identificar objetos afetados e registra suas versões confirmadas no arquivo de recuperação. Em caso de aborto da transação, o gerenciador de recuperação usa a lista de intenções para remover versões tentativas de objetos. Isso garante que as alterações confirmadas sejam registradas e persistam com precisão, mesmo no caso de falhas de transação.
5. **Informações Adicionais nos Arquivos de Recuperação**: Para servidores envolvidos em transações distribuídas, os arquivos de recuperação armazenam informações adicionais além apenas dos valores dos objetos. Isso inclui o status de cada transação (confirmada, abortada ou preparada para confirmação) e associações entre objetos e transações. Armazenar essas informações no arquivo de recuperação garante que mesmo em cenários de transações distribuídas, a durabilidade seja mantida e que as alterações confirmadas sejam registradas e recuperadas de forma confiável.
