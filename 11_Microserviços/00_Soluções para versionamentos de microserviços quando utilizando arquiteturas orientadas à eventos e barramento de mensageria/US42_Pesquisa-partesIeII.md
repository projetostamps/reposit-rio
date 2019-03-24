uma pergunta:

no projeto foi contemplado o uso de “schemas” para envelopar as mensagens que trafegam usando tópicos do KAFKA?

ou, se não, existe uma estrutura de mensagens que vocês estejam usando?


o porque desta pergunta:

* “schemas” proveem uma espécie de contrato que define como uma mensagem deve ser estruturada. dependendo da tecnologia por trás do esquema do schema (coisa de louco) podemos validar se o esquema é compatível com versões anteriores, isto é, alterações num esquema que permitem que versões antigas dos consumidores possam consumir a nova versão, enquanto que novos consumidores podem usufruir dos novos recursos da nova versão (versão da mensagem aqui estou eu me referindo).”

para que precisamos disso:

* existem muitos modelos de serializar e de-serializar dados, ou seja, transformar os dados que temos na memória durante a execução de um programa (uma aplicação web, um sistema de entrada de dados de um mainframe, etc.) para dados que devem ser gravados num arquivo ou banco de dados. muitas vezes observamos uma série de incompatibilidades entre formatos (datas, strings, números, etc.) e o formato JSON é muito usado por API’s mas é totalmente agnóstico de formato - no final das contas todo é STRING.

o que eu estava procurando:

* existem duas preocupações com relação ao controle de versões de micro-serviços:
    * versão da mensagem;
    * versão do código executável.

> ou seja, podemos mudar a estrutura de dados e processar tudo do mesmo modo, ou podemos manter a estrutura de dados mas processar de um modo diferente, ou podemos mudar tudo.

> se procurar responder primeiro à questão da estrutura de dados temos os “schemas”.

daí eu “trombei” com um aplicativo que eu ainda não conhecia (estou lendo o “getting started” dele neste momento - se chama ->AVRO<-.

> ele tem interfaces C, C++, C#, Java, PHP, Python e Ruby (não vi javascript no site). esquemas são descritos em JSON então usar via javascript não deve ser um grande problema!

> o AVRO cria e mantém “schemas” e valida essa compatibilidade para nós. além disso ele se encarregaria da serialização/de-serialização das mensagens para um formato binário (economia de espaço, banda, etc.), o RPC (com um recurso de só transferir o “schema” no handshake inicial para validação.

ele tem suporte ao KAFKA

porém é mais uma “coisa” no meio de todas as outras “coisas” que tornam a aplicação mais complexa. do ponto de vista do programador, é um intermediário para empacotar/desempacotar os dados, então é uma troca de seis por meia dúzia.

* porém se procurar responder à segunda questão - o código executável, o AVRO vai acabar permitindo que código velho continue residindo no sistema respondendo às mensagens novas como se nada de novo tivesse acontecido.
* então, se o AVRO responde ao registro dos “schemas” das mensagens, precisamos de um registro dos “serviços” também! e em seguida temos que ter um mapeamento “schema” X “service” que mantenha as versões corretamente ligadas e faça o “liga/desliga” de mensagens e serviços deprecados!



PARTE II


o que pretendemos fazer:
* investigar mecanismos ou dispositivos de registros de esquemas mais amigáveis ao AMQP (o AVRO continua como referência);
* investigar mecanismos ou dispositivos de bind entre as implementações feitas em SenecaJS e esquemas que descrevem o payload das mensagens transmitidas ou recebidas via AMQP;

resultados:
* em tempo de desenvolvimento, a associação (bind) entre a implementação e a versão que esta implementação usa de determinado esquema de mensagem que será enviada ou será recebida via AMQP;
* em tempo de execução, uma forma correta e segura de garantir que implementações recusem esquemas indevidos (versão incorreta).

à estes resultados chamaremos de: controle de versão de esquemas de dados numa arquitetura baseada em eventos sob um sistema de transporte via mensageria.

o que ficará fora do escopo desta pesquisa:
* o versionamento das implementações propriamente dita não é escopo nem preocupação desta pesquisa;
* o mecanismo de discovery, por enquanto, não será objeto desta pesquisa.



PARTE III


antes de mais nada, esquecer as partes I e II

questionamentos:

* o sistema que estará sob este mecanismo de "Arquitetura Orientada à Eventos" será todos os sistemas da WiBX ou somente algum módulo em particular?
