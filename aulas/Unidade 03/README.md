# Unidade 03 - Processos

**Processos:**

- Papel crucial em SD.
- Embora processos sejam blocos fundamentais em SDs, threads, com granularidade mais fina, são frequentemente utilizadas para melhorar o desempenho.

**Modelos de Implementação:**

**Modelos de Implementação:**

|                 | Espaço do Usuário (M:1)                                                               | Espaço do Kernel (1:1)                                                           | Modelo Híbrido (M:N)                                                                 |
| --------------- | ------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Definição       | Múltiplas threads de usuário são mapeadas para uma única thread do kernel.            | Cada thread de usuário é mapeada diretamente para uma thread do kernel.          | Múltiplas threads de usuário são mapeadas para um número menor de threads do kernel. |
| Características | Eficiência em termos de troca de contexto de thread.                                  | Maior controle e suporte do kernel para gerenciamento de threads.                | Combina eficiência e controle do espaço do usuário com o suporte do kernel.          |
|                 | Gerenciadas sem intervenção do kernel.                                                | Trocas de contexto mais custosas.                                                | Oferece um equilíbrio entre eficiência e flexibilidade.                              |
| Uso em SD       | Adequado para sistemas distribuídos onde a eficiência de troca de contexto é crucial. | Menos comum em sistemas distribuídos devido à sobrecarga das trocas de contexto. | Amplamente utilizado em sistemas distribuídos para otimizar desempenho e recursos.   |

**Threads em Sistemas não Distribuídos:**

- Benefícios incluem independência de contextos de execução dentro de um processo e exploração de paralelismo em sistemas multiprocessadores.
- Uso eficaz para economizar recursos do sistema, evitando sobrecarga de IPC causada por processos cooperativos.
- Estruturação mais simples para muitas aplicações como IDEs, jogos, GUIs, editores de texto e planilhas.

**Threads em Sistemas Distribuídos:**

- Permitir que chamadas de sistemas bloqueantes ocorram sem que todo o processo seja bloqueado.
- Facilitam expressão de comunicação em sistemas distribuídos, mantendo múltiplas conexões lógicas simultâneas.
- Explorar o paralelismo em sistemas multiprocessados, sejam eles tanto do lado cliente quanto do lado servido.

**Clientes Multithreaded:**

- Ocultar latências de comunicação em sistemas WAN, facilitando a expressão de comunicação ao manter várias conexões lógicas simultâneas:
  - Iniciar a comunicação e ao mesmo tempo executar alguma outra atividade até a chegada da resposta.
- Exemplo: navegadores da web.

**Servidores Multithreaded:**

- Benefícios não apenas na simplificação do código do servidor, mas também na exploração de paralelismo para alto desempenho.
- Não é adequado para cenários em que o número de clientes simultâneos é elevado (ex. 10 mil ou mais) e uma thread é criada servir cada cliente.
- Exemplo: servidor de arquivos.

| Modelo                           | Características                                                                                                                                                            |
| -------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Threads                          | Paralelismo, chamadas de sistema bloqueantes                                                                                                                               |
| Processo de única thread         | Sem paralelismo, chamadas de sistema bloqueantes                                                                                                                           |
| Máquina de estados finitos (FSM) | Paralelismo. Não pode usar chamadas de sistemas bloqueantes, não suporta múltiplos fluxos de execução independentes, atendimento de múltiplas requisições concorrentemente |

**Organização de Clientes e Servidores:**

**Cliente:**

- Foco em GUI.
- Modos de operação: software no lado do cliente interagindo com serviço remoto através de protocolo.
- Adaptado para transparência de distribuição: transparências de acesso, localização/migração, replicação e de falha.
- **Thick** (rich): Usa recursos locais (CPU, Memória, Armazenamento)
- **Thin**: executado inteiramente no lado do servidor, não usando recursos da máquina cliente

**Servidor:**

- Processo que implementa um serviço específico em nome de uma coleção de clientes.
- Tipos comuns incluem servidor de arquivos, web, banco de dados, aplicativos, comunicação, segurança, nome/diretório, tempo, instalação/atualizações.
- Geralmente organizado para esperar por uma solicitação de cliente e garantir que a solicitação seja atendida, aguardando a próxima solicitação.
- Tipos:
  - Servidores iterativos: lidam com um cliente de cada vez.
  - Servidores concorrentes: lidam com vários clientes simultaneamente.

**Contato com um Servidor: End Points:**

- Aspecto importante: onde os clientes contatam o servidor.
- Clientes enviam solicitações para um end point (porta) na máquina onde o servidor está em execução.
- Soluções incluem atribuir globalmente pontos finais para serviços conhecidos ou usar serviços de nome (DNS).

**Serviços sem End Points Pré-Atribuídos:**

- Alguns serviços não requerem um end point pré-atribuído.
- Exemplo: Servidor de hora do dia pode usar um end point atribuído dinamicamente pelo sistema operacional local.
- Soluções:
  - Daemon em cada máquina que executa servidores, rastreando o ponto final atual.
  - Superservidor (inetd) em sistemas Unix, ouvindo vários pontos finais e criando processos para manipular solicitações.

**Interrupção de um Servidor:**

- Importante ao considerar como um servidor pode ser interrompido.
- Soluções incluem encerrar abruptamente o cliente ou enviar dados fora de banda para processamento urgente pelo servidor.

| Stateless                                                                                                                                                       | Stateful                                                                                                                                                                       |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Não mantêm informações sobre o estado dos clientes.                                                                                                             | Processam solicitações com base em informações de solicitações atuais e anteriores.                                                                                            |
| Processam solicitações com base nas informações fornecidas, sem depender de informações de solicitações anteriores.                                             | Exigem acesso a informações de estado durante o processamento. Clientes não podem trocar de servidor a cada requisição, apenas em cada conexão.                                |
| Cada requisição encapsula todas as informações do cliente necessárias para o servidor executar essa requisição. Exemplo: Servidores web para páginas estáticas. | Pode processar todas as solicitações associadas às mesmas informações de estado ou compartilhar informações de estado entre servidores. Exemplo: Servidores de banco de dados. |
