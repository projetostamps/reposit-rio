
algumas coisas para estudar, conhecer e saber como funcionam. são apropriadas para as CARTEIRAS PÚBLICAS dos clientes.

para a CARTEIRA PRIVADA do sistema estou estudando um modelo de assinatura mas antes tenho que estudar as vulnerabilidades da própria carteira e do modelo de assinatura para verificar se não vão existir “buracos”.

já para o CLIENTE (consumidor, empresario ou até lojista) se cadastrar no sistema, ele vai informar sua carteira de moedas, a carteira que o sistema vai reconhecer como sendo do CLIENTE e assim poder creditar e debitar corretamente os fundos na CARTEIRA PÚBLICA.

ou não! talvez o sistema simplesmente informe para o cliente qual a sua CARTEIRA PÚBLICA foi gerada pelo sistema e ele poderá receber créditos de qualquer um e também poderá enviar créditos para qualquer um. de qualquer modo, seria mais seguro ter registrado internamente a CARTEIRA PESSOAL DO CLIENTE e manter as transações com a CARTEIRA PÚBLICA trancadas naquela!

podemos proteger o cliente e sua carteira e de modo geral o acesso ao SISTEMA da seguinte maneira:

formas de se proteger chaves, senhas e outros recursos:

* 2FK - chave de dois fatores, ou seja, um token gerado pelo servidor onde queremos nos autenticar, enviado por outro meio diferente do meio que estamos usando para tentar o acesso (uma página web ou um aplicativo de celular via socket). o meio alternativo pode ser:
    * via SMS por exemplo (pode ser caro, as operadoras do Brasil não fornecem pacotes de SMS para empresas, de devemos comprar via brokers de SMS):
        * A Sony e diversas empresas usam esta opção.
    * via aplicação proprietária:
        * um aplicativo instalado no celular do cliente que recebe a mensagem utilizando um serviço proprietário e obscuro criado pelos desenvolvedores:
        * a Apple e o Google usam esta opção.
    * vulnerabilidade: o ataque de espiões do Irã usou uma combinação de “phishing” e “watering hole” para criar um site falso de atualização de senha no google e enquanto o usuário entra com seus dados no site falso o espião reproduz tudo, inclusive o PIN que o google envia para o celular da vítima, no site real do google. o resultado é uma sessão aberta do google no computador do espião e uma vítima que acredita que acabou de mudar sua senha com segurança;
        * uma maneira fácil mas não muito efetiva de evitar isso é observar acessos simultâneos à mesma conta com IP’s diferentes;
        * observar que sempre acessamos a mesma conta simultaneamente quando usamos um desktop e o celular está ligado;
        * podemos desligar as sessões se os IP’s das mesmas forem diferentes (se o cliente estiver com desktop e celular na mesma rede o IP será igual (vale o IP do roteador que ele estiver usando);
        * se o cliente usar 3G no celular ao mesmo tempo que o wifi no desktop vai ser desligado!
        * é um jeito fácil de testar para ver se o sistema de segurança funciona!
* gerador de tokens - um dispositivo que gera tokens aleatórios e que são validados pela aplicação, ou seja, a aplicação só funciona se o dispositivo estiver presente gerando seus tokens:
    * o padrão ->[FIDO U2F Protocol](https://fidoalliance.org)<- permite que várias aplicações sejam compatíveis com o dispositivo;
    * muito usado por bancos (tenho geradores de tokens do ITAÚ e do Bradesco);
    * vulnerabilidades: a pessoa pode simplesmente perder o dispositivo (alguns são tão pequenos que podem desaparecer até dentro de uma carteira);
    * um exemplo popular é a ->[Yubico](https://www.yubico.com)<-.

segundo o modelo de negócio de Coura&Edizon, podemos designar:
SMS ou aplicativo proprietário gerador de tokens para os clientes “C” (consumidores?), ou seja, pessoas que compraram WiBX para gastar com produtos e serviços ou que ganham WiBX como parte de promoções e em ações de marketing;
* tokens com dispositivo para os clientes “E” (empresários?), ou seja, empresas que compraram WiBX para distribuir como prêmios, incentivos para compra, ações de marketing, prêmios para os funcionários por bom comportamento, etc.
* os lojistas (clientes “L”? não estão representados na figura abaixo) que estão aceitando WiBX poderão ter uma das duas formas dependendo de quanto de WiBX estão movimentando.

em outras palavras, o SMS ou aplicativo proprietário seria para os “peixes pequenos”, e o dispositivo de tokens seria para os “peixes grandes”!

e os peixinhos - aqueles que somente estão na rede social do cliente "C" e não são clientes da WiBX e que receberam um link para dar like, visitar o site de um produto e eventualmente comprar o produto usando um código que deu crédito em WiBX para o cliente "C"?

a GDPR ou LGPD permite guardar os dados desse "não cliente"?

> NÃO sem permissão explícita dele! ou seja, ele tem que ser informado de quais dados (IP, e-mail ou ID do facebook, twitter, GMail, Youtube ou seja lá o que for que estiver sendo usado, e URL's da página que está visitando (do produto) estão sendo guardados pela WiBX, porque a WiBX está fazendo isso, e pedir o consentimento explícito para fazê-lo).
