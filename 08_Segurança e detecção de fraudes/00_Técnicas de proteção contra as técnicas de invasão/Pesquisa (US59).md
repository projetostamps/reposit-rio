#### O que já vimos

##### Como ocorrem os ataques

* A maioria dos ataques utiliza vulnerabilidades conhecidas a vários anos;
* Muitos usuários ainda utilizam sistemas desatualizados e vulneráveis;
* Uma grande maioria dos acessos à internet se faz por meio de dispositivos móveis;
* A maior parte dos ataques utiliza como método a engenharia social.

##### O que pretendemos (ou não) mostrar agora

* Defesas efitivas contra ataques via engenharia social não são possíveis:
    * Listar proteções mínimas que "podem" inibir ataques.
* Não vamos mostrar o mínimo que se deve fazer para se defender, como por exemplo:
    * Manter sistemas operacionais e aplicativos atualizados;
    * Usar anti-virus;
    * Usar _firewall_ em redes corporativas;
    * Usar _VPN's_ para acessos remotos;
    * Usar _SSH_ para acessar servidores;
    * Segregar redes com os sistemas de _backoffice_ da integração com fornecedores e clientes externos;
    * Usar **DMZ** para os portais internet; etc.

#### Antes de mais nada uma palavrinha sobre "DoS", "DDoS" e "DNS Hijack"

Muitas vezes encontramos na imprensa especializada menções à estes dois tipos de ataques. **DoS** basicamente significa "_Denial of Service_" (do inglês Negação de Serviço) e a variante **DDoS** é a versão distribuída do ataque.

O que o **DoS** faz basicamente é inundar um sistema com requisições especialmente construídas explorando vulnerabilidades de servidores de aplicação ou sistema operacionais ou simplesmente inundar com um número excessivo de requisições simples de modo a tornar o sistema inacessível por usuários legítimos.

O objetivo principal de um **DoS** é justamente impedir que usuários legítimos tenham acesso à determinado sistema. O objetivo secundário pode ser desde impedir que certas pessoas tenham acesso à determinada informação (censura) ou obrigar as pessoas a acessarem um sistema alternativo que tenha sido comprometido previamente pelos atacantes e assim estes ganhariam acesso e controle de informações de suas vítimas.

Para se proteger de ataques **DoS** ou **DDoS** existem serviços como o da [CloudFlare](https://www.cloudflare.com) que promete mitigação ilimitada contra este tipo de ataque.

Já o **DNS Hijack** é um ataque ao painel de controle de provedores e por meio deste as informações DNS de clientes são adulteradas para desviar o tráfego do site legítimo para um site falso que por sua vez irá capturar informações como senhas por exemplo e em seguida desviar para o servidor original, de modo que a adulteração fica imperceptível a não ser por um aumento marginal do tempo de resposta das requisições difícil de discernir se de origem natural ou criminosa.

Este ataque pode se perpetrar comprometendo-se a senha administrativa do cliente no provedor de registro de domínio - fácil se o provedor utilizar um mecanismo de autenticação básico de usuário e senha, ou do próprio provedor.

Para se proteger de um **DNS Hijack** é necessário que o provedor do registro de domínio utilize medidas de segurança eficazes, como autenticação por dois fatores ou o uso de **DNSSEC**, que por sua vez é uma implementação segura do serviço **DNS**.

#### Proteção de ataques de "phishing" e "whaling"

**Lembrando**:
> "Phishing" é a técnica de, usando e-mail ou qualquer outro tipo de correspondência eletrônica, o atacante tenta induzir a vítima a fornecer qualquer tipo de informação que possa trazer algum lucro ou entrar em um sistema no qual a vítima tem acesso.

Algo que jamais deve ser esquecida ao se observar o comportamento dos criminosos quando desejamos desenvolver uma proteção de seus ataques: _eles **sempre** irão atacar o ponto mais fraco da organização e o espectro de ataque é **sempre** amplo e pode atingir elementos e recursos com os quais interagimos indiretamente_.

##### Modos de "phishing"

* O "Phising" pode ser focalizado num grupo específico de pessoas ("spearfishing") como por exemplo o departamento comercial de uma empresa, torcedores de um time, etc.;
* Pode ser generalizado onde o atacante utiliza um clone de uma mensagem genérica como a de atualização de senha de acesso à conta bancária ou do próprio e-mail das vítimas; ou
* "Whaling", onde o atacante foca numa vítima específica, geralmente altos executivos ou dirigentes de organizações públicas ou privadas, figuras políticas importantes, etc.

##### Passos de um ataque

1. Identificar as vítimas;
1. Configurar vetores de ataque;
1. Distribuir ataque;
1. "Fisgar" as vítimas;
1. Lucrar.

**Um ataque em várias fases**

> Ao fisgar a vítima no ataque genérico acima, o atacante ainda poderá:

1. Infiltrar nos sistemas informatizados ou e-mail das vítimas;
1. Obter informações que possam levar a um ganho:
    * número de contas bancárias e senhas para efetuar transferências; ou
    * mecanismos para trocar estas contas bancárias de clientes ou fornecedores poara outras de controle do atacante;
    * lançar um ataque ao sistema de contas à pagar por exemplo para obter recursos com as contas adulteradas.

**Um ataque "armado"**

> O e-mail ou mensagem, pode conter um anexo que contém um "trojam" ou "malware" que por sua vez irá explorar vulnerabilidades conhecidas de browsers e sistemas operacionais para abrir acesso ao atacante. Para funcionar a vítima precisa baixar o anexo e abri-lo ou executá-lo em sua máquina.

##### Falsas promessas de proteção contra "phishing"

1. Produtos e serviços que prometem proteção contra "phishing" não são 100% efetivos:
    * links falsos criados pelos atacantes duram em geral 40h, tornando difícil, senão impossível, determinar vetores de ataques;
    * maior parte das soluções somente "enxergam" o mercado norte-americano e parte do mercado europeu, sendo menos eficazes em identificar ataques envolvendo engenharia social voltados ao público de outras regiões como o Brasil por exemplo.

##### Perspectivas da proteção contra "phishing"

1. Proteger a si próprio, seus colegas, colaboradores e executivos de sua organização;
    * Proteger familiares.
1. Proteger seus clientes, fornecedores e parceiros comerciais; e
1. Proteção genérica para todos os espectros de possíveis vítimas.

**Proteção genérica**

1. Anti-virus são capazes de deter "quase" 100% dos "trojans", "malwares" e "anexos maliciosos" em e-mails e mensagens;
1. Nunca clicar em links de e-mails e mensagens, mesmo quando parecem autênticos;
1. Se a mensagem com o link for esperada (por exemplo, você acabou de pedir para mudar a sua senha do GMail), copie o link e o cole numa aba de seu browser e observe se ele é autêntico:
    * deve começar com HTTPS ou invés de simplesmente HTTP;
    * deve conter o domínio correto (neste exemplo: google.com);
    * o browser não deve mostrar nenhuma mensagem relacionada a certificados vencidos ou com problemas.
1. Verificação de senhas em dois passos (ver US09);
1. Não re-utilizar senhas para não comprometer vários serviços quando um deles for atacado com sucesso.

**Proteção da organização**

1. Estabelecer canais oficiais para troca de informações de toda natureza:
    * quem deve fornecer ou receber informações financeiras, técnicas, administrativas, comerciais, etc.;
    * quais os meios que estas informacões devem trafegar: e-mail, correspondência física, etc.
1. Treinamento dos funcionários em boas práticas para lidar com todos os canais de comunicação:
    * nunca fornecer qualquer tipo de informação corporativa fora dos canais oficiais;
        * se você não pertence ao grupo que deve receber ou fornecer uma determinada informação, recusar fazê-lo e comunicar o grupo pertinente quando for instigado a burlar um dos canais oficiais;
    * nunca fornecer nenhum tipo de informação pessoal sua ou de colegas para clientes, fornecedores ou parceiros comerciais da empresa onde trabalha, tais como número de documentos, data de nascimento, residência, etc.;
    * fazer simulações e reforçar orientações de acordo com o índice de adesão às boas práticas.

**Proteção de clientes, fornecedores e parceiros comerciais**

1. Informar clientes, fornecedores e parceiros comerciais os canais de comunicação adequados para cada caso e estabelecer claramente as políticas com relação às informações que podem e não podem estar presentes em cada canal.

#### Proteção de ataques de "pretexting" ("phishing" na vida real)

**Lembrando**:
> "Pretexting" é o ataque onde o atacante tenta acessar fisicamente a organização se posando como funcionário de um fornecedor, geralmente de telefonia ou serviço internet, mas pode ser da empresa de águas ou de gás, e uma vez dentro do estabelecimento ele procura comprometer sistemas informatizados ou de telefonia instalado equipamentos de escuta ou captura de dados, muitas vezes com acesso remoto.

##### Modos de "pretexting"

* O "farsante" tem de estar fisicamente presente nas instalações do alvo, seja uma residência, seja uma organização, e do mesmo modo que o "phishing" seu alvo pode ser um alto-executivo e portanto até vale posar como um "limpador de piscinas" para acessar a casa dele;
* O ataque não precisa ser necessariamente por meio de um "falso operário", o atacante pode se apresentar como um "executivo de vendas", um "advogado" ou até mesmo um "fiscal do governo".

##### Perspectivas da proteção contra "pretexting"

1. Proteger seus colegas, colaboradores ou executivos de sua organização;
1. Proteger seus clientes, fornecedores e parceiros comerciais.

**Proteção genérica**

1. Agendamento prévio de todo e qualquer agente externo visitando as instalações da empresa;
1. Acompanhamento de todo e qualquer atente externo enquanto estiver dentro do estabelecimento;
    * Se um funcionário não puder acompanhar, um segurança deve ser chamado para fazê-lo.
1. As credenciais e documentos devem sempre ser verificados:
    * Por conta do GDPR e LGPD os documentos jamais devem ser retidos e as informações coletadas devem ser autorizadas pelos seus portadores.

**Proteção da organização**

> Existem nomras ISO para proteção da informação que podem ser referenciadas e usadas como guia: ISO 27002.

1. Se a organização está instalada num condomínio comercial, esta deve ter acesso ao cadastro de visitantes;
    * Do mesmo modo, o condomínio deve ter acesso à agenda da organização para autorizar a entrada de visitantes;
    * Observar que muitas vezes os atacantes declaram na entrada que estão indo para um lugar quando na verdade o objetivo é outro - os atancantes sempre buscam o elo mais fraco e se no condomínio comercial houver clínicas e escritórios de profissionais liberais estes serão seu vetor de entrada preferencial.

> Lembrando: não existe limite para a amplitude dos ataques, os criminosos por exemplo podem comprometer a rede Wi-Fi de um estabelecimento da Starbucks freqüentado por funcionários de um fornecedor da organização alvo ou até mesmo a rede Wi-Fi doméstica de alguns deles.

**Proteção de clientes, fornecedores e parceiros comerciais**

> Existem normas ISO para proteção de áreas sensíveis que podem ser referenciadas: ISO 27001 e ISO 27003.

1. Estabelecer protocolos claros para a visita de parceiros, tanto vindos de fora para dentro da organização quanto e vice-versa;
    * Toda visita deve ser agendada;
    * Nome e documento de identificação devem ser fornecidos previamente;
    * Visitantes devem sempre constar na agenda com data e horário a serem conferidos na recepeção;
    * Todo visitante deve ser acompanhados por todo o período da visita;
    * Somente funcionários devidamente treinados e qualificados podem acompanhar visitantes nas instalações da organização;
    * Nenhum visitante deve instalar equipamentos nas redes de telefonia, computação e elétrica da organização;
    * Instalações segregadas devem ser criadas para instalação de equipamentos para fins de demonstração e testes;
    * A ligação com redes de telefonia e computação externos de clientes, fornecedores e parceiros deve ser feita por meio de VPN's:
        * a VPN deve ser instalada e configurada dentro dos sistemas da empresa e não contratada externamente para garantir a segurança, a performance e a monitoração.
    * As redes compartilhadas com clientes e fornecedores devem ser lógica e fisicamente separadas das redes e sistemas computacionais da organização;
        * A interligação das redes de parceiros com sistemas internos, quando necessária, deve ser feita por meio de gateways com sistemas de _logging_ e circuitos disjuntores (_circuit breakers_) programados para detectar desvios das funções previamente esperadas.
    * As visitas não devem se extender para além do horário comercial.

#### Como detectar uma invasão

Com tantos vetores possíveis e dada a abrangência dos ataques que podem ser diretos ou indiretos, com os criminosos usando clientes, fornecedores, familiares e até autoridades para ganhar acesso às nossas instalações e sistemas e com isso obter ganhos, mesmo com todas as medidas aqui sugeridas ainda assim sempre vai existir a possibilidade de uma falha, nem que momentânea, que pode e será explorada pelos criminosos se eles perceberem e estiverem empenhados no ataque.

Após a prevenção devemos a pensar nas contra-medidas e meios de detecção para, quando a invasão ocorrer, podermos nos defender, e se a defesa não for possível, ao menos remediar os danos causados.

Podemos, nos sistemas **Linux**, executar alguns comandos e analisar _logs_ para avaliar as conseqüências de uma possível invasão:
* **last** - este comando relata os _logins_, _logouts_, _reboots_ e mudanças de _run level_;
* **lastlog** - comando que lista os _logins_ mais recentes de todos os usuários;
* _/etc/syslog.conf_ - mostra a configuração de seus arquivos de _log_;
* _/etc/rsyslog.conf_ e pasta _/etc/rsyslog.d_ - configuração de _logs_ em sistemas Ubuntu;
* _/var/log/auth.log_ - log de autenticação de usuários; etc.

Em seguida podemos analisar o _log_ privado _.bash_history_ para observar o que o invasor pode ter feito, porém sempre vale o alerta: um _hacker_ de verdade irá também apagar todos os traços de sua "visita".

Já em sistemas **Windows** existe um console administrativo e neste o _Event Viewr_ que agrega todos os logs do sistema, inclusive o que se refere à autenticação de usuários.

Porém o uso de ferramentas especializadas é a melhor opção para se executar este tipo de análise, e em alguns casos estas ferramentas podem adicionar métodos preventivos importantes para a defesa contra ataques.

##### Sistemas de detecção de invasão

Existem dois tipos de sistema de deteção de invasão:
1. HIDS - Host Intrusion Detection System (Sistema de Detecção de Invasão baseado em Servidores); e
1. NIDS - Network Intrusion Detection System (Sistema de Detecção de Invasão baseado em Redes).

**HIDS**

Os sistemas HIDS examinam eventos no computador ao invés do tráfico de rede que entra e sai do computador. Basicamente este tipo de proteção funciona monitorando os arquivos administrativos do sistema de arquivos do computador que está sob sua proteção. Estes arquivos incluem _logs_ e arquivos de configuração.

Um HIDS faz:
1. Backups dos arquivos de configuração permitindo recuperar o sistema caso alguma ação maliciosa tenha tentado aliviar as medidas de segurança modificando parâmetros de operação;
1. Impede ou restringe acesso **root** ao sistema (ou Administrativo em máquinas Windows).

Para ser eficaz todos os computadores devem possuir este tipo de software instalado eliminando assim vetores indiretos de ataque:
    * Um sistema HIDS distribuído precisa ter um módulo de controle central para administração, configuração e monitoramento das defesas.

**NIDS**

Os sistemas NIDS examinam o tráfego da rede local e o tráfego entre a rede local e a internet. Os sistemas NIDS são baseados em _packets sniffers_ (cheiradores de pacotes) que capturaram o tráfego de rede para análise posterior. O mecanismo de análise de um NIDS é baseado em regras e regras novas podem ser adicionadas para aperfeiçar o sistema de proteção. Como é impossível capturar todo o tráfego de uma rede para análise, o sistema de regras trabalha de modo seletivo.

Geralmente o NIDS é instalado num equipamento dedicado com poder de processamento suficiente para a quantidade de análise que será demandada dele.

##### Qual devemos usar

A resposta curta e certeira é: ambos.

Um sistema NIDS irá dar um grande poder de monitoramento e permite interceptar ataques antes que eles aconteçam, enquanto que o HIDS só irá notificar que algo aconteceu depois do fato. Por outro lado, como vimos aqui, ataques originados exclusivamente da internet por meio de mecanismos de invasão altamente sofisticados são raros, difíceis de executar e demandam muitos recursos dos atacantes, enquanto que mecanismos de engenharia social são bastante eficazes em comprometer sistemas, e neste caso um sistema HIDS é a melhor defesa.

##### Tipos de detecção

Os dois sistemas de monitoramento utilizam mecanismos de detecção semelhantes:
1. Signature-based IDS - identificação baseada em assinatura; e
1. Anomaly-based IDS - identificação baseada em anomalia.

**Signature-based IDS**

O método de detecção baseado em assinatura verifica _checksums_ de mensagens ou de arquivos.

Um HIDS por exemplo irá analisar um registro interno dos arquivos de configuração de um sistema e observar se os _checksums_ destes arquivos se mantém desde a última verificação.

Já um NIDS irá observar _checksums_ de pacotes de dados transmitidos pela rede e verificar a integridade das mensagens. O NIDS pode ter um banco de dados de assinaturas de pacotes conhecidos por serem originados de fontes maliciosas. Diferente do que vemos em filmes, os criminosos não ficam sentados em frente ao computador digitando loucamente programas tentando quebrar senhas e invadir sistemas, mas utilizam ferramentas automatizadas criadas por _black hat hackers_ e estas ferramentas possuem características comuns que ao serem analisadas por _white hat hackers_ estes determinam os padrões do ataque e assim geram assinaturas que permitirão sua detecção.

Neste último caso a atualização constante desse banco de dados é de suma importância.

**Anomaly-based IDS**

O método de detecção baseado em anomalias cria um registro do comportamento considerado "normal" e ajusta fronteiras para permitir o funcionamento do sistema dentro de parâmetros considerados "seguros" e configurados pelos usuários.

Tanto sistemas HIDS quanto NIDS podem implementar este tipo de detecção. Para um HIDS por exemplo, várias tentativas frustradas de login por um usuário ou atividade incomum sobre um determinado arquivo podem gerar um alerta.

No caso de um NIDS, a identificação de anomalia depende de se estabelecer uma _baseline_ do que seja o comportamento "normal" de modo a permitir a comparação de padrões de tráfego de rede que possam ser identificados como "anormais". Dentro dos parâmetros considerados "seguros" o tráfego é declarado "normal", porém se o tráfego sobe ou desce além dessa faixa um alerta é gerado. Os parâmetros devem levar em consideração não só variáveis quantitativas, mas também qualitativas, ou seja, o tipo de tráfego, mas também temporais, ou seja, horários e dias das ocorrências.

##### Qual devemos ter instalados

Mais uma vez a resposta curta e mais acertada é: ambos.

Mecanismos de detecção baseados em assinatura são mais rápidos que os baseados em anomalias, porém um sistema baseado em anomalias com talvez até o auxílio de mecanismos de aprendizado de máquina pode detectar vulnerabilidades ainda desconhecidas pelos especialistas.

##### Sistemas de detecção de intrusão vendidos pela Amazon Web Services

A própria Amazon oferece produtos e serviços próprios e de terceiros voltados para a proteção de seus clientes.

Da própria Amazon temos:

| Sistema | Descrição |
| --- | --- |
| [Amazon GuardDuty](https://aws.amazon.com/guardduty/) | Sistema de detecção continua que monitora atividades nos containers EC2 e produz alertas quando observadas atividades suspeitas |
| [Amazon Inspector](https://aws.amazon.com/inspector/) | Sistema de auditoria da instalação e configuração de outros serviços AWS que produz relatórios de vulnerabilidades encontradas baseados em regras pré-definidas

Além destes sistemas a Amazon lista algumas soluções de terceiros em seu "Marketplace":

| Sistema | Fornecedor | Observações |
| --- | --- | --- |
| [Alert Logic SIEMLess Threat Management](https://aws.amazon.com/marketplace/pp/B07K2J16QH) | [Alert Logic](https://www.alertlogic.com/press-releases/alert-logic-introduces-siemless-threat-management-to-help-resource-constrained-organizations-get-the-right-coverage-at-an-optimal-cost/) | Aparentemente disponível apenas para o mercado (ou servidores) Norte Americano, a ferramenta parece ser relativamente nova.<br/>Serviço oferecido exclusivamente como SaaS (Software as a Service). |
| [Trend Micro Deep Security](https://aws.amazon.com/marketplace/pp/B01AVYHVHO/ref=mkt_ste_categtm_adc) | [Trend Micro](https://www.trendmicro.com/en_us/business.html) | Não foi encontrada muita informação sobre a ferramenta no site do fabricante.<br/>Serviço oferecido tanto na modalidade BYOL (Bring Your Own Licensing) como SaaS. |
| [MacAfee Virtual Network Security Platform](https://aws.amazon.com/marketplace/pp/prodview-aacz43ofn4a3s) | [MacAffee](https://www.mcafee.com/enterprise/en-us/products/virtual-network-security-platform.html) | Serviço oferecido tanto na modalidade BYOL como SaaS. |

A maioria destes produtos tendem a reforçar aspectos de produtos "closed source":
* informações somente disponíveis por meio de aquisição de "treinamento" ou "consultoria";
* por serem soluções "proprietárias" tendem a não respeitar padrões e a manter os clientes "reféns" de sua instalação, característica da maioria das soluções SaaS que rodam sob a Amazon;
* deve-se ponderar o custo da solução aberta contra a proprietária levando em consideração não só o aluguel do software (no caso da solução proprietária) mas também:
    * espaço de armazenamento para o "backup" de arquivos, _logs_ e configurações, etc.;
    * transferência de dados entre servidores para gerenciamento e inspeção de pacotes de rede;
    * o uso ou não de instâncias reservadas ou instâncias do tipo "Spot".
* a vantagem dos sistemas próprios da Amazon ou dos fornecidos por terceiros (verificar disponibilidade) é a integração com o console AWS e com o sistema de configuração de regras da Amazon, e alguns inclusive com o Amazon GuardDuty e Amazon Inspector:
    * pressupõe-se ainda que existe uma equipe de especialistas observando as instalações e que poderiam eventualmente agir pro-ativamente caso observem algo fora de normal;
    * também pressupõe-se que o sistema esteja sempre atualizado e protegido contra as vulnerabilidade e táticas mais recentes aplicadas pelos criminosos.
* antes de utilizar as soluções SaaS deve-se observar se são aderentes às normas GDPR e LGPD.

##### Sistemas de deteção de intrusão disponíveis para o mundo

Para construir nossa lista consideramos as soluções disponíveis para Linux, com a disponibilidade para Windows e MacOS um plus. No caso da disponibilidade para Windows e MacOS, consideramos apenas a disponibilidade de um *cliente* ou *agente* rodando nestas plataformas e não a necessidade de se ter um serviço gerenciador.

| Sistema | Tipo | Windows | MacOS |
| --- | :---: | :---: | :---: |
| [Snort](https://www.snort.org) | NIDS | Sim | Não |
| [OSSEC](https://www.ossec.net) | HIDS | Sim | Sim |
| [Suricata](https://suricata-ids.org) | NIDS | Sim | Sim |
| [Bro](https://www.bro.org) | NIDS | Não | Sim |
| [Wazuh](https://wazuh.com) | HIDS | Sim | Sim |
| [Sagan](https://quadrantsec.com/sagan_log_analysis_engine/) | NIDS/HIDS | Não | Sim |
| [Security Onion](https://securityonion.net) | NIDS/HIDS | N/A | N/A |
| [AIDE](http://aide.sourceforge.net) | HIDS | Não | Não |
| [Open WIPS-NG](http://openwips-ng.org) | NIDS | Não | Não |
| [Samhain](https://la-samhna.de/samhain/) | HIDS | Não | Não |
| [Fail2Ban](https://www.fail2ban.org/wiki/index.php/Main_Page) | HIDS | Não | Não |
| [Sguil](https://bammv.github.io/sguil/index.html) | NIDS | Sim | Sim |

De todas as soluções, destacamos o **Security Onion** por se tratar na verdade de uma distribuição Linux com uma série de aplicações voltadas à detecção de intrusão e gerenciamento de _logs_ pré-instaladas:
* [ElasticSearch](https://www.elastic.co/products/elasticsearch) - Mecanismo de busca e análise;
* [LogStash](https://www.elastic.co/products/logstash) - Mecanismo de processamento de dados que obtém dados de várias fontes e os roteiam para um repositório central (neste caso o ElasticSearch);
* [Kibana](https://www.elastic.co/products/kibana) - Ferramenta de visualização de dados armazenados no ElasticSearch;
* [ELSA](https://github.com/mcholste/elsa) - Receptor de _logs_, arquivador, indexador e _frontend_ WEB;
* [Snort](https://www.snort.org) - Ferramenta NIDS;
* [Suricata](https://suricata-ids.org) - Ferramenta NIDS;
* [Bro](https://www.bro.org) - Ferramenta NIDS;
* [Wazuh](https://wazuh.com) - Ferramenta HIDS;
* [Sguil](https://bammv.github.io/sguil/index.html) - Ferramenta NIDS;
* [Squert](http://www.squertproject.org) - Frontend WEB para o *Squil*;
* [CyberChef](https://github.com/gchq/CyberChef) - Console de operações de análise de dados criptografados;
* [NetworkMiner](https://www.netresec.com/?page=networkminer) - Ferramenta de análise forense do tráfego de rede;
* [Xplico](https://www.xplico.org) - Ferramenta de análise forense do tráfego de rede (ver [US63 - Detecção de falhas inerentes à degradação de redes](https://docs.google.com/document/d/1XhYcxbxhgHDJzTKKCiLI3M-NUWe7jqqN99zK9ttzZjQ/edit?usp=sharing), sprint 201705, Fábio&JGomes).

##### Outras ferramentas

Atualmente, com a adoção de diversos projetos de código-livre disponíveis na internet em sistemas corporativos ou aplicações de missão crítica, não só estamos adotando soluções prontas que aceleram a colocação de novos produtos e serviços no mercado, mas também estamos nos expondo a um novo vetor de ataque, ou seja, o código-livre pode ter sido adulterado por criminosos que "contribuiram" com o projeto, em grande parte até de modo legítimo, porém com objetivos maliciosos.

Não que o fato de usarmos apenas código fechado seja uma garantia, pois até o código fonte do Windows da Microsoft já foi roubado e usado por criminosos diversas vezes no passado, porém a nossa dependência cega destes recursos pode vir a ser um grande problema principalmente em se tratando de projetos de código aberto com poucos ou muitas vezes nenhum contribuidor (projetos que foram descontinuados pela equipe original).

O desenvolvedor de um projeto de grande uso e aceitação no mercado pode se sentir esgotado com o trabalho que o projeto demanda e resolve transferi-lo para quem se interessar. O criminoso se candidata, possivelmente até com um perfil e credenciais roubados de outro desenvolvedor relativamente conhecido e que ignora o fato de que foi roubado.

Existem casos de empresas que "compraram" projetos de código livre com o objetivo de instalar _backdoors_ ou _malwares_ nestes projetos com propósitos ilícitos.

Criminosos em posse de projetos de código-livre podem fazer suas alterações, no inicio timidamente e de modo legítimo para não levantar suspeitas da comunidade, que está de olho e muitas vezes aciona o alarme quando observa algum movimento suspeito, porém um dia e quando menos se espera o bandido se prepara para um grande ataque a um alvo específico que já estudou e analisou e está pronto para receber a sua versão especialmente criada - parece até roteiro de um filme sobre _hackers_ sem graça, mas já aconteceu na vida real. Vários _plugins_ do navegador Chrome foram "sequestrados" por meio de "aquisições" ou transferências para outros desenvolvedores interessados em instalar _malwares_ nos computadores de suas vítimas, na maioria das vezes para introduzir mineradores de cripto-moedas ou usar como **zumbis** em redes de para ataques DDoS ou quebrar senhas.

Assim sendo, ferramentas de análise estática de código para localizar este tipo de problema podem se tornar primordiais para quem utiliza código aberto em seus projetos internos.

##### Configuração de um sistema protegido


#### Fui invadido? Fui invadido! E agora?

Como já vimos, não existe proteção completa e total e eventualmente um sistema será invadido. Quanto maior o valor das informações de um sitema, quanto maior for sua divulgação e exposição para o mundo, maior será a probabilidade de ataques.

Sistemas são atacados com maior ou menor grau de sucesso o tempo todo e uma correta resposta após o ataque é tão importante quanto a prevenção.

Com os sistemas sugeridos anteriormente instalados, uma vez detectada uma invasão, mesmo que momentânea ou que não tenha tido sucesso, devemos entrar em alerta e seguir uma série de passos que irão promover uma melhoria contínua das defesas implantadas:

1. Desconectar os sistemas afetados da internet;
1. Modificar todas as senhas de todos os sistemas, todos os computadores e todos os aplicativos;
1. Verificar outros sistemas que estão conectados e que aparentemente não foram invadidos;
1. Se o sistema afetado contém dados pessoais, prosseguir com os procedimentos apontados pela regulamentação GDPR e LGPD;
1. Examine o sistema atacado até:
    * determinar de onde o ataque se originou;
    * por onde o ataque penetrou no sistema;
    * para onde o ataque foi, tentar determinar os objetivos do atacante;
    * uma vez determinados os pontos usados pelo atacante, feche-os imediamente:
        - portas de serviços que não deveriam estar expostos;
        - vulnerabilidades em código de aplicação;
        - lembrar que uma vez encontrada uma vulnerabilidade (um trecho de código vulnerável a SQL Injection por exemplo), este pode ter sido apenas um passo no ataque que ainda tem o banco de dados executando num contexto privilegiado que permitiu acesso a outros elementos do sistema, portanto, todos os passos do atacante devem ser seguidos e todas as falhas documentadas e mitigadas.
1. Faça uma re-instalação de todo o sistema, agora com as correções já aplicadas, antes de reconectá-lo à internet;
1. Restaure um backup anterior ao ataque de todos os dados que este sistema tinha acesso:
    * Usuários terão que ser notificados de que as informações retornaram ao estado que estavam na data e hora do backup.
1. Se o sistema possuia contas de usuários, executar um procedimento para que todos os usuários sejam obrigados a renovarem suas senhas:
    * Aqui vale uma observação importante - imagine que além de comprometer a senha dos usuários, os atacantes comprometeram também o seu método de entrar em contato com estes usuários, seja por telefone, seja por e-mail - isto reforça a razão de se restaurar o backup.

##### Referências

* [Supporters of Mexico’s Soda Tax Targeted With NSO Exploit Links - by John Scott-Railton, Bill Marczak, Claudio Guarnieri, and Masashi Crete-Nishihata - fev 2017](https://citizenlab.ca/2017/02/bittersweet-nso-mexico-spyware/)
* [The curious case of the Raspberry Pi in the network closet - Christian Hascheck's blog - jan 2019](https://blog.haschek.at/2018/the-curious-case-of-the-RasPi-in-our-network.html)
* [Amazon GuardDuty](https://aws.amazon.com/guardduty/)
* [Amazon Inspector](https://aws.amazon.com/inspector/)
* [Alert Logic SIEMLess Threat Management](https://aws.amazon.com/marketplace/pp/B07K2J16QH)
* [Alert Logic](https://www.alertlogic.com/press-releases/alert-logic-introduces-siemless-threat-management-to-help-resource-constrained-organizations-get-the-right-coverage-at-an-optimal-cost/)
* [Trend Micro Deep Security](https://aws.amazon.com/marketplace/pp/B01AVYHVHO/ref=mkt_ste_categtm_adc)
* [Trend Micro](https://www.trendmicro.com/en_us/business.html)
* [MacAfee Virtual Network Security Platform](https://aws.amazon.com/marketplace/pp/prodview-aacz43ofn4a3s)
* [MacAffee](https://www.mcafee.com/enterprise/en-us/products/virtual-network-security-platform.html)
* [Snort](https://www.snort.org)
* [OSSEC](https://www.ossec.net)
* [Suricata](https://suricata-ids.org)
* [Bro](https://www.bro.org)
* [Wazuh](https://wazuh.com)
* [Sagan](https://quadrantsec.com/sagan_log_analysis_engine/)
* [AIDE](http://aide.sourceforge.net)
* [Open WIPS-NG](http://openwips-ng.org)
* [Samhain](https://la-samhna.de/samhain/)
* [Fail2Ban](https://www.fail2ban.org/wiki/index.php/Main_Page)
* [Sguil](https://bammv.github.io/sguil/index.html)
* [ElasticSearch](https://www.elastic.co/products/elasticsearch)
* [LogStash](https://www.elastic.co/products/logstash)
* [Kibana](https://www.elastic.co/products/kibana)
* [ELSA](https://github.com/mcholste/elsa)
* [Sguil](https://bammv.github.io/sguil/index.html)
* [Squert](http://www.squertproject.org)
* [CyberChef](https://github.com/gchq/CyberChef)
* [NetworkMiner](https://www.netresec.com/?page=networkminer)
* [Xplico](https://www.xplico.org)
* [**US63** - Detecção de falhas inerentes à degradação de redes - sprint 201705, FAlves\&JGomes](https://docs.google.com/document/d/1XhYcxbxhgHDJzTKKCiLI3M-NUWe7jqqN99zK9ttzZjQ)
* [Apresentação: Gestão de Continuidade de Negócios - Coura - Curso CE-230 Setembro 2017](https://docs.google.com/presentation/d/1BRLKee91zoFhMASIQ0hZTrP3ZVOlSTHN5ppcfeVxPGQ)
