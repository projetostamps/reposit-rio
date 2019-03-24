#### Testes em produção, algumas recomendações:

* Se for possível segmentar geograficamente ou por algum tipo de segmentação de mercado, uma de cada vez, melhor;
* O teste deve ocorrer nos horários e dias quando o negócio tem menor taxa de operações com clientes e parceiros;
* Se possível o teste deve prover uma identificação dos agentes de testes;
* Notificar os provedores (Akamai, Amazon, CDN's) e provedores:
    * A Amazon em particular possui formulários para notificar a execução de testes.
* As sessões de testes não devem se prolongar indefinidamente, tanto para não entrarem no horário de operação da movimentação de clientes quanto para não acumular um volume grande de mais de informações a serem analisadas;
* Os procedimentos de teste devem ser sólidos:
    * O nível de conhecimento exigido por esse tipo de atividade é muito alto;
    * A quantidade de recursos tecnológicos necessários pode ser sofisticada e relativamente cara:
        * Software - existem muitas opções _open source_ disponíveis, mas as melhores ferramentas implicam em algum custo;
        * Hardware - criar e configurar servidores que servirão de "armadilhas" para potenciais vítimas é prática comum dos criminosos e devem ser adotadas também pelos investigadores;
* Os investigadores não deveriam ser artificialmente "censurados" para realizar seu trabalho:        
    * O tipo de teste é "prospectivo", ou seja, não existem alvos pré-determinados nem procedimentos pré-estabelecidos:
        * Há uma metodologia estabelecida para a realiação dos testes;
        * Do mesmo modo que os criminosos não possuem objetivos específicos (os objetivos são gerais, tais como: obter informação de valor, comprometer serviços executando em servidores, introduzir informações falsas que irão produzir algo de valor ou dano para a vítima), os investigadores não deveriam ser "tolhidos" com barreiras artificiais.

#### Algumas possíveis vulnerabilidades observadas:

* O servidor de e-mail wiboo.com.br possui portas demais abertas:
    * Já o servidor de e-mail wibx.io somente abre a porta para e-mail, o que seria o ideal.
* Remover informações pessoais do registro dos sites:
    * As informações de **wibx.io** no banco de dados WHOIS foram removidas no dia 21-fev;
    * Porém as informações de **wiboo.com.br** ainda estão públicas:
        * Não sabemos se o **registro.br** possui o recurso de ocultar essa informação; mas
        * É possível usar um mecanismo de terceiros para manter a propriedade do nome.
* Fica a critério do cliente manter o acesso via Linkedin a toda equipe no portal **wibx.io**, porém é nossa obrigação fazer o alerta de que este pode ser um foco para ações de engenharia social.

#### Recomendações que já foram seguidas pela Equipe Técnica depois do trabalho realizado nestas três últimas sprints

* Removidas informações privadas do registro de domínio **wibx.io**;
* A Equipe Técnica reforçou a regra de utilização de **Canais Oficiais** para a comunicação interna.

#### Algumas observações:

* Esta investigação conta com recursos limitados do ponto de vista tecnológico, como por exemplo:
    * Não temos tempo nem recursos para criar servidores para _watering hole_ e outras práticas de _phishing_;
    * Não "ousamos" utilizar **EternalBlue** para comprometer máquinas de usuários e que também podem ser atingidos primeiro via _phishing_ e em seguida com o uso desta ferramenta comprometer sua máquina (se for Windows, e em alguns casos, Linux).

#### Um roteiro de ataque usando EternalBlue

* Enviar um _phishing_ para uma das vítimas que **SMB**, um protocolo de compartilhamento de arquivos em rede utilizado tanto pelo Windows quanto pelo Linux;
* Este phishing vai usar **XSS** para ativar o javascript do BeEF - o único objetivo deste exercício é determinar o IP da vítima;
* Em seguida podemos utilizar o **Metasploit** com **MS17** (a exploração do **EternalBlue**) e atacar o IP da vítima.
* Abrimos uma janela de linhas de comando no computador da vítima e iniciamos a busca por alguma informação interessante:
    * Lembretes de senhas;
    * Endereços de e-mail;
    * Aplicações vulneráveis;
    * etc.

#### Problemas para realizar este exercício

* Falta de perícia para um exercício efetivo de invasão:
    * Profissionais que trabalham com este tipo de atividade possuem fontes atualizadas de informaçõe; mas também
    * Acesso à vulnerabilidades que não estão disponíveis para o público geral; algumas até como as conhecidas como
    * _Zero Day Vulnerabilities_, ou **Vulnerabilidades Dia-Zero**, em outras palavras, vulnerabilidades tão recentes que muitas vezes nem os desenvolvedores às conhecem:
        * Em algumas ocasiões podem ser evitadas por sistemas de _firewall_ ou _IDS_ efetivos.
* Falta de tempo e recursos necessários:
    * Alguns dos exercícios exigiriam a disponibilidade de servidores 24x7 e registro de domínios, como o _watering hole_;
* Falta de autorizações e permissões necessárias:
    * As permissões vão além da empresa objeto de invasão, e muitas vezes envolve parceiros, clientes e fornecedores;
    * A Amazon e provedores de CDN devem ser notificados;
    * Datas e horários específicos devem ser programados para a realização dos exercícios;
    * É importante notar que a empresa deve possuir uma política clara de utilização de equipamentos de computação pessoais para realizar atividades profissionais ligadas à ela:
        * Para evitar litítigios envolvendo os profissionais da empresa e a própria caso haja uma invasão de privacidade provocada por profissionais de testes ou até mesmo por criminosos.
