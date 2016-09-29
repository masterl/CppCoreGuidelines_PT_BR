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

### <a name="R0"></a>In.0: Não entre em pânico!

Tome seu tempo para entender as implicações de uma regra de diretriz no seu programa.

Essas diretrizes são projetadas de acordo com o princípio "subconjunto do superconjunto" ([Stroustrup05](#Stroustrup05)).
Elas não simplesmente definem um subconjunto do C++ para ser usado (para confiabilidade, segurança, performance, etc).
Ao invés disso, elas recomendam fortemente o use de algumas "extensões" simples (([Guidelines Support Library](#S-gsl))) que tornam redundante o uso das funcionalidades mais propensas a erro do C++, de forma que estas possam ser banidas (no nosso conjunto de regras).

As regras enfatizam segurança estática de tipos e segurança de recursos.
Por essa razão, elas enfatizam possibilidades para checagem de limites, para evitar derreferência de `nullptr`, para evitar ponteiros soltos e o uso sistemático de exceções (por meio de RAII).
Parte para atingir estes objetivos e parte para minimizar código obscuro como fonte de erros, as regras também enfatizam simplicidade e a abstrair partes complexas em interfaces bem especificadas.

Muitas das diretrizes são prescritivas.
Ficamos incomodados com regras que simplesmente dizem "não faça isso!" sem oferecer uma alternativa.
Uma consequência disso é que algumas diretrizes só tem embasamento em heurísticas, ao invés de checagens precisas e mecânicamente verificáveis.
Outras diretrizes articulam princípios gerais. Para essas diretrizes mais gerais, regras mais específicas e detalhadas proveem checagem parcial.

Estas diretrizes falam do núcleo do C++ e seu uso.
Esperamos que a maioria das organizações, áreas de aplicação específicas e projetos ainda maiores vão precisar de mais regras, possivelmente mais restrições e maior suporte de bibliotecas.
Por exemplo, programadores em tempo real normalmente não podem usar o `free store` (memória dinâmica) livremente e estarão mais restritos na escolha de bibliotecas.
Encorajamos o desenvolvimento de regras mais específicas como adendo a essas diretrizes centrais.
Construa sua pequena biblioteca base e use-a, ao invés de rebaixar seu nível de programação ao glorioso código assembly.

As regras são projetadas para permitir [Adoção gradual](#S-modernizing).

Algumas regras visam aumentar várias formas de segurança enquanto outras visam reduzir a probabilidade de acidentes, muitas fazem ambos.
As diretrizes destinadas a prevenir acidentes frequentemente banem C++ perfeitamente legal.
Entretanto, quando existem duas maneiras de expressar uma ideia e uma é sabidamente uma fonte de erros e a outra não, tentaremos guiar os programadores para utilizarem a segunda.

## <a name="SS-non"></a>In.not: Não-objetivos

As regras não foram feitas para serem mínimas ou ortogonais.
Particularmente, regras gerais podem ser simples, mas não executáveis.
Normalmente é difícil de entender as implicações de uma regra geral.
Regras mais especializadas são mais fáceis de entender e executar, mas sem regras gerais, elas seriam somente uma longa lista de casos especiais.
Nós provemos regras que visam ajudar novatos, bem como regras que auxiliam uso avançado da linguagem.
Algumas regras podem ser completamente executadas, enquanto outras são baseadas em heurísticas.

These rules are not meant to be read serially, like a book.
You can browse through them using the links.
However, their main intended use is to be targets for tools.
That is, a tool looks for violations and the tool returns links to violated rules.
The rules then provide reasons, examples of potential consequences of the violation, and suggested remedies.

These guidelines are not intended to be a substitute for a tutorial treatment of C++.
If you need a tutorial for some given level of experience, see [the references](#S-references).

This is not a guide on how to convert old C++ code to more modern code.
It is meant to articulate ideas for new code in a concrete fashion.
However, see [the modernization section](#S-modernizing) for some possible approaches to modernizing/rejuvenating/upgrading.
Importantly, the rules support gradual adoption: It is typically infeasible to convert all of a large code base at once.

These guidelines are not meant to be complete or exact in every language-technical detail.
For the final word on language definition issues, including every exception to general rules and every feature, see the ISO C++ standard.

The rules are not intended to force you to write in an impoverished subset of C++.
They are *emphatically* not meant to define a, say, Java-like subset of C++.
They are not meant to define a single "one true C++" language.
We value expressiveness and uncompromised performance.

The rules are not value-neutral.
They are meant to make code simpler and more correct/safer than most existing C++ code, without loss of performance.
They are meant to inhibit perfectly valid C++ code that correlates with errors, spurious complexity, and poor performance.

The rules are not perfect.
A rule can do harm by prohibiting something that is useful in a given situation.
A rule can do harm by failing to prohibit something that enables a serious error in a given situation.
A rule can do a lot of harm by being vague, ambiguous, unenforceable, or by enabling every solution to a problem.
It is impossible to completely meet the "do no harm" criteria.
Instead, our aim is the less ambitious: "Do the most good for most programmers";
if you cannot live with a rule, object to it, ignore it, but don't water it down until it becomes meaningless.
Also, suggest an improvement.

## <a name="SS-force"></a>In.force: Execução

Rules with no enforcement are unmanageable for large code bases.
Enforcement of all rules is possible only for a small weak set of rules or for a specific user community.

* But we want lots of rules, and we want rules that everybody can use.
* But different people have different needs.
* But people don't like to read lots of rules.
* But people can't remember many rules.

So, we need subsetting to meet a variety of needs.

* But arbitrary subsetting leads to chaos.

We want guidelines that help a lot of people, make code more uniform, and strongly encourage people to modernize their code.
We want to encourage best practices, rather than leave all to individual choices and management pressures.
The ideal is to use all rules; that gives the greatest benefits.

This adds up to quite a few dilemmas.
We try to resolve those using tools.
Each rule has an **Enforcement** section listing ideas for enforcement.
Enforcement might be by code review, by static analysis, by compiler, or by run-time checks.
Wherever possible, we prefer "mechanical" checking (humans are slow, inaccurate, and bore easily) and static checking.
Run-time checks are suggested only rarely where no alternative exists; we do not want to introduce "distributed fat".
Where appropriate, we label a rule (in the **Enforcement** sections) with the name of groups of related rules (called "profiles").
A rule can be part of several profiles, or none.
For a start, we have a few profiles corresponding to common needs (desires, ideals):

* **type**: No type violations (reinterpreting a `T` as a `U` through casts, unions, or varargs)
* **bounds**: No bounds violations (accessing beyond the range of an array)
* **lifetime**: No leaks (failing to `delete` or multiple `delete`) and no access to invalid objects (dereferencing `nullptr`, using a dangling reference).

The profiles are intended to be used by tools, but also serve as an aid to the human reader.
We do not limit our comment in the **Enforcement** sections to things we know how to enforce; some comments are mere wishes that might inspire some tool builder.

Tools that implement these rules shall respect the following syntax to explicitly suppress a rule:

    [[suppress(tag)]]

where "tag" is the anchor name of the item where the Enforcement rule appears (e.g., for [C.134](#Rh-public) it is "Rh-public"), the
name of a profile group-of-rules ("type", "bounds", or "lifetime"),
or a specific rule in a profile ([type.4](#Pro-type-cstylecast), or [bounds.2](#Pro-bounds-arrayindex)).

## <a name="SS-struct"></a>In.struct: Estrutura deste documento

Each rule (guideline, suggestion) can have several parts:

* The rule itself -- e.g., **no naked `new`**
* A rule reference number -- e.g., **C.7** (the 7th rule related to classes).
  Since the major sections are not inherently ordered, we use a letter as the first part of a rule reference "number".
  We leave gaps in the numbering to minimize "disruption" when we add or remove rules.
* **Reason**s (rationales) -- because programmers find it hard to follow rules they don't understand
* **Example**s -- because rules are hard to understand in the abstract; can be positive or negative
* **Alternative**s -- for "don't do this" rules
* **Exception**s -- we prefer simple general rules. However, many rules apply widely, but not universally, so exceptions must be listed
* **Enforcement** -- ideas about how the rule might be checked "mechanically"
* **See also**s -- references to related rules and/or further discussion (in this document or elsewhere)
* **Note**s (comments) -- something that needs saying that doesn't fit the other classifications
* **Discussion** -- references to more extensive rationale and/or examples placed outside the main lists of rules

Some rules are hard to check mechanically, but they all meet the minimal criteria that an expert programmer can spot many violations without too much trouble.
We hope that "mechanical" tools will improve with time to approximate what such an expert programmer notices.
Also, we assume that the rules will be refined over time to make them more precise and checkable.

A rule is aimed at being simple, rather than carefully phrased to mention every alternative and special case.
Such information is found in the **Alternative** paragraphs and the [Discussion](#S-discussion) sections.
If you don't understand a rule or disagree with it, please visit its **Discussion**.
If you feel that a discussion is missing or incomplete, enter an [Issue](https://github.com/isocpp/CppCoreGuidelines/issues)
explaining your concerns and possibly a corresponding PR.

This is not a language manual.
It is meant to be helpful, rather than complete, fully accurate on technical details, or a guide to existing code.
Recommended information sources can be found in [the references](#S-references).

## <a name="SS-sec"></a>In.sec: Principais seções

* [In: Introdução](#S-introduction)
* [P: Filosofia](#S-philosophy)
* [I: Interfaces](#S-interfaces)
* [F: Funções](#S-functions)
* [C: Classes e hierarquias de classes](#S-class)
* [Enum: Enumerações](#S-enum)
* [R: Gerência de recursos](#S-resource)
* [ES: Expressões e declarações](#S-expr)
* [E: Resolução de erros](#S-errors)
* [Con: Constantes e imutabilidade](#S-const)
* [T: Templates e programação genérica](#S-templates)
* [CP: Concorrência](#S-concurrency)
* [SL: The Standard library](#S-stdlib)
* [SF: Arquivos fonte](#S-source)
* [CPL: Programação estilo C](#S-cpl)
* [Pro: Perfis](#S-profile)
* [GSL: Guideline support library](#S-gsl)
* [FAQ: Respostas a perguntas frequentes](#S-faq)

Seções de suporte:

* [NL: Nomenclatura e layout](#S-naming)
* [Per: Performance](#S-performance)
* [N: Não-Regras e mitos](#S-not)
* [RF: Referências](#S-references)
* [Apêndice A: Bibliotecas](#S-libraries)
* [Apêndice B: Modernizando código](#S-modernizing)
* [Apêndice C: Discussão](#S-discussion)
* [Glossário](#S-glossary)
* [Pendências: Proto-regras não-classificadas](#S-unclassified)
