# Trabalho 03 - Princípios de Substituição de Liskov e Segregação de Interface
- Universidade Federal de São Carlos - Departamento de Computação
- Programação Orientada a Objetos Avançada
- Aluno: Roseval Donisete Malaquias Junior - RA: 758597
- Professor: Delano Medeiros Beder

---

> Para o maior detalhamento desta ferramenta, com explicações mais detalhadas de suas funcionalidades, funcionamento e especificações para sua extensão, o seguinte vídeo tutorial foi gravado e postado no youtube: <https://youtu.be/mXFNUSFyVDY>

Neste repositório, é apresentada uma ferramenta que exemplifica a aplicação do princípio Aberto-Fechado (OCP) no contexto da programação orientada a objetos, respeitando também o princípio da responsabilidade única (SRP). Sendo assim, é possível estender a ferramenta sem que sejam feitas mudanças em código já funcional, e todos módulos de código da ferramenta possuem apenas uma vertente de mudança, apresentando alta flexibilidade, facilidade de manutenção e reutilização.

A ferramenta ``GetNews`` foi programada em Java, e tem como principal objetivo possibilitar que informações de websites sejam baixadas e utilizadas dado algoritmos previamente implementados. Entretanto, o foco das demonstrações da ferramenta serão na extração e utilização de informações de sites de notícias, como títulos e links.

A fim de evitar o design especulativo, 2 extensões são previstas pelo projeto.

- Extensão 1: Deve ser possível incluir novos sites de notícias (ex: UOL, Estadão, etc)
- Extensão 2: Deve ser possível incluir novos algoritmos, além de salvar no formato csv:
-- Exemplos: imprimir na tela, baixar os conteúdos das notícias, seguindo os links, criar
uma nuvem de palavras com todos os títulos de notícias do dia, aplicar um algoritmo de
aprendizado de máquina para detectar tendências e diferenças entre os sites

Diante disso, o projeto da ferramenta apresenta 2 classes abstratas que servem de modelo para possibilitar a extensão do projeto caso necessário. A classe ``ModelHtmlParser`` cobre a extensão 1, e a classe ``ModelHtmlAttributeUse`` cobre a extensão 2. A fim de exemplificar a utilização dessas 2 classes e a possibilidade de extensão por elas providas, dentro deste projeto há 3 classes que herdam da classe ``ModelHtmlAttributeUse`` (``CsvPrint``, ``ScreenPrint`` e ``WordCloudPrint``) e 3 classes que herdam da classe ``ModelHtmlParser`` (``GloboParser``, ``OulParser`` e ``BbcLondonParser``), implementando seus métodos abstratos. Por fim, dentro da classe ``Main`` do projeto é demonstrado o funcionamento destas 6 classes, exemplificando suas funcionalidades, flexibilidade e possibilidade de extensão.

**"Como eu faço para incluir um novo site de notícias? Onde eu tenho que mexer?"**

Inicialmente, é importante destacar o que é a classe ``HtmlAttribute``. Essa classe encapsula os atributos que definem uma busca de um "atributo" em uma página web dado uma url, tag, className e o attributeName. Também, possibilita o armazenamento dos atributos familyTag e familyClassName para realizar uma busca hierarquica. Além disso, mantém um ArrayList<String> chamado ``atributeValues`` para armazenar os elementos encontrados nesta busca. O construtor desta classe e a definição de seus atributos pode ser observado a seguir:

```Java
// ...
public class HtmlAttribute {
    
    // Argumentos para selecionar um htmlAttributeValue.
    private final String url;
    private final String tag; 
    private final String className;
    // Esses argumentos serao setados, caso seja desejado fazer uma busca por 
    // hierarquia, caso contrario eles podem ser nulos.
    private final String familyTag;
    private final String familyClassName;
    // -------------------------------------------------------------------------
    private final String attributeName; 
    
    // Estrutura para armazenar os atributosValues (Strings) encontrados.
    private final ArrayList<String> attributeValues; 
    
    // Inicializa todas as variaveis finais.
    public HtmlAttribute(String url, String tag, String className, String
            familyTag, String familyClassName, String attribute){
        this.url = url;
        this.tag = tag;
        this.className = className;
        this.familyTag = familyTag;
        this.familyClassName = familyClassName;
        this.attributeName = attribute;
        this.attributeValues = new ArrayList<String>();
        
    }
//...
}
```

Para incluir um novo site de notícias, você deve implementar uma nova classe que herda da classe abstrata ``ModelHtmlParser``, implementando os métodos abstratos ``setHtmlAttributes`` e ``selectAttributesValues``.

Na implementação do método ``setHtmlAttributes``, você deve setar os ``HtmlAttributes`` que definem de onde será tirado as informações das notícias e quais informações serão extraidas. Para cada ``HtmlAttribute`` setado será feita uma busca na página html do site especificado no ``HtmlAttribute``, sendo que dentro de cada ``HtmlAttribute`` há um ArrayList\<String\> (``attributeValues``) para armazenar as informações extraídas. Um exemplo de implementação deste método na classe ``GloboParser``, para extrair títulos e links das notícias, pode ser observado a seguir:

```Java
// Tanto o titulo quanto o link se encontram dentro da mesma tag a.post__link
// so muda o argumento.
@Override
protected void setHtmlAttributes() {
    // Nao e necessario realizar uma busca hierquica para encontrar os links.
    // Entao setei o familyTag e familyClassName como nulos.
    htmlAttributes.add(new HtmlAttribute("https://www.globo.com/", "a", 
            "post__link", null, null,"title"));
    htmlAttributes.add(new HtmlAttribute("https://www.globo.com/", "a", 
            "post__link", null, null,"href"));
}
```

Na implementação do método ``selectAttributesValues``, você deve especificar como serão extraidos as informações dos sites de notícias (dado os ``HtmlAttributes`` setados pelo método ``setHtmlAttributes``) e popular o ArraList\<String\> (``attributeValues``) de cada ``HtmlAttribute``. Um exemplo de implementação deste método na classe ``GloboParser``, que utiliza o ``Jsoup`` para baixar as páginas html e realizar a seleção, pode ser observado a seguir:

```Java
// Abaixa a pagina especificada (Jsoup), retornando-a como um Document, onde sera feita
// uma selecao dado uma tag, className e attributeName de cada htmlAttribute setado.
// Por fim, o resultado da selecao (uma lista de Strings) e armazenando na estrutura
// ArrayList<String> htmlAttributeValues do seu respectivo htmlAttribute.
@Override
protected void selectAttributesValues(){
    // Busca para cada um dos htmlAttributes.
    for(int i = 0 ; i < htmlAttributes.size(); i++){

        // Possivel que o HandShake nao de certo (site da globo costuma recusar mais)!
        Document doc = null;
        try {
            doc = Jsoup.connect(htmlAttributes.get(i).getUrl()).get();
        } catch (IOException ex) {
            Logger.getLogger(GloboParser.class.getName()).log(Level.SEVERE, null, ex);
        }

        // Seleciona todos elementos dado uma tag e um className especificado.
        Elements elements = doc.select(htmlAttributes.get(i).getTag() + "." +
                htmlAttributes.get(i).getClassName());

        // Nao e necessario realizar uma busca hierquica para encontrar os links!
        // Caso o nome do atributo seja "text" e necessario acessar o conteudo.
        if(htmlAttributes.get(i).getAttributeName().equals("text"))
            for(Element element: elements)
                htmlAttributes.get(i).addAttributeValue(element.text());
        else
            for(Element element: elements)
                htmlAttributes.get(i).addAttributeValue(element.attr(htmlAttributes.get(i).getAttributeName()));
    }
}
```

Importante destacar que, dada essa estratégia utilizada para prever a extensão 1 especificado pelo professor, a ferramenta não está limitada a utilizar uma determinada ferramenta para realizar o download das páginas html. Com a implementação deste dois métodos abstratos é preciso apenas setar o que será extraído das páginas html, seguindo os atributos da classe ``HtmlAttribute``, e realizar as buscas de cada um dos ``HtmlAttribute`` populando o ArrayList\<String\> (``attributeValues``) de cada um dos ``HtmlAttributes``. Além disso, dada essa implementação a ferramenta não está limitada a extrair informações de sites de notícias, ela pode extrair informações de qualquer site desejado, como extrair comentarios de websites de compras ou extrair titulos de videos do youtube.

**"Como eu faço para incluir um algoritmo para processar as notícias extraídas? Onde eu tenho que mexer?"**

Para incluir um algoritmo para processar notícias extraídas, é preciso implementar uma classe que herde da classe abstrata ``ModelHtmlAttributeUse``, implementando seu método abstrato ``Use``.

Na implementação deste método ``Use`` é necessário processar os ``HtmlAttribute`` que serão passados como entradas. Dentro destes ``HtmlAttribute``, é possível acessar os metadados da busca que foi realizada e os ``attributeValues``, que são as Strings que foram extraídas na busca realizada. Diante disso, é aqui que será definido como as notícias extraídas serão utilizadas. A implementação de um método ``Use``, que apenas printa na tela a URL e os ``attributeValues`` para cada um dos ``HtmlAttribute`` passados como entrada, pode ser observado a seguir:

```Java
// Percorre todos os htmlAttributes e printa os htmlAttributeValues (Strings)
// para cada um deles na tela, especificando antes em qual URL foi retirado.
@Override
public void use(ArrayList<HtmlAttribute> htmlAttributes) {
    for(int i = 0 ; i < htmlAttributes.size(); i++){
        System.out.println("Retirado da URL: " + htmlAttributes.get(i).getUrl());
        for(int j = 0 ; j < htmlAttributes.get(i).getAttributeValuesSize(); j++){
            System.out.println(htmlAttributes.get(i).getAttributeValue(j));
        }
    }
}
```

Importante destacar que, a escolha de ter como entrada deste método os ``HtmlAttribute`` foi feita para que o método ``Use`` que processa esses dados tenha acesso aos metadados da busca realizada, sendo que em alguns casos isso pode ser interessante, como na implementação visualizada acima e, possivelmente, em uma implementação do método que aplique algum algoritmo de aprendizado de máquina, onde possa ser pertinente saber de onde foram retiradas as informações do site, quais tags foram buscadas e etc.

**"Como eu utilizo as extensões implementadas?"**

Um cliente que deseja utilizar as classes das extensões especificadas deve apenas instanciar uma classe que herde de ``ModelAttributeUse`` e outra que herde de ``ModelHtmlParse``. Por fim, o método ``useHtmlAttributesValues`` da classe que herda de ``ModelHtmlParse`` deve ser chamado, passando como parâmetro a instância da classe que herda de ``ModelAttributeUse``. Isso pode ser melhor visualizado dentro da classe ``Main`` da ferramenta que possui alguns exemplos de utilização das classe implementadas, como pode ser observado a seguir. Devido a estratégia de implementação da ferramenta, uma única instância de uma classe que herda da classe ``ModelHtmlParse`` pode chamar o método ``useHtmlAttributesValues`` múltiplas vezes, passando cada vez diferentes instâncias de classes que herdam de ``ModelHtmlAttributesUse``.

```Java
package br.ufscar.dc.pooa.java.getnews;

/* Exemplificando a utilizacao das classes implementadas para a extracao de titulos
 * e links de noticias do Oul, Globo e Bbc de londres, realizando o print na tela,
 *  geracao de um csv file e geracao de uma imagem de um wordCloud. */
public class Main {

    // Possivel que ocorra um erro de conexao, caso o website recuse o handshake!
    public static void main(String[] args){
        // printScreenOul(); // Printa na tela.
        // printCsvGlobo(); // Gera um arquivo CSV na raiz do projeto.
        printWordCloudBbc(); // Gera uma imagem de um wordCloud na raiz do projeto.
    }
    
    public static void printCsvGlobo(){
        // Instanciando classe que dita o modelo de como serão utilizadas
        // as strings retornadas do globo.
        CsvPrint use = new CsvPrint();
        
        // Instanciando a classe que define de onde e como serão retiradas as strings
        GloboParser globo = new GloboParser();
        
        // Utiliza as strings retornadas nesta classe.
        globo.useHtmlAttributesValues(use);
    }
    
    public static void printScreenOul(){
        ScreenPrint use = new ScreenPrint();
        OulParser oul = new OulParser();
        oul.useHtmlAttributesValues(use);
    }
    
    public static void printWordCloudBbc(){
        WordCloudPrint use = new WordCloudPrint();
        BbcLondonParser bbc = new BbcLondonParser();
        bbc.useHtmlAttributesValues(use);
    }
}
```
