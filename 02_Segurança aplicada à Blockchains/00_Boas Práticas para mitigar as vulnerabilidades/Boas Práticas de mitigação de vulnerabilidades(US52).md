# 1.	Introdução
Durante a Sprint 201812, a US52 foi desenvolvida, tendo como objetivo gerar um compêndio de boas práticas na escrita de Smart Contracts. A lista de vulnerabilidades, construída na Sprint 201811, na US47 é a base para a pesquisa das boas práticas desta US.

Este relatório apresenta o desenvolvimento, resultados e conclusões do trabalho desta Sprint.

## 1.1.	Lista de vulnerabilidades
Os próximos tópicos apresentam a lista de vulnerabilidades, pesquisada durante a US47, Sprint 201811. Os itens foram reavaliados e atualizados, conforme necessidade.

##### 1.1.1.	EVM Code Coverage
EVM Code Coverage compreende a cobertura de código do bytecode EVM. É o número de opcodes executados/número todos de opcodes.

__Melhor maneira de detecção:__ a melhor maneira de detecção, descrito de acordo com as USs pesquisadas nas Sprints passadas é uma checagem com a ferramenta Oyente-NG, atualizadas pelos pesquisadores do projeto STAMPS.
__Melhor prática para mitigar esta vulnerabilidade:__ não foram encontrados resultados consistentes de melhores práticas para este item.

##### 1.1.2.	Integer Underflow
Integer Underflow é, resumidamente, a atribuição de um valor inteiro menor que o suportado. Considerando o código de uma transferência simples de um token:
```sh
    mapping (address => uint256) public balanceOf;
    
    // INSECURE
    function transfer(address _to, uint256 _value) {
        /* Check if sender has balance */
        require(balanceOf[msg.sender] >= _value);
        /* Add and subtract new balances */
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
    }
    
    // SECURE
    function transfer(address _to, uint256 _value) {
        /* Check if sender has balance and for overflows */
        require(balanceOf[msg.sender] >= _value && balanceOf[_to] + _value >= balanceOf[_to]);
    
        /* Add and subtract new balances */
        balanceOf[msg.sender] -= _value;
        balanceOf[_to] += _value;
    }

```

Se o saldo atingir o valor máximo de uint (2^256), o resultado retornado será zero. Isso reflete a checagem da condição do tamanho dos valores inseridos no código.

A vulnerabilidade em uint reflete ao nível de acesso à esse valor, bem como as funções que podem chamá-lo, como esta variável muda de estado, etc.. 

__Melhor maneira de detecção:__ checagem com as ferramentas Oyente-NG e Mythril.
__Melhor prática para mitigar esta vulnerabilidade:__ a utilização de SafeMath, realizando checagens de tamanhos de valores para dados que são acessíveis por níveis de usuários que não sejam administradores.

##### 1.1.3.	Integer Overflow
Ao contrário do Integer Underflow, descrita no tópico 3.1.2, a Integer Overflow é uma vulnerabilidade em que há a atribuição de um valor inteiro maior do que o suportado pelas variáveis.

Tipos de dados como uint8, uint16, uint24, entre outros, são mais susceptíveis à este tipo de vulnerabilidade, por atingirem mais facilmente o máximo valor permitido.

__Melhor maneira de detecção:__ checagem com as ferramentas Oyente-NG e Mythril.
__Melhor prática para mitigar esta vulnerabilidade:__ a utilização de SafeMath, realizando checagens de tamanhos de valores para dados que são acessíveis por níveis de usuários que não sejam administradores.


##### 1.1.4.	Parity Multisig Bug
Parity Multising Bug, basicamente, é uma vulnerabilidade na qual ocorre um erro de verificação de assinatura do contrato.

Foi identificada em 2017, sendo referente ao código de uma biblioteca do Smart Contract, implantada como um componente compartilhado de todas as carteiras. Um usuário anônimo encontrou a vulnerabilidade e pode tornar-se “proprietário” do contrato da biblioteca.
	
As carteiras que utilizavam de Parity Multi-signature foram bloqueadas e dependiam deste componente foram bloqueadas.

__Melhor maneira de detecção:__ utilização da ferramenta Oyente-NG para verificação do erro.
__Melhor prática para mitigar esta vulnerabilidade:__ Nenhuma encontrada. Esta vulnerabilidade foi deprecada, tendo em vista que a biblioteca afetada foi substituída.

##### 1.1.5.	Callstack Depth Attack
Callstack Depth Attack é uma vulnerabilidade relacionada ao limite de chamadas entre contratos maiores que 1024.
Assim, qualquer chamada, mesmo que confiável e correta, pode apresentar falhas. Isso ocorre por conta do limite na “profundidade” da “pilha de chamadas”. Caso o atacante realizar inúmeras chamadas recursivas e, consequentemente, aumentar a profundidade da pilha para 1023, essas chamadas poderão chamar uma função automaticamente, fazendo com que todas as sub-chamadas (subcalls) falhem.
__Melhor maneira de detecção:__ checagem com a ferramenta Oyente-NG.
__Melhor prática para mitigar esta vulnerabilidade:__ Nenhuma. Esta vulnerabilidade está deprecada.

##### 1.1.6.	Transaction Order Dependence (TOD)
Transaction Order Dependency é uma vulnerabilidade em que ocorre mudança de estado do programa entre sua chamada e a execução.

__Melhor maneira de detecção:__ aplicar checagens com a ferramenta Oyente-NG.
__Melhor prática para mitigar esta vulnerabilidade:__ realizar auditoria de código.

##### 1.1.7.	Reentrancy vulnerability
Reentrancy vulnerability é uma vulnerabilidade relacionada com o descrito no item 3.1.6, em que a função é inserida novamente antes da rescisão.

Sua primeira detecção envolveu funções que poderiam ser chamadas repetida e recursivamente antes que a primeira chamada da função fosse finalizada totalmente.

Assim, diferentes chamadas de função podem interagir, resultando em uma vulnerabilidade. Analisando o código inseguro abaixo, pode-se verificar que, como o saldo não é definido como zero até que a função termine, as chamadas posteriores ainda serão consideradas válidas e serão executadas. O saldo será retirado várias vezes, apesar de não ser uma transação possível.


```sh
mapeamento (endereço => uint) userBalances privados;
function withdrawBalance () public {
  	uint amountToWithdraw = userBalances [msg.sender];
  If (! (msg.sender.call.value (amountToWithdraw) ())) {
lançamento;
}
/* Neste ponto, o código do chamador é executado e pode chamar o withdrawBalance novamente
  userBalances [msg.sender] = 0;
}
```

__Melhor maneira de detecção:__ checagem com a ferramenta Oyente-NG.
__Melhor prática para mitigar esta vulnerabilidade:__ não realizar chamadas de funções externas enquanto o trabalho interno necessário não for finalizado. Auditoria de código.

##### 1.1.8.	Timestamp dependency
Timestamp dependency é uma vulnerabilidade também relacionada com o item 3.1.6, na qual o timestamp do bloco é alterado por um minerador.

A manipulação de timestamp pode ser realizada pelo usuário, através de ajustes nas configurações de horário por alguns segundos, para conseguir benefícios próprios. Quando um Smart Contract usa o registro de data e hora para gerar números aleatórios, o minerador pode postar um registro de data e hora dentro de trinta segundos do bloco sendo validado. Isso permite que preveja uma opção mais preferida, sendo um problema para mercados descentralizados, onde uma transação para compra de tokens pode ser visualizada por todos os usuários.

Assim, os mineiradores podem gerenciar timestamps dentro do bloco de trinta segundos, sendo que o timestamp não deve ser menor que o timestamp do bloco anterior.

__Melhor maneira de detecção:__ checagem com a ferramenta Oyente-NG, Manticore, Mythrill, entre outras.
__Melhor prática para mitigar esta vulnerabilidade:__ utilizar da regra de quinze segundos no código. Esta regra dita que, se a função do contrato pode tolerar um desvio de quinze segundos, é seguro utilizar timestamp. Caso não tolere, evitar o timestamp, utilizando block.timestamp. O block.number como timestamp também deve ser evitado. A utilização de interfaces ao invés de addresses também é sugerida, bem como a não utilização de extcodesize na validação de contas externas.

##### 1.1.9.	Call to the unknown
Call to the unknown é uma vulnerabilidade na qual o contrato de um invasor pode reivindicar a liderança enviando um saldo suficiente para um contrato inseguro. Por consequência, as transaçõe de outro jogador, que tentaria reivindicar a liderança, seriam lançadas.

Esta vulnerabilidade causa a negação de serviço permanente ao contrato alvo, tornando-o inutilizável.

__Melhor maneira de detecção:__ checagem com a ferramenta Oyente-NG.
__Melhor prática para mitigar esta vulnerabilidade:__ o contrato atacante chama para si um item, em seguida bloqueando qualquer outro de tentativas de lances maiores, impedindo o contrato atacado de se desfazer de seu lance.

##### 1.1.10.	Out-of-gas send
Out-of-gas send compreende a execução de fallback do callee. Um exemplo que pode ser citado é: um site realiza um jackpot, em que uma grande quantidade de Ether pode ser reivindicada pelo ganhador. No entanto, por um erro de código, uma quantidade maior de gas foi necessária para a reivindicação, excedendo o limite de uma transação.

__Melhor maneira de detecção:__ checagem com a ferramenta Oyente-NG.
__Melhor prática para mitigar esta vulnerabilidade:__ a vulnerabilidade encontra-se deprecada, tendo sido solucionada.

##### 1.1.11.	Type casts
Type casts é uma vulnerabilidade em que ocorre erro nas conversões de tipos de dados.

__Melhor maneira de detecção:__ checagem com a ferramenta Oyente-NG.
__Melhor prática para mitigar esta vulnerabilidade:__ nenhuma forma de mitigação foi encontrada até o momento.

##### 1.1.12.	Field disclosure
Field disclosure é uma vulnerabilidade em que o valor privado é publicado pelo minerador.

__Melhor maneira de detecção:__ utilização do Hawk Framework.
__Melhor prática para mitigar esta vulnerabilidade:__ nenhuma forma de mitigação foi encontrada até o momento.

##### 1.1.13.	Immutable bug
Immutable bug é uma vulnerabilidade em que, após a implantação, um contrato sofre alterações.

__Melhor maneira de detecção:__ não foram encontradas maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ realizar auditoria por várias ferramentas, experts e aplicação de extensivos testes unitários e integrados.

##### 1.1.14.	Ether lost
Ether lost é uma vulnerabilidade em que há o envio de Ether para um endereço órfão.

__Melhor maneira de detecção:__ checagem com a ferramenta Oyente-NG.
__Melhor prática para mitigar esta vulnerabilidade:__ enviar Ether para um contrato ERC20 sem utilizar o protocolo resulta em perda. A rede Ethereum não valida endereços. A melhor prática é evitar digitar endereços e utilizar aplicações com endereços já validados.

##### 1.1.15.	Randomness bug
Randomness bug é uma vulnerabilidade em que a semente (seed) é influenciada por um minerador malicioso.

__Melhor maneira de detecção:__ checagem com a ferramenta Oyente-NG.
__Melhor prática para mitigar esta vulnerabilidade:__ não introduzir entropia na rede Ethereum utilizando valores do próprio contrato - que podem ser previstos pelo minerador ou qualquer usuário que esteja observando a Blockchain. Utilizar de geradores de números aleatórios confiáveis, como o Oraclize.

##### 1.1.16.	Cleanup of Exponent in Exponentiation
Cleanup of Exponent in Exponentiation é uma vulnerabilidade em que há utilização de números inteiros de tipos pequenos como expoente de operação de exponenciação, cusando resultados inválidos.

Números inteiros menores que 256 devem ser preenchido com zeros para executar operações, às vezes a evm demora para realizar esta operação, causando uma vulnerabilidade que pode ser explorada.

__Melhor maneira de detecção:__ checagem com a ferramenta Oyente-NG.
__Melhor prática para mitigar esta vulnerabilidade:__ utilizar uint256 nos expoentes, tomando cuidado com as vulnerabilidades Integer Underflow e Integer Underflow.

##### 1.1.17.	Memory Corruption in Multi-Dimensional Array Decoder
Memory Corruption in Multi-Dimensional Array Decoder é uma vulnerabilidade em que há chamadas de funções de outros contratos que retornam matrizes de tamanhos fixos multidimensionais. Isso pode resultar em corrupção de espaços de memória.

__Melhor maneira de detecção:__ não foram encontradas melhores maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ corrigida na versão 0.4.22. Fazer conversão após a chamada à função que retorna da matriz.

##### 1.1.18.	Invalid Encoding of Structs in Events
Invalid Encoding of Structs in Events é uma vulnerabilidade em que estruturas como parâmetros de eventos não são manipuladas corretamente. Este problema foi introduzido na versão 0.4.17.

__Melhor maneira de detecção:__ não foram encontradas melhores maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ não utilizar estruturas como parâmetros de eventos. Foi corrigida na versão 0.4.25.

##### 1.1.19.	Arbitrary Jump with Function Type Variable
Arbitrary Jump with Function Type Variable é uma vulnerabilidade em que um invasor pode apontar uma variável de tipo de função para qualquer instrução de código.

__Melhor maneira de detecção:__ não foram encontradas melhores maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ minimizar o uso de assembly. Um desenvolvedor não deve permitir que um usuário atribua valores arbitrários às variáveis do tipo de função.

##### 1.1.20.	Incorrect Inheritance Order
Incorrect Inheritance Order é uma vulnerabilidade em que, através de múltiplas heranças, ocorra erros, sobrescrita ou comportamentos inesperados.

__Melhor maneira de detecção:__ não foram encontradas melhores maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ especificar cuidadosamente a herança na ordem correta, ao herdar vários contratos - especialmente se tiverem funções idênticas. A regra geral é herdar os contratos na ordem do mais geral para o mais específico.

##### 1.1.21.	batchOverflow
batchOverflow é uma vulnerabilidade em que há transferência de valores sem serem debitados do saldo.

__Melhor maneira de detecção:__ não foram encontradas melhores maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ utilizar da ferramenta Mythrill e de SafeMath.

##### 1.1.22.	proxyOverflow
proxyOverflow é uma vulnerabilidade relacionada à Integer Overflow.

__Melhor maneira de detecção:__ não foram encontradas melhores maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ utilizar da ferramenta Mythrill e de SafeMath.

##### 1.1.23.	transferFlaw
transferFlaw é uma vulnerabilidade relacionada à Integer Overflow.

__Melhor maneira de detecção:__ não foram encontradas melhores maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ utilizar da ferramenta Mythrill e de SafeMath e de mudanças no código implementado.

##### 1.1.24.	ownerAnyone
ownerAnyone é uma vulnerabilidade em que qualquer usuário pode se tornar o dono do contrato, havendo falta de controladores de acesso na função setOwner (DoS).

__Melhor maneira de detecção:__ não foram encontradas melhores maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ realizar mudanças no código.

##### 1.1.25.	multiOverflow
multiOverflow é uma vulnerabilidade relacionada com a transferência de valor sem que haja retirada do saldo. Ocorre Integer Overflow na multiplicação, sendo relacionada com a vulnerabilidade de Integer Overflow.

__Melhor maneira de detecção:__ não foram encontradas melhores maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ utilizar da ferramenta Mythrill e de SafeMath.

##### 1.1.26.	burnOverflow
burnOverflow é uma vulnerabilidade em que há transferência de uma grande quantidade de tokens sem gasto real de nenhum token. Relacionada, também, à vulnerabilidade Integer Overflow.

__Melhor maneira de detecção:__ não foram encontradas melhores maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ utilizar da ferramenta Mythrill e de SafeMath.

##### 1.1.27.	ceoAnyone
ceoAnyone é uma vulnerabilidade que permite que o chamador altere o endereço do beneficiário.

__Melhor maneira de detecção:__ não foram encontradas melhores maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ aplicar correções no nome do construtor.

##### 1.1.28.	allowAnyone
allowAnyone é uma vulnerabilidade que apresenta o construtor com um nome diferente do nome do contrato. Assim, qualquer pessoa pode transferir tokens em nome de outro que tenha saldo maior que zero.

__Melhor maneira de detecção:__ não foram encontradas melhores maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ utilizar de SafeMath.

##### 1.1.29.	tradeTrap
tradeTrap é uma vulnerabilidade em que o owner pode redefinir o valor de compra e venda. Há falta de um controlador de acesso e ocorre Integer Overflow após a fase inicial de lançamento da moeda.

Os tokens ERC-20 publicamente negociáveis possuem um valor de mercado considerável. Várias centrais, como Binance, Houbi.pro e OKex, ou descentralizadas, como IDEX, EtherDelta e ForkDelta, fornecem ao mercado negociações públicas. A transparência e segurança dos Smart Contracts correspondentes é fundamental, para proteger os dados dos clientes.

Depois que esses contratos publicamente negociáveis são implantados, não devem estar sujeitos à controle ou manipulação centralizados. No entanto, essa vulnerabilidade, tradeTrap, permite que haja interfaces manipuláveis nestes contratos, afetando diretamente os preços de compra e venda de tokens, por exemplo.

__Melhor maneira de detecção:__ não foram encontradas melhores maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ utilizar de SafeMath, estabelecendo limites de valores e duração para o período de lançamento no contrato. Além disso, evitar valores de compra e venda diferentes.

##### 1.1.30.	evilReflex
evilReflex é uma vulnerabilidade em que o invasor pode assumir o comando do contrato, chamando sua própria função de transferência.

Analisando o código abaixo, é possível identificar a vulnerabilidade na função approveAndCallcode(), de um Smart Contract afetado pelo evilReflex. Na linha em que _sprender.call() é chamado com o parâmetro controlável pelo usuário em _extraData.
Pela construção, o uso pretendido dessa função de retorno de chamada é o envio de uma notificação, relacionada com a conclusão da approve().

No entanto, ajustando o _extraData, um invasor poderia ter acesso aos dados de retorno da chamada.


```sh
/* Aprroves and then calls the contract code */
functionaprroveAndCallcode(address _spender, uint256 _value, bytes _extraData) returns (bool success) {
	Allowed[msg.sender][_spender] = _value;
Aprroval(msg.sender, _spender, _value);

	// Call the contract code
if(!_spender.call(_extraData)) {
revert();
}
}

```

Assim, essa vulnerabilidade permite que um invasor chame qualquer endereço de contrato de um contrato vulnerável, com parâmetros arbitrários. Dados como privilégios de contratos estariam vulneráveis.

__Melhor maneira de detecção:__ não foram encontradas melhores maneiras de detecção.
__Melhor prática para mitigar esta vulnerabilidade:__ evitar o uso de callback no contrato.


## 2.	Resultados e conclusões obtidas
Através de análises das vulnerabilidades listadas, foi possível identificar que nem todas estão resolvidas, enquanto que uma parcela encontra-se deprecada.

Além disso, pode-se verificar que, das sugestões de melhor prática, destacam-se:

-	Uso de SafeMath;
-	Preocupação com os tipos de dados de entrada e saída;
-	Preocupação com os níveis de acesso a dados; e
-  	Auditoria de código.

Para uma maioria esmagadora, a melhor solução de detecção é a utilização de ferramentas como Oyente-NG e a ferramenta Mythril.

## 3.	Referências
