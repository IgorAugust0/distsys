# Unidade 08 - Sistemas de Arquivos Distribuídos

DFS permitem que múltiplos processos compartilhem dados de várias localizações, fundamentais em SD; têm o mesmo propósito dos sistemas de arquivos locais em sistemas não distribuídos, construídos em cima deles, porém exige mecanismos adicionais relacionados à comunicação, sincronização, replicação, consistência, etc. Exemplo: NFS e AFS

**ARQUITETURAS**

**Arquitetura Cliente-Servidor:** cada servidor de arquivo distribuído fornece uma visão padronizada de seu sistema de arquivos local; os clientes acessam arquivos armazenados em um servidor através de um protocolo de comunicação (RPC).

- Acesso remoto: o conteúdo do arquivo reside no servidor de arquivos e todas as operações sobre esse arquivo, demandadas pelos clientes, são realizadas por meio de chamadas de procedimentos remotos. Ex.: NFS.
- Upload/Download: uma cópia do arquivo é enviada do servidor para o cliente operar sobre ela localmente. Após a conclusão das operações, no cliente, a cópia atualizada do arquivo é enviada para o servidor a fim de substituir a cópia original (old) pela nova cópia atualizada
  (new). Ex.: GFS.
- **NFS**: baseado na arquitetura de **acesso remoto** que utiliza RPC (stubs (cliente e servidor), marshalling, caching, etc.) para a execução remota de operações. Aplicações no cliente executam operações de arquivo (ex. read/write, open, seek, etc.) como se os arquivos estivessem armazenados localmente. O SO, por meio do VFS (abaixo do system call), identifica se a operação chamada é para um arquivo local ou remoto. Local: o fluxo segue pelos subsistemas do kernel que lidam com operações no storage local. Remoto: um módulo NFS client é acionado para se comunicar com o servidor. No servidor, a mesma abordagem é seguida até o acesso físico ao arquivo.

**DFS Baseados em Cluster:** utilizados para aplicações paralelas, onde os arquivos são distribuídos entre vários servidores. _File-striping:_ distribuir um único arquivo em múltiplos servidores.

- **GFS:** projetado para operar em ambientes de clusters de computadores, visando alta-disponibilidade e performance voltado para cargas de trabalho voltadas para grandes arquivos, que são “_fatiados_” e as partes resultantes são distribuídas em diferentes nós do cluster de servidores de arquivos. Dessa forma, é possível acessar diferentes partes do arquivo de forma _paralela_. Um _nó master_ fornece informações de onde estão as diferentes partes de um arquivo, desta forma, clientes acessam o nós Master para obter os servidores em que encontrarão as partes do arquivo que necessitam; posteriormente, os clientes acessam os servidores de _chunks_ para ter acesso aos dados distribuídos do arquivo de interesse.

**Processos:** única diff → diferentes tipos de processos cooperando, como serv. de armaz. e gerenc. de arq.

- **Stateless (v2 - v3):** simples, não requer recuperação de estado se o servidor falhar, mas não oferece garantias ao cliente se uma requisição foi concluída; difícil de implementar totalmente em casos práticos em NFS. abordagem no NFS
- a cada operação sobre um arquivo, o cliente deve enviar a identificação do arquivo a ser operado, bem como a posição onde a operação será feita e demais informações que permitam o servidor a validar a operação e executá-la, tal como o ID do usuário, entre outras. Como consequência, o volume de dados de controle trocado entre cliente e servidor é maior.
- **Stateful (v4):** servidor mantêm informações mínimas sobre os clientes. Necessário para funcionar em redes de longa distância. Requer um protocolo eficiente de consistência de cache. Oferece garantias de acesso exclusivo a arquivos com base em contratos de locação.
- o servidor mantém informações de estado de um arquivo aberto e, a partir da sua abertura, operações sobre o arquivo (ex. read, write, seek) poderão ser realizadas sem a necessidade de identificar o arquivo, a posição em que a operação terá efeito, etc

**Comunicação:** muitos DFS são baseados em RPC, visando tornar o sistema independente dos SO subjacentes, das redes e dos protoc. de transp..

**NFSv3 —** 4 oper.: **Request** _LOOKUP_ → **Response** _Lookup name_; **Request** _READ_ → **Response** _read file data_;

**NFSv4 —** 2 oper., procedimento composto: **Request** LOOKUP, OPEN, READ → **Response** _Lookup name, open file, read file data;_ Suporte a callback: permite serv. fazer chamada de RPC p/ o cliente.

**Naming:** nome geral. organizados em um namespace hierárquico. No NFS, os clientes têm acesso transparente a um sistema de arquivos remoto, permitindo montar um sistema de arquivos remoto em seu próprio sistema de arquivos local.

**Mounting/Export:** serv. NFS exporta partes de seu sistema de arquivos local, permitindo que clientes montem e acessem essas partes localmente. A montagem no cliente associa uma parte do sistema de arquivos remoto ao seu próprio sistema. Quando o cliente acessa arquivos nesse namespace montado, são feitas chamadas remotas ao servidor para acesso aos arquivos. Esse processo pode ser aninhado, com partes do namespace montado tb sendo montadas de outros servidores.

**Automounting:** montar um namespace remoto sob demanda, ou seja, quando é acessado no cliente e não na inicialização do sistema.

**Sincronização:** nos SD a semântica (UNIX, sessão, arq. imutáveis, transações) do compartilhamento de arquivos se torna um pouco complicada quando questões de desempenho estão em jogo.

**File locking:** em arq. cliente-servidor stateless lock manager é necessário. NFSv4 distingue **read** lock de **write** lock.

- Leases: tempo concedido pelo servidor para um lock esteja válido. Ao seu término, caso não seja renovado, o lock é removido pelo servidor.

**Consistência e Replicação:** importante papel DFS operando em wide-area networks (WAN)

NFSv3: client-side caching → de fora e geralmente não garante consistência.

NFSv4: client-side caching → a consistência do cache ainda é tratada de forma dependente da implementação, com estratégias diferentes para lidar com a consistência dos dados e atributos do arquivo:

- a cache de dados lidos do servidor e a cache de dados escritos no cliente e descarregados no servidor apenas quando o arquivo é fechado. Vários processos no cliente podem compartilhar a cache. A cache pode ser mantida no cliente, mesmo depois que o arquivo é fechado; essa cache deve ser revalidada quando o arquivo for aberto novamente no cliente.

Segurança: é tratada pela autenticação e controle de acesso nos servidores.

- Funcionalidades: uso de secure RPC para a comunicação entre cliente e servidor; controle de acesso do usuário, normalmente, via ACLs; autenticação descentralizada, combinando o uso de usando Secure File System (SFS) com um serviço de autenticação (ex. LDAP)
