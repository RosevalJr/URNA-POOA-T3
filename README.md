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

## Como foram distribuidas as responsabilidades e por que?
## Como o projeto pode ser estendido sem causar impacto no restante da arquitetura?
## O "contrato" de cada tipo é respetado, independente da instância real daquele tipo?
## Como está a dependência entre as classes/módulos/funções? Um móduolo exerga apenas aquilo que precisa do outro?
