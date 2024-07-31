# Unidade 01 - Introdução

[**Middleware:**](middleware.md) camada intermediária que simplifica comunicação e integração entre aplicativos distribuídos, fornecendo abstração para complexidades inerentes, facilitando o desenvolvimento.

**Definição SD:** coleção de computadores independentes que se apresenta aos usuários como um único sistema coeso. Aspectos importantes: computadores autônomos, cooperação entre sistemas autônomos e a capacidade de ocultar diferenças na interconexão e organização interna. Metas:

1. **Disponibilidade de Recursos:** facilitar o compartilhamento de informações e colaboração. Aumentar a importância da segurança com o aumento da conectividade e compartilhamento de dados.
2. **Transparência de Distribuição:** apresentar o sistema distribuído como único para usuários. Diferentes formas de transparência incluem:
   - Acesso: ocultar diferenças de representação de dados e como os recursos são acessados.
   - Localização: ocultar onde um recurso está localizado.
   - Migração: ocultar que um recurso pode ser movido para outro local.
   - Realocação: ocultar que um recurso pode ser movido para outro local enquanto está sendo acessado.
   - Replicação: ocultar que um recurso pode ser replicado.
   - Concorrência: ocultar que um recurso pode ser compartilhado por múltiplos usuários concorrentes.
   - Falhas: ocultar a falha e recuperação de recursos.
3. **Interoperabilidade aberta:** um sistema aberto é possível de interagir com outros serviços de sistemas abertos, seguindo regras padrão para serviços, geralmente especificados por interfaces em uma IDL. Interoperabilidade e portabilidade são cruciais.
4. **Escalabilidade:** propriedade essencial medida em dimensões de tamanho, geografia e administração. Para crescer em cada dimensão é necessário resolver diferentes problemas de escalabilidade:
   - Serviços centralizados: concentrar a lógica de negócio em um único serviço, gargalos e pontos únicos de falha.
   - Algoritmos centralizados: algoritmo é executado em um único nó, limita a capacidade de processamento e aumentar a latência.
   - Dados centralizados: dados armazenados em um único local, problemas de capacidade e desempenho.

**Problemas comuns em Desenv. de SDs:**

- Redes não são confiáveis, seguras ou homogêneas. A topologia pode mudar. Latência não é zero. Largura de banda não é infinita. Custos de transporte não são zero. Há um admin.

**Técnicas de Escalabilidade:**

1. **Ocultando Latência:** evitar, tanto quanto possível, o uso de comunicação síncrona**.** Adotar comunicação assíncrona e mover processamento do servidor para o cliente em aplicações interativas.
2. **Distribuição:** dividir componentes em partes menores e distribuí-las para resolver problemas de capacidade limitada de processamento e armazenamento.
3. **Replicação:** distribuir várias instâncias do mesmo componente para aumentar disponibilidade e equilibrar a carga do sistema. Cache é uma forma especial de replicação, mas consistência é um desafio.

- Premissas falsas: rede confiável, segura e homogênea, topologia inalterável, latência zero, largura de banda infinita e custos de transporte zero.
