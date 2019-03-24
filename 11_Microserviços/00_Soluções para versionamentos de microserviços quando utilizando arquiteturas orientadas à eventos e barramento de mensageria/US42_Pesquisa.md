#### Introdução

##### A arquitetura dos sistemas de retaguarda do projeto STAMPS

Partindo da figura abaixo e das apresentações feitas pela equipe técnica do projeto STAMPS, esta é a nossa compreensão da arquitetura proposta pela referida equipe para solucionar o problema de prover um sistema de alta performance para atender um número muito grande de requisições oriundas de um número elevado de fontes diferentes e ao mesmo tempo mantendo a garantia de integridade da informação e possibilitando a facilidade de realização de auditorias no sistema.

![Arquitetura do projeto STAMPS](../geral/images/wibx-arq.png)

Vemos no sistema acima a aplicação do conceito de _Enterprise Service Bus_, conceito esse que implementa a chamada _Service Oriented Architecture_ (ver [The Enterprise Service Bus: Making service-oriented architecture real](https://doi.org/10.1147/sj.444.0781)).

Na figura cima, o **_ESB_** é implementado utilizando a tecnologia de _Micro Services_ (ver [Microservices: yesterday, today, and tomorrow](https://arxiv.org/pdf/1606.04036.pdf)) e para realizar o _Bus_ de comunicações é utilizada a tecnologia de _Message Queues_.

A arquitetura ainda apresenta divisões de responsabilidade não funcionais, sendo uma delas uma _Facade_ (ver Gamma et. al. Design Patterns: Elements of Reusable Object-Oriented Software), porém a separação mais significativa para este trabalho está nos elementos arquiteturais denominados _Listeners_ e _Senders_. Apresentaremos princípios de desenho de sistemas que utilizando estes conceitos aplicados na arquitetura do projeto STAMPS irão justificar nossa proposta de implementação de um modelo de evolução tanto dos programas quanto as estruturas de dados que servem para identificar dados e informações que são recuperadas ou armazenadas por estes mesmos programas.

#### Princípios

##### Command Query Separation

O primeiro princípio a ser abordado é o **CQS** - _Command and Query Separation_. Este princípio prega que dada uma classe de objeetos, métodos que apenas consultam dados (idempotentes) devem ser separados de métodos que os atualizam.

Segundo Meyer, em 1988, eles devem ser separados em:

* _Funções_: produzem resultados, porém não afetam o estado;
* _Procedimentos_: explicitamente afetam o estado, porém não produzem resultado.

Esta dicotomia é chamada de _commands_ e _queries_ e o princípio de _command and query separation_.

##### Single Responsability Principle

O segundo princípio é o **SRP** - _Single Responsability Principle_. Este princípio sugere que cada objeto deve possuir uma única responsabilidade encapsulada pela classe ou módulo. Este princípio é considerado como boa prática em programação utilizando linguagens orientadas à objetos.

Estes dois princípios, principalmente o **CQS**, são os conceitos que influenciaram o desenvolvimento do **CQRS** - _Command Query Responsibility Separation_, que elevou a separação das responsabilidades de gravação e leitura dos dados do nível de objetos para o nível do sistema como um todo.

Um sistema típico aplica o princípio de segregação de responsabilidades do seguinte modo:

* Camada de apresentação ou interface do sistema - que é o que os usuários normalmente veêm do sistemas;
* Camada de negócios ou modelo - que é onde as regras de negócio e o sistema propriamente dito existe;
* Camada de dados - que é onde os dados e informações são armazenados e de onde são resgatados sempre que necessários.

![Sistemas tradicionais em três camadas](../geral/images/sistema-tradicional.png)

O prinçipio **CQRS** procura separar as responsabilidades de leitura e gravação, afetando portando a camada de modelo e sua relação com as camadas de negócio e armazenamento de dados:

![Sistema aplicando arquitetura **CQRS**](../geral/images/sistema-cqrs.png)

Uma evolução natural é separar o banco de dados aplicando a clusterização da leitura, e por motivos de consistência a gravação deve ser centralizada:

![Sistemas tradicionais em três camadas](../geral/images/sistema-cqrs-evolucao.png)

##### Event Sourcing

Para reduzir o eventual gargalo tanto com a gravação de dados quanto com a leitura, a proposta de utilizar um sistema baseado em eventos torna os processos de acesso aos repositórios de dados assíncronos (_Event Sourcing_):

![Sistemas tradicionais em três camadas](../geral/images/sistema-cqrs-es.png)

##### Command Query Responsibility Separation - Event Sourcing

A junção destas tecnologias apresenta, se não uma arquitetura de sistemas, mas um _Design Pattern_ que pode ser aplicado a sistemas com características que justifiquem a separação de modelos de gravação e leitura e ao tratamento de eventos. Como afirmou [Martin Fowler - _When to use it_](https://martinfowler.com/bliki/CQRS.html) - "Muitos sistemas se encaixam perfeitamente na noção de uma base de informações que é atualizada do mesmo modo que é lida, e adicionando-se CQRS a este sistema pode adicionar significante complexidade. Vi certamente casos onde isto provocou significante perda de produtividade, adicionando risco desnecessário ao projeto."

#### Objetivos

Este trabalho investiga respostas para o seguinte problema: como garantir que eventos passados, presentes e futuros possam ser lidos e processados pelo sistema independente das modificações realizadas na estrutura de informações contidas nos eventos durante toda a vida do sistema?

Partindo de uma categorização dos tipos de alterações que um sistema dessa categoria pode sofrer, principalmente no que se refere à alterações no conteúdo dos próprios eventos, vamos propor uma solução para responder a questão acima.

#### Proposta

Para tornar possível o que nos é pedido, analisamos as abordagens possíveis:

* Um sistema de armazenamento de dados que permita:
    * versionamento dos esquemas de dados - possível se o sistema de banco de dados permitir o acesso à informações utilizando interfaces de modo restrospectivo por meio de interfaces definidas pelo usuário e versionadas;
    * evolução do esquema - possível se o sistema de banco de dados permite modificar o esquema sem perda dos dados.

O armazenamento de eventos existe em três níveis de abstração:

1. A _event store_ - uma coleção de _streams_, cada uma de um certo tipo e com um identificador único;
1. A _event stream_ - uma coleção de eventos originados de uma fonte única e ordenada pela data e hora de criação;
1. O evento - conteúdo na forma de pares **chave e valor**.

As operações de modificação dos eventos em cada nível pode ser classificado em básico e complexo, conforme a tabela a seguir:

<style type="text/css">
.tg  {border-collapse:collapse;border-spacing:0;}
.tg td{padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg th{font-weight:bold;font-weight:normal;padding:10px 5px;border-style:solid;border-width:1px;overflow:hidden;word-break:normal;border-color:black;}
.tg .tg-c3ow{border-color:inherit;text-align:center;vertical-align:top}
.tg .tg-0pky{border-color:inherit;text-align:left;vertical-align:top}
.tg .tg-xldj{border-color:inherit;text-align:left}
</style>
<table class="tg">
  <tr>
    <th class="tg-0pky">Nivel</th>
    <th class="tg-0pky">Complexidade</th>
    <th class="tg-0pky">Operação</th>
  </tr>
  <tr>
    <td class="tg-xldj" rowspan="5">Evento</td>
    <td class="tg-xldj" rowspan="3">Básica</td>
    <td class="tg-0pky">Adicionar um atributo</td>
  </tr>
  <tr>
    <td class="tg-0pky">Excluir um atributo</td>
  </tr>
  <tr>
    <td class="tg-0pky">Modificar o nome ou tipo de um atributo</td>
  </tr>
  <tr>
    <td class="tg-xldj" rowspan="2">Complexa</td>
    <td class="tg-0pky">Dois ou mais atributos são unidos como um só</td>
  </tr>
  <tr>
    <td class="tg-0pky">Um atributo é dividio em dois ou mais atributos</td>
  </tr>
  <tr>
    <td class="tg-xldj" rowspan="6">Stream</td>
    <td class="tg-xldj" rowspan="3">Básica</td>
    <td class="tg-0pky">Adicionar um evento</td>
  </tr>
  <tr>
    <td class="tg-0pky">Exluir um evento</td>
  </tr>
  <tr>
    <td class="tg-0pky">Um evento é renomeado</td>
  </tr>
  <tr>
    <td class="tg-xldj" rowspan="3">Complexa</td>
    <td class="tg-0pky">Múltiplos eventos são combinados num só</td>
  </tr>
  <tr>
    <td class="tg-0pky">Um evento é dividido em dois ou mais eventos</td>
  </tr>
  <tr>
    <td class="tg-0pky">Um atributo é movido de um tipo de evento para outro</td>
  </tr>
  <tr>
    <td class="tg-xldj" rowspan="6">Store</td>
    <td class="tg-xldj" rowspan="3">Básica</td>
    <td class="tg-0pky">Adicionar um stream</td>
  </tr>
  <tr>
    <td class="tg-0pky">Excluir um stream</td>
  </tr>
  <tr>
    <td class="tg-0pky">Modificar o nome ou tipo de um stream</td>
  </tr>
  <tr>
    <td class="tg-xldj" rowspan="3">Complexa</td>
    <td class="tg-0pky">Múltiplos streams são combinados num só</td>
  </tr>
  <tr>
    <td class="tg-0pky">Um stream é dividido em dois ou mais streams</td>
  </tr>
  <tr>
    <td class="tg-0pky">Um evento é movido de um stream para outro</td>
  </tr>
</table>

##### Técnicas de migração de _Event Stores_

Existem cinco técnicas possíveis de migração de uma _event store_:

1. Múltiplas versões - nesta técnica, várias versões do evento é suportada pela aplicação. A estrutura do evento é extendida com um número de versão, e ao ser lido por todos os _listeners_, estes devem possuir conhecimento suficiente para lidar com todas as versões existentes;
1. _Upcasting_ - nesta técnica o conhecimento da versão fica contido num _upcaster_, um componente que transforma o evento antes de oferecê-lo à aplicação. Nesta situação os _listeners_ não tem conhecimento nem suporte à diferentes versões e bastam dar suporte apenas à última versão;
1. Transformação preguiçosa - esta técnica utiliza um _upcaster_ também transforma o evento antes de cada operação, porém também armazena o evento atualizado na _event store_;
1. Transformação no local - esta técnica é a utilizada por bancos de dados relacionais que implementam em sua DDS (Data Definition Languagem) os comandos `ALTER TABLE` para atualizar os esquemas e `UPDATE` para atualizar os dados. Bancos de dados **NoSQL** não possuem esta possibilidade;
1. Cópia e transformação - nesta técnica se faz uma cópia e transformação de todos os eventos para uma nova _event store_.

##### Técnicas de atualização das Aplicações e _Event Stores_

As técnicas de atualização das aplicações variam da mais simples e arriscada até as mais complexas e que garantem diferentes níveis de segurança e resiliência e vão depender dos requisitos operacionais de cada usuário.

1. Atualização simples - a nova aplicação é simplesmente copiada sobre a antiga. O tempo que levar mantém o sistema inoperante. A simplicidade é a sua maior vantagem;
1. _Big Flip_ - nesta estratégia, um mecanismo de roteamento suspende a comunicação com parte das máquinas que então são atualizadas enquanto a outra parte mantém o atendimento aos usuários. Ao final da atualização da primeira parte, esta é colocada "no ar" e a segunda parte é tirada do "ar". Durante a atualização somente parte das máquinas irá atender a demanda e portanto deve ser programada para quando isto não cause prejuízos ao funcionamento considerado "normal" da aplicação;
1. _Rolling upgrade_ - a mesma forma da _Big Flip_, porém o número de máquinas que é atualizado de cada vez é um número menor que a metade das máquinas do sistema. Este tipo de modalidade pode ser necessária se a quantidade de máquinas é muito grande e a atualização é muito demorada e não é possível atender a demanda com um certo número mínimo de máquinas. Nesta modalidade as duas versões estarão "no ar" simultaneamente;
1. _Blue-green_ - toda a aplicação tem sempre duas instalações: a versão corrente e a outra versão, que pode ser a anterior ou a próxima versão. Quando uma nova versão é instalada, o fazemos sobre a versão que não está em uso. Quando todo o sistema for atualizado faz-se a "virada" para a nova versão em todos as máquinas simultaneamente;
1. Expansão do contrato - nesta modalidade uma _interface_ é criada para suportar as versões atual e nova. Após isso um processo de migração atualiza as versões antigas para a nova versão. No final o contrato é modificado removendo-se a interface por outra que suporta apenas a nova versão. Esta estratégia é necessária para dar suporte a sistemas que ainda utilizam a versão antiga enquanto novos sistemas começam a usar a nova versão simultaneamente.

##### Combinação de técnicas

Listamos na tabela a seguir as combinações de técnicas para atualizar a _event store_ e aplicação com tempo zero fora de operação:

|  | **Estratégia para aplicação** | **Estratégia para dados** |
| --- | --- | --- |
| Múltiplas versões        | _Big flip_, rolling upgrade_, blue-green_ |  |
| _Upcasting_              | _Big flip_, rolling upgrade_, blue-green_ |  |
| Transformação preguiçosa | _Big flip_, rolling upgrade_, blue-green_ |  |
| Transformação no local   | _Big flip_, rolling upgrade_, blue-green_ | Expansão do contrato |
| Cópia e transformação    | _Big flip_, rolling upgrade_, blue-green_ | Expansão do contrato, _blue-green_ |

#### Metodologia de Atualização de Sistemas _Event Sourcing_

Baseados nas categorizações descritas anteriormente, propomos uma Metodologia de Atualização de Sistemas _Event Sourcing_. Na figura abaixo demonstramos, partindo da classificação de complexidade das operações a serem realizadas, as melhores técnicas a serem empregadas para a atualiação do sistema.

![Framework de atualização de Event Stores](../geral/images/event-store-upgrade-framework.png)

Vemos na figura que, com exceção de uma operação complexa na _Event Store_, podemos quase sempre confiar em atualizar nossos sistemas utilizando **Múltiplas Versões**, **_Upcasting_** ou **Transformação Preguiçosa** para atualizar os eventos ou a _Stream_ de eventos. A atualização da aplicação, nestes casos, pode prosseguir utilizando **_Big Flip_**, **_Rolling Upgrade_** ou **_Blue-Green_**.

Já nos casos de atualizações complexas da _Event Store_, podemos confiar na **Transformação no Local** ou **Cópia e Transformação** conforme o caso da _Event Store_ suportar uma das duas opções. As aplicações podem ser atualizadas por meio de **_Blue-Green_** or **Expansão do Contrato**, isso vai depender do tamanho da atualização. Se optarmos por **Expansão do Contrato**, isso vai exigir aplicar **_Big Flip_**, **_Rolling Upgrade_** ou **_Blue-Green_** quando o contrato cair em desuso.

#### Conclusões

Apresentamos nossa compreensão da arquitetura atual e nos atemos apenas aos fatos, sem fazer uma crítica de sua aplicabilidade.

Categorizamos a problemática relacionada à atualizações de sistemas baseados no _pattern_ de _Event Sourcing_ e apresentamos uma série de soluções que juntas compõem uma metodologia de atualização de sistemas, tanto dos dados quanto dos aplicativos que o compõe.

Assim acreditamos qe com este trabalho apresentamos um método factível de garantir que eventos passados, presentes e futuros possam ser lidos e processados pelo sistema independente das modificações realizadas na estrutura de informações contidas nestes mesmos eventos durante toda a vida do sistema.

#### Trabalhos futuros

Este trabalho apresentou soluções que podem ser tomadas apenas como um exercício teórico, restando agora aplicar estas soluções na prática para verificar sua eficácia e efetividade.

Para isto sugerimos o prosseguimento deste trabalho com uma série de tarefas a serem executadas nos próximos ciclos de desenvolvimento:

* Planejamento de construção de uma prova de conceito:
    * esta atividade contaria com a construção de um modelo em escala da arquitetura real do sistema STAMPS, sem os elementos de _frontend_ de usuários e sem o _backend_ de persistência de dados, mas porém com todos os elementos relacionados desde a recepção das rquisições do _frontend_, o processamento interno e a entrega para gravação no _backend_ e as respostas do sistema a serem apresentadas de volta para o usuário pelo _frontend_.
* Contrução da **Prova de Conceito** propriamente dita e implementação do _framework_ proposto pelo presente trabalho;
* Realização de testes para estressar a **Prova de Conceito** e amadurecer a ferramenta até termos um produto com qualidade suficiente para ser aplicado num sistema real.

Este projeto consumiria de dois a três corridas e necessitaria de pelo menos dois ou três pesquisadores com domínio técnico, alguma experiência em programação e disposição para realiar o trabalho.

#### Referências

1. Schmidt, M-T., et al. "The enterprise service bus: making service-oriented architecture real." IBM Systems Journal 44.4 (2005): 781-797.
2. Thönes, Johannes. "Microservices." IEEE software 32.1 (2015): 116-116.
3. Michelson, Brenda M. "Event-driven architecture overview." Patricia Seybold Group 2.12 (2006): 10-1571.
4. Overeem, Michiel, Marten Spoor, and Slinger Jansen. "The dark side of event sourcing: Managing data conversion." Software Analysis, Evolution and Reengineering (SANER), 2017 IEEE 24th International Conference on. IEEE, 2017.
5. Gamma, Erich. Design patterns: elements of reusable object-oriented software. Addison-Wesley, 1995.
