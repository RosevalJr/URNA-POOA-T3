# Trabalho 03 - Princípios de Substituição de Liskov e Segregação de Interface
- Universidade Federal de São Carlos - Departamento de Computação
- Programação Orientada a Objetos Avançada
- Aluno: Roseval Donisete Malaquias Junior - RA: 758597
- Professor: Delano Medeiros Beder

---

> Para o maior detalhamento desta arquitetura projetada, com explicações mais detalhadas de suas funcionalidades, funcionamento e especificações para sua extensão, o seguinte vídeo explicativo foi gravado e postado no youtube: <https://youtu.be/mXFNUSFyVDY>

Neste repositório, é apresentado o projeto de sistema de software para uma urna eletrônica. No documento de requisitos disponibilizado pelo professor é feita a especificação detalhada do funcionamento de cada funcionalidade, além de especificar quais partes de hardware da urna que serão disponibilizadas para cada funcionalidade. Esse sistema de software é capaz de realizar 4 funções bàsicas. 
- F1: Receber comandos do mesário para iniciar uma nova votação;
- F2: Informar o eleitor sobre a votação;
- F3: Receber o voto do eleitor;
- F4: Registrar o voto do eleitor;

Com o intuito de evitar o design especulativo, foram especificadas extensões esperadas para as funcionalidades F2 e F3 da urna.

Para especificar a descrição arquitetural da solução de software projetada pelo autor, foi produzido um diagrama de classes com a Unified Modeling Language (UML). Esse modelo UML especifica todas as classes idealizadas para o funcionamento do sistema, juntamente com as cardinalidades e respectivas relações entre as classes idealizadas. A seguir será feito e detalhamento de como essas funcionalidades foram idealizadas pelo autor e expressadas no modelo UML de classes produzido. Também, será destacado como o arquitetura idealizada respeita os princípios SOLI do acrônimo SOLID, e pensamentos e técnicas aplicadas para evitar a quebra de certos princípios.

Inicialmente, é importante destacar a estrutura geral da solução arquitetural criada. A classe ``Urna`` foi criada para funcionar como o cliente de todas as funcionalidades projetadas nesta solucação. Essa classe é responsável por instanciar todas as classes desejadas das funcionalidades e executar seus métodos **Strategy**. Sendo assim, todos os diferentes mecanimos são opcionais na urna projetada, tangendo a classe cliente (``Urna``) escolher quais funcionalidades serão executadas. Como pode ser observado na Figura 1, a classe ``Urna`` possui argumentos do tipo **List** para cada uma das classes interface **Strategy**, sendo que nos métodos "Procedimento" apenas é chamado o método **Strategy** de cada uma das interfaces instanciadas como desejado no cliente. No método ``votacao()`` é chamado cada um dos métodos "Procedimento" ligados a execução da votação, por fim o voto é armazenado chamando o seu "Procedimento" respectivo de estratégias de armazenamento interno. Também, é importante destacar que as informações dos candidatos utilizados na votação são inseridas na urna manualmente, utilizando a classe **Strateygy** ligado ao armazenamento interno de cadndidaotos. A seguir será feito uma detalhamento minuncioso da solução propostao no diagrama UML para cada uma das funcionalidades.

![Figura 1](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig1.png)
<div align="center">
  <b>Figura 1: Classe "Urna", utilizada como cliente de todas as funcionalidades.</b>
</div>

# F1: Receber comandos do mesário para iniciar uma nova votação

A primeira funcionalidade faz com que seja necessário uma forma de comunicação entre o mesario e urna. Diante disso, foi idealizada a classe ``MesarioInput`` que é responsavel por receber os comandos do equipamento do mesário através de seu método ``ActionRecebeInput()``, como pode ser observado na Figure 2. Essa classe é utilizada pela classe ``urna`` para ler a entrada do mesário e saber quando a votação deve ser inicializada.

![Figura 2](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig2.jpeg)
<div align="center">
  <b>Figura 2: Classe "MesarioInput", responsável por receber o input do mesário.</b>
</div>

# F2: Informar o eleitor sobre a votação

Para projetar a segunda funcionalidade foi utilizado o padrão de projeto **Strategy**, sendo criada a classe interface ``OutputMostraVotacao`` que contém a assinatura do método **Strategy** que mostra as informações da votação para o eleitor dado um meio de output especifico. Como a urna pode ter diversos meios de output e todos eles devem ser possiveis de utilizar em uma votação, além de possibilitar a extensão para a utilização de novos meios de output para mostrar a votação, foram criadas as classes ``BrailleMostraVotacao``, ``TelaMostrVotacao`` e ``AudioMostraVotacao``. Essas classes são responsaveis por implementar o método **Strategy** para mostrar as informações da votação ao eleitor, utilizando um mecanismo de hardware disponibilizado pela urna. Diante disso, essas classes possuem um argumento da classe que faz a interação com o mecanismo de hardware de urna como a classe ``OutputTela``. Dentro da impleementação do método **Strategy** ``MostraVotacao()`` é implementado como é feita a mostragem das informações da votação, utilizando a classe que faz interação com o hardware da urna como meio de comunicação com o hardware, como pode ser visualizado na Figura 3.

![Figura 3](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig3.jpeg)
<div align="center">
  <b>Figura 3: Utilização do padrão de projeto **Strategy** para arquiteturar a segunda funcionalidade.</b>
</div>


Portanto, as classes envolvidas nesta funcionalidade respeitam o principio SRP, sendo que cada classe possue apenas uma resposanbilidade para com a funcionalidade. Também respeitam o princípio de OCP, sendo que para realizar a extensão de utilização de um novo meio de output para mostrar a votação, simplesmente é precisso criar uma nova classe que herda da classe interface ``OutputMostraVotacao`` e possue como argumento uma classe que interage com o novo mecanimos de hardware projetado. Por fim, essa nova classe irá implementar o método **Strategy** herdado. Para essa nova funcionalidade seja utilizada pela urna, a classe ``Urna`` deve instanciar uma nova instancia da nova classe criada. Dessa forma, quando o método ``ProcedimentoMostraVotacao`` da classe ``Urna`` for chamado, o método **Strategy** implementado da nova classe será chamado e a nova adição a funcionalidade será executada. Além disso, também respeita o princípio de LSP sendo que os contratos entre os tipos das classes são respeitados, qualquer uma das classes que implementam o método **Strategy** substituem a classe interface ``OutputMostraVotacao``. Por fim, também respeita o princípio de ISP, sendo que cada um dos módulos enxergam apenas os métodos que são utilizados, no caso apenas o método **Strategy** herdado.

# F3: Receber o voto do eleitor

A terceira funcionalidade é demonstrada pela parte mais a direita do modelo UML, e assim como a segunda funcionalidade o strategy também foi utilizado para sua idealização. Neste caso tem-se a classe interface que define a assinatura do strategy, as classe que herdam da classe strategy e implementam o strategy, e utilizam a classe que tem contato direto com os meios de input da urna. Diante disso, é possivel realizar a extensão desta funcionalidade dado que é apenas necessário realizar a implementação de uma nova classe que implementa o strategy e ter como argumento a classe com contato direito com o novo input criado na urna. Diante disso, é possivel observar que o contrato entre os tipos e subtipos é respeitando, sendo que o tipo pode ser substituido sobre seu subtipo. os módulos tem acesso apenas a quilo que utiliza dentro da herança do strategy.

# F4: Registrar o voto do eleitor

Por fim, irei fazer um detalhamento de cada um dos principios pensando no projeto como um todo, e como cada funcionalidade é opcional para utilizar a urna eletrônica, possibilitando sua toda flexibilidade na utilização de suas funcionalidades.

Por fim, é possivel observar que o projeto idealizado da urna eletronica respeita os princípios SOLI. (Fazer isso la em cima mesmo)


## Como foram distribuidas as responsabilidades e por que?
## Como o projeto pode ser estendido sem causar impacto no restante da arquitetura?
## O "contrato" de cada tipo é respetado, independente da instância real daquele tipo?
## Como está a dependência entre as classes/módulos/funções? Um móduolo exerga apenas aquilo que precisa do outro?
