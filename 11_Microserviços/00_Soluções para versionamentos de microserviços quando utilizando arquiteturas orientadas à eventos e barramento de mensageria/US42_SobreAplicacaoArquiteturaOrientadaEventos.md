## Sobre a Aplicação da Arquitetura Orientada à Eventos

### Daitan Withe-Paper Event-Driven Architecture

Nenhum exemplo justifica o uso da arquitetura orientada à eventos como requisito funcional. O exemplo da empresa de Telecom simplesmente justifica a aplicação ao criar contadores para registros de eventos.
* O exemplo da Uber também aplica o conceito de contadores;
* O exemplo do serviço de entregas de alimentos mais uma vez é a aplicação de contadores porém com o adicional do mecanismo de retardo para disparar processos somente após certos acumulos de contadores derivados dos contadores primários;
* O exemplo do banco utilizada a arquitetura de eventos aplicada como requisito não funcional e em seguida usa o exemplo de contadores que é independente do primeiro para justicar a arquitetura;
* O exemplo da empresa de energia, também, foca nos contadores. O aspecto técnico da arquitetura orientada à eventos demonstra a realização de requistos não funcionais;
* No final o documento escapa pela tangente e justifica o "modelo de negócios dos vendedores de produtos de processamento de eventos".

### Event-Driven Architecture Overview

Ótimo material que descreve e justifica a arquitetura como requisito não funcional.

### Demais documentos

Não acrescentam nada com relação à arquitetura orientada à eventos.

## O que justifica uma arquitetura orientada à eventos do ponto de vista funcional

Justificativas funcionais:
* Log de auditoria;
    * A própria _blockchain_ é um caso de implementação interessante dada a naturesa "indelével" de sua estrutura de dados.
* Representação de estados presente e passado da aplicação;
    * Usado para reproduzir o estado passado no presente;
    * Projetar o estado futuro de acordo com tendências observadas no histórico de estados.

## O que justificativa uma arquitetura orientada à eventos do ponto de vista não funcional

Justificativas não funcionais:
* Balanceamento de carga;
* Tolerância à falhas;
* Altíssimo desacoplamento.

## O que deve ser versionado num sistema orientado à eventos como requisito aspecto funcional

* Os dados propriamente ditos e que trafegam dentro dos eventos.

### Versionando os dados num sistema orientado à eventos

Os dados devem ser versionandos no momento que são gerados.

O versionamento pode ser dar no modo **imediato** (_eager_) ou **tardio** (_lazy_).

No modo **imediato** os dados submetidos são versionados imediatamente - neste método pode ser utilizado um registro data e hora que garanta a unicidade da versão - o registro data e hora pode ser insuficiente se o sistema tiver a capacidade de gerar mais de um evento dentro da menor fração de tempo possível de ser registrada.

No modo **tardio** os processos geradores (_writers_) submetem os dados para o sistema, e este usa um processo interno que irá versionar os dados posteriormente - este método pode ser utilizado se a ordem de chegada não for um fator possível para versionar os dados.

Um problema que deve ser endereçado pelo versionamento de dados num sistema orientado à eventos é que muitas vezes os dados não podem ser tomados isoladamente mas dentro de um contexto. O contexto deve ser o de uma transação ACID e pode envolver várias tuplas em diferentes tabelas de um mesmo banco de dados ou até mesmo diferentes bancos de dados, podendo ainda ter de se levar em consideração dados externos como imagens e outros tipos de binários ou arquivos textuais, referências a páginas na internet, etc.

> **Observação**: independente ou não do uso do registro de data e hora, a data e hora pode fazer parte do modelo de dados da informação e o versionamento deve ser mantido totalmente à parte das informações pertinentes ao negócio.

## O que deve ser versionado num sistema não funcionalmente orientado à eventos como requisito não funcional

* A implementação do evento, o que envolve a sua estrutura de dados (e não os valores _per se_); e
* Os processos que atuam sobre estes dados.
