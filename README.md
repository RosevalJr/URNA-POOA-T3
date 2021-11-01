# Trabalho 03 - Princípios de Substituição de Liskov e Segregação de Interface
- Universidade Federal de São Carlos - Departamento de Computação
- Programação Orientada a Objetos Avançada
- Aluno: Roseval Donisete Malaquias Junior - RA: 758597
- Professor: Delano Medeiros Beder

---
> Este documento efetua o detalhamento solicitado pelo professor sobre o diagrama UML de classes produzido. O arquivo PDF do diagrama UML de classes da urna encontra-se neste repositório com o nome de [UML-URNA.pdf](https://github.com/RosevalJr/URNA-POOA-T3/blob/main/UML-URNA.pdf).

# Introdução

Neste repositório, é apresentado o projeto de sistema de software para uma urna eletrônica. No documento de requisitos disponibilizado pelo professor é feita a especificação detalhada do funcionamento de cada funcionalidade, além de especificar quais partes do hardware da urna serão disponibilizadas para cada funcionalidade. Esse sistema de software é capaz de realizar 4 funções bàsicas. 
- F1: Receber comandos do mesário para iniciar uma nova votação;
- F2: Informar o eleitor sobre a votação;
- F3: Receber o voto do eleitor;
- F4: Registrar o voto do eleitor.

Importante destacar que, todos os mecanismos disponibilizados são opcionais na urna. O cliente deve escolher o nível de segurança, acessibilidade e auditoria que ele desejar. Além disso, com o intuito de evitar o design especulativo, foram especificadas extensões esperadas para as funcionalidades F2 e F3 da urna.

Para especificar a descrição arquitetural da solução de software projetada pelo autor, foi produzido um diagrama de classes com a Unified Modeling Language (UML). Esse modelo UML especifica todas as classes idealizadas para o funcionamento do sistema, juntamente com as cardinalidades e respectivas relações entre as classes idealizadas. A seguir será feito o detalhamento de como essas funcionalidades foram idealizadas pelo autor e expressadas no modelo UML de classes produzido. Também, será destacado como a arquitetura idealizada respeita os princípios SOLI do acrônimo SOLID, detalhando os pensamentos e técnicas aplicadas para evitar a quebra de certos princípios.

Inicialmente, é importante destacar a estrutura geral da solução arquitetural criada. A classe ``Urna`` foi criada para funcionar como o cliente de todas as funcionalidades projetadas nesta solucação. Essa classe é responsável por instanciar todas as classes desejadas das funcionalidades e executar seus métodos **"Strategy"**. Sendo assim, todos os diferentes mecanimos são opcionais na urna projetada, tangendo a classe cliente (``Urna``) escolher quais funcionalidades serão executadas. Como pode ser observado na Figura 1, a classe ``Urna`` possui argumentos do tipo **"List<>"** para cada uma das classes interface **"Strategy"**, sendo que nos métodos **"Procedimento"** apenas é chamado o método **"Strategy"** de cada uma das interfaces instanciadas como desejado pelo cliente. Diante disso, no método ``votacao()`` é chamado cada um dos métodos **"Procedimento"** ligados a execução da votação, e por fim o voto é armazenado chamando o seu **"Procedimento"** respectivo de estratégias de armazenamento. Importante destacar que as informações dos candidatos utilizados na votação são inseridas na urna manualmente, utilizando a classe **"Strategy"** ligado ao armazenamento interno de candidatos. Também, para recuperar essas informações armazenadas internamente na urna, é utilizado a classe **"Strategy"** ligado ao armazenamento interno de candidatos. A seguir será feito um detalhamento minuncioso da solução proposta no diagrama UML para cada uma das funcionalidades.

![Figura 1](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig1.png)
<div align="center">
  <b>Figura 1: Classe "Urna", utilizada como cliente de todas as funcionalidades.</b>
</div>

# F1: Receber comandos do mesário para iniciar uma nova votação

A primeira funcionalidade faz com que seja necessário uma forma de comunicação entre o mesario e urna. Diante disso, foi idealizada a classe ``MesarioInput`` que é responsavel por receber os comandos do equipamento do mesário através de seu método ``ActionRecebeInputMesario()``, como pode ser observado na Figure 2. Essa classe é utilizada pela classe ``urna`` para ler a entrada do mesário e saber quando a votação deve ser inicializada, executando o método ``AguardaInicioVotoMesario()`` da classe ``Urna``.

![Figura 2](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig2.png)
<div align="center">
  <b>Figura 2: Classe "MesarioInput", responsável por receber o input do mesário.</b>
</div>

# F2: Informar o eleitor sobre a votação

Para projetar a segunda funcionalidade foi utilizado o padrão de projeto **"Strategy"**, sendo criada a classe interface ``OutputMostraVotacao`` que contém a assinatura do método **"Strategy"** que mostra as informações da votação para o eleitor dado um meio de output especifico. Como a urna pode ter diversos meios de output e todos eles devem ser possiveis de utilizar em uma votação, além de possibilitar a extensão para a utilização de novos meios de output para mostrar a votação, foram criadas as classes ``BrailleMostraVotacao``, ``TelaMostraVotacao`` e ``AudioMostraVotacao``. Essas classes são responsaveis por implementar o método **"Strategy"** para mostrar as informações da votação ao eleitor, utilizando um mecanismo de hardware disponibilizado pela urna. Diante disso, essas classes possuem um argumento da classe que faz a interação com o mecanismo de hardware de urna como as classes ``OutputTela``, ``OutputBraile`` e ``OutputAudioFone``. Dentro da impleementação do método **"Strategy"** ``MostraVotacao()`` é implementado como é feita a mostragem das informações da votação, utilizando a classe que faz interação com o hardware da urna como meio de comunicação com o hardware, como pode ser visualizado na Figura 3.

Além disso, com o intuito de encapsular a entrada do método **"Strategy"** ``MostraVotacao()``, foi criado a classe ``TextoImagem``. Como nesta funcionalidade é informado ao eleitor sobre a votação através de texto e/ou imagem, foi criada esta classe que encapsula esse tipo de dados.

![Figura 3](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig3.png)
<div align="center">
  <b>Figura 3: Utilização do padrão de projeto Strategy para arquiteturar a segunda funcionalidade.</b>
</div>


Portanto, as classes envolvidas nesta funcionalidade respeitam o principio SRP, sendo que cada classe possue apenas uma resposanbilidade para com a funcionalidade. Também respeitam o princípio de OCP, sendo que para realizar a extensão de utilização de um novo meio de output para mostrar a votação, simplesmente é precisso criar uma nova classe que herda da classe interface ``OutputMostraVotacao`` e possue como argumento uma classe que interage com o novo mecanimos de hardware projetado. Por fim, essa nova classe irá implementar o método **Strategy** herdado. Para essa nova funcionalidade seja utilizada pela urna, a classe ``Urna`` deve instanciar uma nova instancia da nova classe criada. Dessa forma, quando o método ``ProcedimentoMostraVotacao`` da classe ``Urna`` for chamado, o método **Strategy** implementado da nova classe será chamado e a nova adição a funcionalidade será executada. Além disso, também respeita o princípio de LSP sendo que os contratos entre os tipos das classes são respeitados, qualquer uma das classes que implementam o método **Strategy** substituem a classe interface ``OutputMostraVotacao``. Por fim, também respeita o princípio de ISP, sendo que cada um dos módulos enxergam apenas os métodos que são utilizados, no caso apenas o método **Strategy** herdado.

# F3: Receber o voto do eleitor

A terceira funcionalidade, assim como a primeira, foi projetada utilizando o padrão de projeto **Strategy**, como pode ser visualizado na Figura 4. Sendo assim, foi idealizada a classe interface ``InputPegaOpcaoVoto`` que contém a assinatura do método **Strategy** ``pegaOpcaoVoto()``. Existe as classes que implementam o método **Strategy** herdando desta classe interface, sendo elas ``TecladoFisicoPegaOpcaoVoto`` e ``TouchScreenPegaOpcaoVoto``. Cada uma dessas classes, possui como argumento uma classe que interage com o harware de input da urna, e implementa o método **Strategy** herdado a fim de captar a opção do voto do eleitor passado por um input específico.

![Figura 4](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig4.png)
<div align="center">
  <b>Figura 4: Utilização do padrão de projeto Strategy para arquiteturar a terceira funcionalidade.</b>
</div>

Além disso, para enviar o aviso que o voto foi computado com sucesso, também foi utilizado o padrão de projeto **Strategy**, como pode ser visualizado na Figura 5. A classe interface ``OutputAvisaFimVoto`` fornece a assinatura do método **Strategy** ``avisaFimVoto()``. As classes ``TelaAvisaFimVoto``, ``AudioAvisaFmVoto`` e ``MesarioAvisaFimVoto`` implementam esse método **Strategy** e cada uma delas possui como argumento uma classe que interage com um mecanismo de hardware da urna a fim de utilizar esse meio dentro da implementação do método **Strategy** para realizar o output requerido.

![Figura 5](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig5.png)
<div align="center">
  <b>Figura 5: Utilização do padrão de projeto Strategy para arquiteturar a funcionalidade de aviso de fim de voto.</b>
</div>

Por fim, essas duas caracteristicas da terceira funcionalidade podem ou não serem utilizadas pela classe ``Urna``, sendo que para serem utilizadas apenas é necessario que a classe cliente ``Urna``, instancie as classes referentes a funcionalidades desejadas

Portanto, as classes referentes à essa funcionalidade respeitam o princípio de SRP, sendo que cada classe possui apenas um papel para a execução da terceira funcionalidade. Também respeitam o OCP, dado que para realizar a extensão desta funcionalidade assemelhasse a extensão da primeira funcionalidade dado a utilização do padrão de projeto **Strategy**, sendo necessária uma nova classe que implemente o método **Strategy** e tenha como argumento uma classe ligada a funcionalidade de um mecanismo de hardware da urna. Respeita também o LSP dado as classes que herdam as classes interface podem ser substiteum o tipo da classe interface em sua utilização na classe cliente ``Urna``. Por fim, também respeita o ISP, dado que todas as classes que herdam das classes interface necessitam e utilizam apenas o método **Strategy** herdado.

# F4: Registrar o voto do eleitor
Por fim, para explicar a F4 é necessario explicar como o armazenamento de votos é feito. Para essa funcionalidade também foi utilizada o padrão de projeto **Strategy**, sendo idealizado a classe ``ArmazenamentoVoto`` que contém a assinatura do método **Strategy** ``ArmazenaVoto()``, como pode ser observado na Figura 6. Esse método então é implementado por classes que representam as diversas estratégias de armazenamento de votos, que interagem com os mecanimos de armazenamento da urna. Sendo elas, as classes ``ArmazenamentoVotoInternet``, ``ArmazenamentoVotoInterno`` e ``ArmazenamentoVotoExterno``.

![Figura 6](https://raw.githubusercontent.com/RosevalJr/URNA-POOA-T3/main/imgsDoReadme/Fig6.png)
<div align="center">
  <b>Figura 6: Utilização do padrão de projeto Strategy para arquiteturar a quarta funcionalidade.</b>
</div>

Diante disso, é possivel destacar que as classes que envolvem essa funcionalidade respeitam o SRP, sendo que cada classe possue apenas uma resposabilidade dentro da execução da quarta funcionalidade. Também respeita o princípio OCP, sendo que para realizar a extensão desta funcionalidade não será necessario alterar código ja implementado, sendo necessario apenas implementar o método **Strategy** da classe interface ``ArmazenamentoVoto``.

# Conclusão
Diante do detalhamento da solução arquitetural da urna produzida neste trabalho, é possivel observar que os princípios SOLI do acrônimo SOLID são respeitado. O SRP é respeitado, sendo a ``Urna`` a classe principal responsavel por ser o cliente de todas as funcionalidades disponiveis à ela, estando encarregada de escolher quais funcionalidades serão executadas também. Além disso, todas as outras classes possuem apenas uma responsabilidade cada para a execução de determinada funcionalidade. O principio OCP também é respeitado, sendo que foi coberto todas as extensões previstas no documento de especificações disponibilizado pelo professor. Além disso, caso uma das extensões seja feita, não será necessário alterar código ja implementando, sendo que foi utilizado o padrão de projeto **Strategy** durante toda a modelagem da solução arquitetural produzida. O principio LSP também é respeitado, sendo que toda classe que herda da classe interface que contém a assinatura do método **Strategy** pode substituir o tipo desta classe interface, afim de possibiliar a abstração para a execução de todas as classes que implementa, esse método **Strategy**. Por fim, também é respeitado o principio ISP, sendo que todo módulo presente na solução produzida enxerga apenas aquilo que irá utilizar, por isso foi criada a classe interface ``ArmazenamentoCandidato``, sendo que o armazenamento de cadnidadto é diferente de armazenamento de voto, caso todas as classes herdassem de uma unica interface haveria a existenca de métodos não utilizados pelas classes que herdam, quebrando entao o princípio ISP.
