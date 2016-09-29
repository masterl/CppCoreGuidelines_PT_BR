# <a name="main"></a>Diretrizes Centrais para C++

#### Tradução de um snapshot de 28/09/2016
***!!! tradução em progresso !!!***

Editores:

* [Bjarne Stroustrup](http://www.stroustrup.com)
* [Herb Sutter](http://herbsutter.com/)

Este documento é um rascunho inicial. Está incorreto, incompleto e porcamente formatado.
Se fosse um projeto open source (código), esta seria a versão 0.7.
Cópia, utilização, modificação e criação de trabalhos derivados deste projeto está licenciada pela licença MIT.
Contribuir para este projeto requer concordar com a Licença de Contribuidor. Ver [LICENSE](LICENSE) para maiores detalhes.
Deixamos este projeto disponível para "usuários amigáveis" usarem, copiarem, modificarem e derivarem dele, na esperança de uma contribuição positiva.

Comentários e sugestões de melhoria são bem-vindas.
Planejamos modificar e extender este documento conforme nosso entendimento melhorar e a linguagem e o conjunto de bibliotecas disponíveis melhorem também.
Antes de comentar, ver [a introdução](#S-introduction) que define nossos objetivos e abordagem geral.
A lista de contribuidores está [aqui](#SS-ack).

Problemas:

* Os conjuntos de regras não foram profundamente checados em termos de completude, consistência ou exequibilidade.
* Pontos de exclamação triplos (???) marcam falta de informações
* Atualizar seções de referência; muitos dos exemplos pre-C++11 são muito antigos.
* Para uma lista de pendências mais ou menos atualizada, ver: [Pendências: Proto-regras não-classificadas](#S-unclassified)

Você pode [ler uma explicação do escopo e estrutura destas diretrizes](#S-abstract) ou apenas ir direto a:

* [In: Introdução](#S-introduction)
* [P: Filosofia](#S-philosophy)
* [I: Interfaces](#S-interfaces)
* [F: Funções](#S-functions)
* [C: Classes and hierarquias de classes](#S-class)
* [Enum: Enumerações](#S-enum)
* [R: Gerência de recursos](#S-resource)
* [ES: Expressões e declarações](#S-expr)
* [Per: Performance](#S-performance)
* [CP: Concorrência](#S-concurrency)
* [E: Resolução de erros](#S-errors)
* [Con: Constantes e imutabilidade](#S-const)
* [T: Templates e programação genérica](#S-templates)
* [CPL: Programação no estilo C](#S-cpl)
* [SF: Arquivos fonte](#S-source)
* [SL: A biblioteca padrão](#S-stdlib)

Seções de suporte:

* [A: Idéias arquiteturais](#S-A)
* [N: Não-regras e mitos](#S-not)
* [RF: Referências](#S-references)
* [Pro: Perfis](#S-profile)
* [GSL: Guideline support library](#S-gsl)
* [NL: Nomenclatura e layout](#S-naming)
* [FAQ: Respostas para perguntas frequentes](#S-faq)
* [Apêndice A: Bibliotecas](#S-libraries)
* [Apêndice B: Modernizando código](#S-modernizing)
* [Apêndice C: Discussão](#S-discussion)
* [Glossário](#S-glossary)
* [Pendências: Proto-regras não-classificadas](#S-unclassified)

ou olhe uma funcionalidade específica da linguagem

* [atribuição](#S-???)
* [`class`](#S-class)
* [construtor](#SS-ctor)
* [`class` derivada](#SS-hier)
* [destrutor](#SS-dtor)
* [exceção](#S-errors)
* [`for`](#S-???)
* [`inline`](#S-class)
* [inicialização](#S-???)
* [expressão lambda](#SS-lambdas)
* [`operator`](#S-???)
* [`public`, `private`, e `protected`](#S-???)
* [`static_assert`](#S-???)
* [`struct`](#S-class)
* [`template`](#S-???)
* [`unsigned`](#S-???)
* [`virtual`](#SS-hier)

Definição de termos usados para expressar e discutir as regras, que não são técnicos da linguagem, mas se referem a técnicas de projeto e programação

* erro
* exceção
* falha
* invariante
* vazamento
* pre-condição
* pós-condição
* recurso
* garantia de exceção

# <a name="S-abstract"></a>Abstract

Este documento é um conjunto de diretrizes para utilizar C++ bem.
O objetivo deste documento é ajudar as pessoas a utilizarem C++ moderno efetivamente.
Por "C++ moderno" queremos dizer C++11 e C++14 (e em breve C++17).
Em outras palavras, como você gostaria que seu código fosse visto em 5 anos, dado que você pode iniciar agora? Em 10 anos?

As diretrizes focam em problemas de alto-nível, tais como interfaces, gerência de recursos, gerência de memória e concorrência.
Tais regras afetam arquitetura da aplicação e projeto de bibliotecas.
Seguir as diretrizes irá proporcionar código com segurança estática de tipos, sem vazamento de memória, além de pegar muitos dos erros de lógica que são comums hoje em dia e irá rodar rápido -- você pode fazer as coisas da forma correta.

Estamos menos preocupados com problemas de baixo-nível, tais como convenções de nome e estilo de indentação, entretanto nenhum tópico que possa auxiliar um programador está fora dos limites.

Nosso conjunto inicial de regras enfatizam segurança (de diversas formas) e simplicidade. Elas podem até ser muito estritas.
Esperamos ter que introduzir exceções para melhor acomodar necessidades do mundo real.
Também precisamos de mais regras.

Você irá considerar algumas das regras contrárias às suas expectativas ou mesmo contrárias à sua experiência.
Se não tivéssemos sugerido que você mudasse seu estilo de codificação de alguma forma, então falhamos!
Por favor teste para verificar ou negar as regras!
Adoraríamos ter algumas de nossas regras melhor embasadas por medidas ou melhores exemplos.

Você irá achar algumas das regras muito óbvias ou mesmo triviais.
Tente lembrar que um dos propósitos de uma diretriz é ajudar alguém que tenha pouca experiência ou que venha de um ambiente ou linguagem diferente a se nivelarem.

As diretrizes são projetadas para terem suporte de uma ferramenta de análise.
Violações das regras serão indicadas com referências (ou links) para a regra em questão.
Não esperamos que você memorize todas as regras antes de começar a escrever código.
Uma maneira de descrever estas diretrizes é como uma especificação para ferramentas que coincidentemente é legível por humanos.

As diretrizes se destinam a serem introduzidas de forma gradual à uma base de código.
Planejamos construir ferramentas para isso e esperamos que outros façam o mesmo.

Comentários e sugestões para melhorias são bem-vindas.
Planejamos modificar e extender este documento conforme nosso entendimento melhorar e a linguagem e o conjunto de bibliotecas disponíveis melhorem também.

# <a name="S-introduction"></a>In: Introdução

Este é um conjunto de diretrizes para C++ moderno, C++14, e absorvendo melhorias futuras e levando Especificações Técnicas ISO (TSs) (*ISO Technical Specifications*) em consideração.
O objetivo é ajudar programadores C++ a escreverem códigos mais simples, eficientes e manuteníveis.

Índice de introdução:

* [In.target: Leitores alvo](#SS-readers)
* [In.aims: Objetivos](#SS-aims)
* [In.not: Não-objetivos](#SS-non)
* [In.force: Execução](#SS-force)
* [In.struct: Estrutura deste documento](#SS-struct)
* [In.sec: Seções importantes](#SS-sec)

## <a name="SS-readers"></a>In.target: Leitores alvo

Todos os programadores C++. Isso inclui [programadores que talvez considerem C](#-cpl).

## <a name="SS-aims"></a>In.aims: Objetivos

A proposta deste documento é auxiliar desenvolvedores a adotar C++ moderno (C++11, C++14 e, em breve, C++17) e para atingirmos um estilo mais uniforme através das bases de código.

Nós não temos a ilusão de que cada uma dessas regras possam ser efetivamente aplicadas a todas as bases de código. Atualizar sistemas antigos é difícil. Entretanto, acreditamos que um programa que utilize uma regra é menos propenso a erros e mais manutenível que um que não utilize. Frequentemente, as regras levam a um desenvolvimento inicial mais rápido/fácil.
Até onde sabemos, essas regras levam a código que rodam tão bem ou melhor que técnicas antigas ou mais convencionais; são regras que seguem o princípio de zero gasto adicional (*zero-overhead*) (*"O que vc não utiliza, você não irá pagar por isso"* ou *"quando você utiliza um mecanismo de abstração de forma correta, você obtém performance pelo menos tão boa quanto se você tivesse escrito o código a mão utilizando construções de baixo-nível da linguagem"*).
Considere estas diretrizes como ideais para novos códigos, oportunidades para explorar quando estiver trabalhando com código antigo e tente se aproximar desses ideais o máximo possível.
Lembre:
