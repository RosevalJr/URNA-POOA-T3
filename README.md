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

Inicialmente, é importante destacar a estrutura geral da solução arquitetural criada. A classe ``Urna`` foi criada para funcionar como o cliente de todas as funcionalidades projetadas nesta solucação. Essa classe é responsável por instanciar todas as classes desejadas das funcionalidades e executar seus métodos *Strategy*. Sendo assim, todos os diferentes mecanimos são opcionais na urna projetada. Cabendo ao cliente (``Urna``) escolher quais funcionalidades serão executadas pela urna. Como pode ser observado na Figura 1, a classe ``Urna`` possui argumentos do tipo *list* para cada uma das classes interface *Strategy*, sendo que nos métodos "processo" apenas é chamado o método strategy de cada uma das interfaces instanciadas como desejado no cliente. E no método ``votacao`` é chamado cada um dos métodos "processo" ligados a execução da votação, por fim o voto é armazenado chamando o seu processo respectivo de estratégias de armazenamento interno. Também, é importante destacar que as informações dos candidatos utilizados na votação é inserido na urna manualmente também utilizando o strateygy ligado ao armazenamento interno de cadndidaotos. A seguir será feito uma detalhamento minuncioso da solução propostao no diagrama UML para cada uma das funcionalidades.

![Figura 1](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig1.png)
<div align="center">
  <b>Figura 1: Classe ``Urna``, utilizada como cliente de todas as funcionalidades.</b>
</div>

# F1: Receber comandos do mesário para iniciar uma nova votação

A primeira funcionalidade faz com que seja necessário uma forma de comunicação entre o mesario e urna. Dessa forma, foi idealizada a classe "MesarioInput" que é responsavel por ler os comandos do mesário. Essa classe é utilizada pela classe "urna" para ler a entrada do mesario e souber quando a votação deve ser inicializada.

# F2: Informar o eleitor sobre a votação

A segunda funcionalidade é demonstrada pela parte mais a esquerda no modelo UML, foi utilizado o Strategy para modelar a abstração necessaria que possibilita a extensão desta funcionalidade. Diante disso, existe a classe interface herdada pelas classes que implementam o método strategy, sendo que essas classes utilizam a classe que tem contato direto com o equipamento de hardware da urna, como a tela de cristal e o display de braile. Além disso, importante destacar que o método strategy tem como entrada uma abstração dos dados de texto e imagem, para que possam ser utilizados dentro da implementação do strategy, sendo que no documento é especificado que apenas serão passados imagens e texto no futuro, entao esse tipo armazena esses dois tipos de dados.


# F3: Receber o voto do eleitor

A terceira funcionalidade é demonstrada pela parte mais a direita do modelo UML, e assim como a segunda funcionalidade o strategy também foi utilizado para sua idealização. Neste caso tem-se a classe interface que define a assinatura do strategy, as classe que herdam da classe strategy e implementam o strategy, e utilizam a classe que tem contato direto com os meios de input da urna. Diante disso, é possivel realizar a extensão desta funcionalidade dado que é apenas necessário realizar a implementação de uma nova classe que implementa o strategy e ter como argumento a classe com contato direito com o novo input criado na urna. Diante disso, é possivel observar que o contrato entre os tipos e subtipos é respeitando, sendo que o tipo pode ser substituido sobre seu subtipo. os módulos tem acesso apenas a quilo que utiliza dentro da herança do strategy.

# F4: Registrar o voto do eleitor



Por fim, irei fazer um detalhamento de cada um dos principios pensando no projeto como um todo, e como cada funcionalidade é opcional para utilizar a urna eletrônica, possibilitando sua toda flexibilidade na utilização de suas funcionalidades.

Por fim, é possivel observar que o projeto idealizado da urna eletronica respeita os princípios SOLI. (Fazer isso la em cima mesmo)


## Como foram distribuidas as responsabilidades e por que?
## Como o projeto pode ser estendido sem causar impacto no restante da arquitetura?
## O "contrato" de cada tipo é respetado, independente da instância real daquele tipo?
## Como está a dependência entre as classes/módulos/funções? Um móduolo exerga apenas aquilo que precisa do outro?
