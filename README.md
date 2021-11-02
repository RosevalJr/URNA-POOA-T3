# Trabalho 03 - Princípios de Substituição de Liskov e Segregação de Interface
- Universidade Federal de São Carlos - Departamento de Computação
- Programação Orientada a Objetos Avançada
- Aluno: Roseval Donisete Malaquias Junior - RA: 758597
- Professor: Delano Medeiros Beder

---
> Este documento realiza o detalhamento solicitado pelo professor sobre o diagrama UML de classes produzido. O arquivo PDF do diagrama UML de classes da urna encontra-se neste repositório com o nome de [UML-URNA.pdf](https://github.com/RosevalJr/URNA-POOA-T3/blob/main/UML-URNA.pdf).

# Introdução

Neste repositório, é apresentado o projeto de sistema de software para uma urna eletrônica. No documento de requisitos disponibilizado pelo professor é feita a especificação detalhada do funcionamento de cada funcionalidade, além de especificar quais partes do hardware da urna serão disponibilizadas para cada funcionalidade. Esse sistema de software é capaz de realizar 4 funções básicas. 
- F1: Receber comandos do mesário para iniciar uma nova votação;
- F2: Informar o eleitor sobre a votação;
- F3: Receber o voto do eleitor;
- F4: Registrar o voto do eleitor.

Importante destacar que, todos os mecanismos disponibilizados são opcionais na urna. O cliente deve escolher o nível de segurança, acessibilidade e auditoria que ele desejar. Além disso, com o intuito de evitar o design especulativo, foram especificadas extensões esperadas para as funcionalidades F2 e F3 da urna.

Para especificar a descrição arquitetural da solução de software projetada pelo autor, foi produzido um diagrama de classes com a Unified Modeling Language (UML). Esse modelo UML especifica todas as classes idealizadas para o funcionamento do sistema, juntamente com as cardinalidades e respectivas relações entre as classes idealizadas. A seguir será feito o detalhamento de como essas funcionalidades foram idealizadas pelo autor e expressadas no modelo UML de classes produzido. Também, será destacado como a arquitetura idealizada respeita os princípios SOLI do acrônimo SOLID, detalhando os pensamentos e técnicas aplicadas para evitar a quebra de certos princípios.

Inicialmente, é importante destacar a estrutura geral da solução arquitetural criada. A classe ``Urna`` foi criada para funcionar como o cliente de todas as funcionalidades projetadas nesta solução. Essa classe é responsável por instanciar todas as classes desejadas das funcionalidades e executar seus métodos **"Strategy"**. Sendo assim, todos os diferentes mecanismos são opcionais na urna projetada, tangendo a classe cliente (``Urna``) escolher quais funcionalidades serão executadas. Como pode ser observado na Figura 1, a classe ``Urna`` possui argumentos do tipo **"List<>"** para cada uma das classes interface **"Strategy"**, sendo que nos métodos **"Procedimento"** apenas é chamado o método **"Strategy"** de cada uma das interfaces instanciadas como desejado pelo cliente. Diante disso, no método ``Votacao()`` é chamado cada um dos métodos **"Procedimento"** ligados a execução da votação, e por fim o voto é armazenado chamando o seu **"Procedimento"** respectivo de estratégias de armazenamento. Importante destacar que as informações dos candidatos utilizados na votação são inseridas na urna manualmente, utilizando a classe **"Strategy"** ligado ao armazenamento interno de candidatos. Também, para recuperar essas informações armazenadas internamente na urna, é utilizado a classe **"Strategy"** ligado ao armazenamento interno de candidatos. A seguir será feito um detalhamento minucioso da solução proposta no diagrama UML para cada uma das funcionalidades.

![Figura 1](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig1.png)
<div align="center">
  <b>Figura 1: Classe "Urna", utilizada como cliente de todas as funcionalidades.</b>
</div>

# F1: Receber comandos do mesário para iniciar uma nova votação

A primeira funcionalidade faz com que seja necessário uma forma de comunicação entre o mesário e a urna. Diante disso, foi idealizada a classe ``MesarioInput`` que é responsável por receber os comandos do equipamento do mesário através de seu método ``ActionRecebeInputMesario()``, como pode ser observado na Figura 2. Essa classe é utilizada pela classe ``Urna`` para ler a entrada do mesário e saber quando a votação deve ser iniciada. A classe ``Urna`` utiliza esta classe através da chamada de seu método ``AguardaInicioVotoMesario()``.

![Figura 2](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig2.png)
<div align="center">
  <b>Figura 2: Classe "MesarioInput", responsável por receber o input do mesário.</b>
</div>

# F2: Informar o eleitor sobre a votação

Para projetar a segunda funcionalidade foi utilizado o padrão de projeto **"Strategy"**, sendo criada a classe interface ``OutputMostraVotacao`` que contém a assinatura do método **"Strategy"** que mostra as informações da votação para o eleitor dado um meio de output específico. Como a urna pode ter diversos meios de output e todos eles devem ser possíveis de utilizar em uma votação, além de possibilitar a extensão para a utilização de novos meios de output para mostrar a votação, foram criadas as classes ``BrailleMostraVotacao``, ``TelaMostraVotacao`` e ``AudioMostraVotacao``. Essas classes são responsáveis por implementar o método **"Strategy"** para mostrar as informações da votação ao eleitor, utilizando um mecanismo de hardware disponibilizado pela urna. Diante disso, essas classes possuem uma classe como argumento que faz a interação com o mecanismo de hardware de output da urna como as classes ``OutputTela``, ``OutputBraile`` e ``OutputAudioFone``. Dentro da implementação do método **"Strategy"** ``MostraVotacao()`` é exibida as informações da votação ao eleitor, utilizando a classe que faz interação com o hardware da urna como meio de comunicação com o hardware, como pode ser visualizado na Figura 3.

Além disso, com o intuito de encapsular a entrada do método **"Strategy"** ``MostraVotacao()``, foi criado a classe ``TextoImagem``. Como nesta funcionalidade é informado ao eleitor sobre a votação através de texto e/ou imagem, foi criada esta classe que encapsula esse tipo de dados.

![Figura 3](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig3.png)
<div align="center">
  <b>Figura 3: Utilização do padrão de projeto Strategy para arquiteturar a segunda funcionalidade.</b>
</div>


Portanto, as classes envolvidas nesta funcionalidade respeitam o princípio SRP, sendo que todas as classes envolvidas possuem um papel dentro do padrão de projeto **"Strategy"** para a execução da segunda funcionalidade. Também respeitam o princípio de OCP, sendo que para realizar a extensão de uma nova forma de mostrar a votação ao eleitor, simplesmente, é preciso criar uma nova classe que herda da classe interface ``OutputMostraVotacao`` e possui como argumento uma classe que interage com um mecanismo de hardware projetado. Por fim, essa nova classe irá implementar o método **"Strategy"** herdado, implementando a nova forma de demonstração da votação. Para que essa nova funcionalidade seja utilizada pela urna, a classe ``Urna`` deve instanciar uma nova instância da nova classe criada, inserindo-a na **"List<>"** ``outputMostraVoto``. Dessa forma, quando o método ``ProcedimentoMostraVotacao`` da classe ``Urna`` for chamado, o método **"Strategy"** implementado da nova classe será chamado e a nova adição a funcionalidade será executada. Além disso, também respeita o princípio de LSP sendo que os contratos entre os tipos das classes são respeitados, qualquer uma das classes que implementam o método **"Strategy"** substituem a classe interface ``OutputMostraVotacao``. Por fim, também respeita o princípio de ISP, sendo que cada um dos módulos enxergam apenas os métodos que são utilizados, no caso apenas o método **"Strategy"** herdado da classe ``OutputMostraVotacao``.

# F3: Receber o voto do eleitor

A terceira funcionalidade, assim como a primeira, foi projetada utilizando o padrão de projeto **"Strategy"**, como pode ser visualizado na Figura 4. Sendo assim, foi idealizada a classe interface ``InputPegaOpcaoVoto`` que contém a assinatura do método **"Strategy"** ``pegaOpcaoVoto()``. Diante disso, existem as classes que são responsáveis por implementar o método **"Strategy"** herdado desta classe interface, especificando como será feito o recebimento do voto do eleitor, sendo elas as classes ``TecladoFisicoPegaOpcaoVoto`` e ``TouchScreenPegaOpcaoVoto``. Cada uma dessas classes, possui como argumento uma classe responsável por interagir com o hardware de input da urna, sendo elas as classes ``InputTecladoFisico`` e ``InputTouchScreen``. Essas classes que interagem como hardware de input da urna obtêm a opção de votação do eleitor, após sua confirmação.

![Figura 4](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig4.png)
<div align="center">
  <b>Figura 4: Utilização do padrão de projeto Strategy para arquiteturar a terceira funcionalidade.</b>
</div>

Ademais, para enviar o aviso que o voto foi computado com sucesso, também foi utilizado o padrão de projeto **"Strategy"**, como pode ser visualizado na Figura 5. A classe interface ``OutputAvisaFimVoto`` fornece a assinatura do método **"Strategy"** ``AvisaFimVoto()``. As classes ``TelaAvisaFimVoto``, ``AudioAvisaFmVoto`` e ``MesarioAvisaFimVoto`` são responsáveis por implementar esse método **"Strategy"**, especificando como será feito o aviso de fim de voto para o eleitor. Cada uma dessas classes possui como argumento uma classe que interage com um mecanismo de hardware de output da urna. Essas classes que interagem com os mecanismos de hardware de output da urna também são utilizadas na segunda funcionalidade. Diante disso, essas classes são ``OutputTela`` e ``OutputAudioFone``. Entretanto, também há uma classe de hardware de output da urna somente utilizada nesta funcionalidade que é ``MesarioOutput``, responsável por enviar uma mensagem de texto ao equipamento do mesário.

![Figura 5](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig5.png)
<div align="center">
  <b>Figura 5: Utilização do padrão de projeto Strategy para arquiteturar a funcionalidade de aviso de fim de voto.</b>
</div>

Portanto, as classes referentes à essa funcionalidade respeitam o princípio de SRP, sendo que cada classe possui apenas um papel dentro do padrão de projeto **"Strategy"**. Também respeitam o princípio OCP, dado que para realizar a extensão desta funcionalidade assemelhasse a extensão da segunda funcionalidade, por causa da utilização do padrão de projeto **"Strategy"**, sendo necessária uma nova classe que implemente o método **"Strategy"** e tenha como argumento uma classe ligada à funcionalidade de um mecanismo de hardware de urna (output ou input). Além disso, respeita o princípio LSP dado que as classes que herdam das classes interface podem substituir o tipo da classe interface em sua utilização na classe cliente ``Urna``. Por fim, também respeita o ISP, dado que todas as classes que herdam das classes interface necessitam e utilizam apenas o método **"Strategy"** herdado.

# F4: Registrar o voto do eleitor

Por fim, para detalhar a quarta funcionalidade é necessário explicar como o armazenamento de votos é feito. Para essa funcionalidade também foi utilizada o padrão de projeto **"Strategy"**, sendo idealizado a classe ``ArmazenamentoVoto`` que contém a assinatura do método **Strategy** ``ArmazenaVoto()``, como pode ser observado na Figura 6. Esse método **"Strategy"** então é implementado por classes que representam as diversas estratégias de armazenamento de votos, que interagem com os mecanimos de armazenamento da urna. Essas classes responsáveis por implementar o método **"Strategy"** são``ArmazenamentoVotoInternet``, ``ArmazenamentoVotoInternoCripto`` e ``ArmazenamentoVotoExterno``. Cada uma dessas classes, responsáveis por implementar a estratégia de armazenamento do voto, possuem como argumento uma classe responsável por interagir com os mecanimos de hardware de armazenamento da urna, sendo essas classes ``ArmazenamentoInternet``, ``ArmazenamentoExterno`` e ``ArmazenamentoIterno``.

No documento de requisitos, também foi especificado que as informações ligadas a votação são inseridas manualmente na urna um dia antes da votação. Sendo assim, foi criado a classe ``Candidato``, responsável por encapsular as informações de um candidato para a votação. Também, foi criado a classe interface ``ArmazenamentoCandidato``, que contém a assinatura de dois métodos **"Strategy"** para a recuperação dos candidatos armazenados na urna (``RecuperaCandidatos()``) e armazenamento dos candidatos na urna (``ArmazenaCandidatos()``). Diante disso, há a classe que herda da classe interface e implementa esses dois métodos **"Strategy"**, sendo ela a classe ``ArmazenamentoCandidatoInterno``, responsável por ditar como os candidatos são armazenados e recuperados. Essa classe possui como argumento uma classe que interage com o mecanismo de armazenamento interno da urna, sendo ela a clase ``ArmazenamentoInterno``. Diate disso, a classe ``ArmazenamentoCandidatoInterno`` utiliza os métodos da classe que interage com o mecanimos de armazenamento interno para implementar os métodos **"Strategy"** herdados.

Além disso, também foi idealizada a classe ``Voto``, responsável por encapsular as informações de um voto do eleitor. Como o voto deve ser anônimo, essa classe apenas encapsula a opção do voto e também a data e hora do voto.

![Figura 6](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig6.png)
<div align="center">
  <b>Figura 6: Utilização do padrão de projeto Strategy para arquiteturar a quarta funcionalidade.</b>
</div>

Diante disso, é possível destacar que as classes que envolvem essa funcionalidade respeitam o SRP, sendo que cada classe possui apenas uma resposábilidade dentro da aplicação do padrão de projeto **"Strategy"**. Também respeita o princípio OCP, sendo que para realizar a extensão desta funcionalidade não será necessário alterar código já implementado, sendo necessário apenas implementar uma classe que herda da classe interface **"Strategy"**, implementando o método **"Strategy"** e que tenha como argumento uma classe ligada à funcionalidade de um mecanismo de hardware de armazenamento da urna. Além disso, respeita o princípio LSP dado que as classes subtipos presentes nesta funcionalidade podem substituir as classes tipos (classes interface). Por fim, também respeita o ISP, dado que todas as classes que herdam das classes interface têm acesso apenas aos métodos que irão utilizar.

# Conclusão
Diante do detalhamento da solução arquitetural da urna produzida neste trabalho, é possível observar que os princípios SOLI do acrônimo SOLID são respeitados. O SRP é respeitado, sendo a ``Urna`` a classe principal responsável por ser o cliente de todas as funcionalidades disponíveis à ela, estando encarregada de escolher quais funcionalidades serão executadas também. Além disso, todas as outras classes possuem apenas uma responsabilidade cada, para a execução de determinada funcionalidade. O princípio OCP também é respeitado, sendo que foi coberto todas as extensões previstas no documento de especificações disponibilizado pelo professor. Além disso, caso uma das extensões seja feita, não será necessário alterar código já implementado, sendo que foi utilizado o padrão de projeto **"Strategy"** durante toda a modelagem da solução arquitetural produzida. O princípio LSP também é respeitado, sendo que toda classe que herda da classe interface que contém a assinatura do método **Strategy** pode substituir o tipo desta classe interface, a fim de possibilitar a abstração para a execução de todas as classes que implementam esse método **"Strategy"**. Por fim, também é respeitado o princípio ISP, sendo que todo módulo presente na solução produzida enxerga apenas aquilo que irá utilizar. Diante disso, foi criada a classe interface ``ArmazenamentoCandidato``, sendo que o armazenamento de cadnidato é diferente de armazenamento de voto, caso todas as classes herdassem de uma única interface haveria a existência de métodos não utilizados pelas classes que herdam, quebrando então o princípio ISP.
