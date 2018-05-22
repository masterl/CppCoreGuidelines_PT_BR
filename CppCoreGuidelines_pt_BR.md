# <a name="main"></a>Diretrizes Centrais para C++

#### Tradução de um snapshot de 21/04/2018

***!!! tradução em progresso !!!***

Editores:

* [Bjarne Stroustrup](http://www.stroustrup.com)
* [Herb Sutter](http://herbsutter.com/)

Este é um documento vivo que sofre melhorias constantemente.
Se fosse um projeto open source (código), esta seria a versão 0.8.
Cópia, utilização, modificação e criação de trabalhos derivados deste projeto está licenciada sob uma licença MIT.
Contribuir para este projeto requer concordar com a Licença de Contribuidor. Ver [LICENSE](LICENSE) para maiores detalhes.
Deixamos este projeto disponível para "usuários amigáveis" usarem, copiarem, modificarem e derivarem dele, na esperança de uma contribuição positiva.

Comentários e sugestões de melhoria são bem-vindos.
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
* [CP: Concorrência e paralelismo](#S-concurrency)
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
* [NL: Nomenclatura e regras de layout](#S-naming)
* [FAQ: Respostas para perguntas frequentes](#S-faq)
* [Apêndice A: Bibliotecas](#S-libraries)
* [Apêndice B: Modernizando código](#S-modernizing)
* [Apêndice C: Discussão](#S-discussion)
* [Apêndice D: Ferramentas de suporte](#S-tools)
* [Glossário](#S-glossary)
* [Pendências: Proto-regras não-classificadas](#S-unclassified)

ou olhe uma funcionalidade específica da linguagem

* atribuição:
[tipos regulares](#Rc-regular) --
[prefira inicialização](#Rc-initialize) --
[cópia](#Rc-copy-semantics) --
[movimento](#Rc-move-semantics) --
[outras operações](#Rc-matched) --
[padrão](#Rc-eqdefault)
* `class`:
[dados](#Rc-org) --
[invariante](#Rc-struct) --
[membros](#Rc-member) --
[helpers](#Rc-helper) --
[tipos concretos](#SS-concrete) --
[construtores, = e destrutores](#S-ctor) --
[hierarquia](#SS-hier) --
[operadores](#SS-overload)
* `concept`:
[regras](#SS-concepts) --
[em programação genérica](#Rt-raise) --
[argumentos de templates](#Rt-concepts) --
[semântica](#Rt-low)
* construtor:
[invariante](#Rc-struct) --
[estabelecer invariante](#Rc-ctor) --
[`throw`](#Rc-throw) --
[default](#Rc-default0) --
[não necessário](#Rc-default) --
[`explicit`](#Rc-explicit) --
[delegante](#Rc-delegating) --
[`virtual`](#Rc-ctor-virtual)
* classe derivada:
[quando usar](#Rh-domain) --
[como interface](#Rh-abstract) --
[destrutores](#Rh-dtor) --
[cópia](#Rh-copy) --
[getters e setters](#Rh-get) --
[herança múltipla](#Rh-mi-interface) --
[sobrecarga](#Rh-using) --
[fatiando](#Rc-copy-virtual) --
[`dynamic_cast`](#Rh-dynamic_cast)
* destrutores:
[e construtores](#Rc-matched) --
[quando são necessários?](#Rc-dtor) --
[não devem falhar](#Rc-dtor-fail)
* exceção:
[erros](#S-errors) --
[`throw`](#Re-throw) --
[somente para erros](#Re-errors) --
[`noexcept`](#Re-noexcept) --
[minimize o uso de `try`](#Re-catch) --
[e se eu não puder usar exceções?](#Re-no-throw-codes)
* `for`:
[range-for e for](#Res-for-range) --
[for e while](#Res-for-while) --
[for-initializer](#Res-for-init) --
[corpo vazio](#Res-empty) --
[variável de laço](#Res-loop-counter) --
[tipo da variável de laço ???](#Res-???)
* função:
[nomenclatura](#Rf-package) --
[operação singular](#Rf-logical) --
[no throw](#Rf-noexcept) --
[argumentos](#Rf-smart) --
[passagem de argumentos](#Rf-conventional) --
[múltiplos valores de retorno](#Rf-out-multi) --
[ponteiros](#Rf-return-ptr) --
[lambdas](#Rf-capture-vs-overload)
* `inline`:
[funções pequenas](#Rf-inline) --
[em cabeçalhos](#Rs-inline)
* inicialização:
[sempre](#Res-always) --
[prefira `{}`](#Res-list) --
[lambdas](#Res-lambda-init) --
[inicializadores 'in-class'](#Rc-in-class-initializer) --
[membros de classe](#Rc-initialize) --
[funções fábrica](#Rc-factory)
* expressão lambda:
[quando usar](#SS-lambdas)
* operador:
[convencional](#Ro-conventional) --
[evite operadores de conversão](#Ro-conversion) --
[e lambdas](#Ro-lambda)
* `public`, `private`, e `protected`:
[escondendo informação](#Rc-private) --
[consistência](#Rh-public) --
[`protected`](#Rh-protected)
* `static_assert`:
[checagem em tempo de compilação](#Rp-compile-time) --
[e concepts](#Rt-check-class)
* `struct`:
[para organizar dados](#Rc-org) --
[use se não houver invariantes](#Rc-struct) --
[nenhum membro privado](#Rc-class)
* `template`:
[abstração](#Rt-raise) --
[contêiners](#Rt-cont) --
[concepts](#Rt-concepts)
* `unsigned`:
[e `signed`](#Res-mix) --
[manipulação de bits](#Res-unsigned)
* `virtual`:
[interfaces](#Ri-abstract) --
[não `virtual`](#Rc-concrete) --
[destrutor](#Rc-dtor-virtual) --
[nunca falhe](#Rc-dtor-fail)

Definição de termos usados para expressar e discutir as regras, que não são técnicos da linguagem, mas se referem a técnicas de projeto e programação

* asserção: ???
* erro: ???
* exceção: ???
* garantia de exceção: ???
* falha: ???
* invariante: ???
* vazamento: ???
* biblioteca: ???
* pré-condição: ???
* pós-condição: ???
* recurso: ???

# <a name="S-abstract"></a>Abstract

Este documento é um conjunto de diretrizes para utilizar C++ bem.
O objetivo deste documento é ajudar as pessoas a utilizarem C++ moderno efetivamente.
Por "C++ moderno" queremos dizer C++17, C++14 e C++11.
Em outras palavras, como você gostaria que seu código fosse visto em 5 anos, dado que você pode iniciar agora? Em 10 anos?

As diretrizes focam em problemas de alto-nível, tais como interfaces, gerência de recursos, gerência de memória e concorrência.
Tais regras afetam arquitetura da aplicação e projeto de bibliotecas.
Seguir as diretrizes irá proporcionar código com segurança estática de tipos, sem vazamento de recursos, além de pegar muitos dos erros de lógica que são comums hoje em dia e irá rodar rápido -- você pode fazer as coisas da forma correta.

Estamos menos preocupados com problemas de baixo-nível, tais como convenções de nomenclatura e estilo de indentação, entretanto, nenhum tópico que possa auxiliar um programador está fora dos limites.

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

Este é um conjunto de diretrizes para C++ moderno, C++17, C++14 e C++11, absorvendo melhorias futuras e levando em consideração Especificações Técnicas ISO (TSs) (*ISO Technical Specifications*).
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

A proposta deste documento é auxiliar desenvolvedores a adotar C++ moderno (C++17, C++14 e C++11) e para atingirmos um estilo mais uniforme através das bases de código.

Nós não temos a ilusão de que cada uma dessas regras possam ser efetivamente aplicadas a todas as bases de código. Atualizar sistemas antigos é difícil. Entretanto, acreditamos que um programa que utilize uma regra é menos propenso a erros e mais manutenível que um que não utilize. Frequentemente, as regras levam a um desenvolvimento inicial mais rápido/fácil.
Até onde sabemos, essas regras levam a código que rodam tão bem ou melhor que técnicas antigas ou mais convencionais; são regras que seguem o princípio de zero gasto adicional (*zero-overhead*) (*"O que vc não utiliza, você não irá pagar por isso"* ou *"quando você utiliza um mecanismo de abstração de forma correta, você obtém performance pelo menos tão boa quanto se você tivesse escrito o código a mão utilizando construções de baixo-nível da linguagem"*).
Considere estas diretrizes como ideais para novos códigos, oportunidades para explorar quando estiver trabalhando com código antigo e tente se aproximar desses ideais o máximo possível.
Lembre:

### <a name="R0"></a>In.0: Não entre em pânico!

Tome seu tempo para entender as implicações de uma regra das diretrizes no seu programa.

Essas diretrizes são projetadas de acordo com o princípio "subconjunto do superconjunto" ([Stroustrup05](#Stroustrup05)).
Elas não simplesmente definem um subconjunto do C++ para ser usado (para confiabilidade, segurança, performance, etc).
Ao invés disso, elas recomendam fortemente o uso de algumas "extensões" simples ([Guidelines Support Library](#S-gsl)) que tornam redundante o uso das funcionalidades mais propensas a erro do C++, de forma que estas possam ser banidas (no nosso conjunto de regras).

As regras enfatizam segurança estática de tipos e segurança de recursos.
Por essa razão, elas enfatizam possibilidades para checagem de limites, para evitar derreferência de `nullptr`, para evitar ponteiros soltos e o uso sistemático de exceções (por meio de *RAII*).
Em parte para atingir estes objetivos e parte para minimizar código obscuro como fonte de erros, as regras também enfatizam simplicidade e a abstrair as partes complexas em interfaces bem especificadas.

Muitas das diretrizes são prescritivas.
Ficamos incomodados com regras que simplesmente dizem "não faça isso!" sem oferecer uma alternativa.
Uma consequência disso é que algumas diretrizes só tem embasamento em heurísticas, ao invés de checagens precisas e mecânicamente verificáveis.
Outras diretrizes articulam princípios gerais. Para essas diretrizes mais gerais, regras mais específicas e detalhadas proveem checagem parcial.

Estas diretrizes falam do núcleo do C++ e seu uso.
Esperamos que a maioria das organizações, áreas de aplicação específicas e projetos ainda maiores vão precisar de mais regras, possivelmente mais restrições e maior suporte de bibliotecas.
Por exemplo, programadores de aplicações em tempo real normalmente não podem usar o `free store` (memória dinâmica) livremente e estarão mais restritos na escolha de bibliotecas.
Encorajamos o desenvolvimento de regras mais específicas como adendo a essas diretrizes centrais.
Construa sua pequena biblioteca base e use-a, ao invés de rebaixar seu nível de programação ao glorioso código assembly.

As regras são projetadas para permitir [adoção gradual](#S-modernizing).

Algumas regras visam aumentar várias formas de segurança enquanto outras visam reduzir a probabilidade de acidentes, muitas fazem ambos.
As diretrizes destinadas a prevenir acidentes frequentemente banem trechos C++ perfeitamente legais.
Entretanto, quando existem duas maneiras de expressar uma ideia e uma é sabidamente uma fonte de erros e a outra não, tentaremos guiar os programadores para utilizarem a segunda.

## <a name="SS-non"></a>In.not: Não-objetivos

As regras não foram feitas para serem mínimas ou ortogonais.
Particularmente, regras gerais podem ser simples, mas talvez não seja possível impor tais regras.
Normalmente é difícil de entender as implicações de uma regra geral.
Regras mais especializadas são mais fáceis de entender e impor, mas sem regras gerais, elas seriam somente uma longa lista de casos especiais.
Nós provemos regras que visam ajudar novatos, bem como regras que auxiliam uso avançado da linguagem.
Algumas regras podem ser completamente impostas, enquanto outras são baseadas em heurísticas.

Estas regras não foram feitas para serem lidas sequencialmente, como um livro.
Você pode verificá-las usando os links.
Entretanto, seu maior uso é para serem alvos para ferramentas.
Ou seja, uma ferramenta procura violações das regras e retorna links para as regras que foram violadas.
As regras provêem razões, exemplos de prováveis consequências da violação e sugestões de correção.

Essas diretrizes não tem por objetivo substituir tutoriais à linguagem C++.
Se você precisa de algum tipo de tutorial para algum nível de experiência na linguagem, veja [as referências](#S-references).

Este não é um guia de como converter código C++ "velho" em código mais moderno.
O objetivo é articular idéias para serem utilizadas em códigos novos de forma concreta.
Entretanto, veja [a seção de modernização](#S-modernizing) para algumas abordagens para modernizar/rejuvenescer/atualizar código antigo.
Importante: as regras suportam adoção gradual, afinal normalmente é impraticável querer converter uma grande base de código inteira de uma só vez.

Essas diretrizes não têm a intenção de serem completas ou exatas em todos os detalhes técnicos da linguagem.
Para uma palavra final em problemas relativos à definição da linguagem, incluindo todas as exceções às regras gerais e cada funcionalidade, olhe o padrão ISO C++.

As regras não têm a intenção de forçar você a programar utilizando um subconjunto empobrecido do C++.
Elas *enfaticamente* não se destinam a definir um tipo de subset "a la Java" de C++.
Elas não se destinam a definir uma única "verdadeira linguagem C++".
Nós valorizamos expressividade e performance sem compromisso.

As regras não são neutras.
Elas foram feitas para tornar código mais simples e mais correto/seguro do que a maior parte dos códigos C++ existentes, sem perda de performance.
Elas foram feitas para inibir códigos C++ perfeitamente válidos que correlacionam com erros, complexidade falsa e baixa performance.

As regras não são perfeitas.
Uma regra pode causar mal ao proibir algo que é útil em uma determinada situação.
Uma regra pode causar mal ao falhar em proibir algo que permita um erro sério em uma determinada situação.
Uma regra pode causar muito mal ao ser vaga, ambígua, não ser possível de impor ou por permitir toda solução para um problema.
É impossível garantir completamente o critério de "não fazer mal".
Ao invés disso, nosso objetivo é menos ambicioso: "Fazer o maior bem para a maioria dos programadores".
Se você não puder viver sob uma determinada regra, se oponha a ela, ignore-a, mas não a altere de forma que ela se torne sem sentido.
Não esqueça de sugerir melhorias.

## <a name="SS-force"></a>In.force: Imposição das regras

Ter regras sem imposição não é algo gerenciável em grandes bases de código.
Imposição das regras só é possível para um pequeno e fraco conjunto de regras ou para uma comunidade de usuários específica.

* Mas queremos várias regras e queremos regras que todos possam usar.
* Mas pessoas diferentes tem necessidades diferentes.
* Mas pessoas não gostam de ler um monte de regras.
* Mas pessoas não vão lembrar de tantas regras.

Então, precisamos de um subconjunto que atenda a várias necessidades.

* Mas criar um subconjunto arbitrário leva ao caos.

Nós queremos diretrizes que ajudem várias pessoas, torne o código mais uniforme e que encorajam as pessoas a modernizar seu código.
Nós queremos encorajar melhores práticas, ao invés de deixar tudo nas mãos de escolhas individuais e pressões gerenciais.
O ideal é utilizar todas as regras que tragam os melhores benefícios.

Isso traz um monte de dilemas.
Nós tentamos resolvê-los utilizando ferramentas.
Cada regra tem um campo **Imposição** listando idéias de como impor tal regra.
Imposição pode ser feita por revisão de código, por análise estática, pelo compilador ou por checagens em tempo de execução.
Sempre que possível, nós preferimos checagem "mecânica" (humanos são lentos, inexatos e se entediam facilmente) e análise estática.
Checagens em tempo de execução são sugeridas raramente onde nenhuma outra alternativa existe; nós não queremos introduzir "gordura distribuída".
Sempre que apropriado, nós categorizamos uma regra (nas seções de **Imposição**) com o nome de grupos de regras associadas (chamadas "perfis").
Uma regra pode ser parte de vários perfis ou nenhum.
Para começar, temos alguns perfis correspondente a necessidades comuns (desejos, ideais):

* **type**: Nenhuma violação de tipo (re-interpretar um `T` como `U` através de casts, unions ou `varargs`)
* **bounds**: Nenhuma violação de limites (acessar posições que não pertençam a um array)
* **lifetime**: Nenhum vazamento (esquecer de executar `delete` ou múltiplos `delete`) e nenhum acesso à objetos inválidos (de-referenciar `nullptr`, utilizando uma referência "solta")

Os perfis se destinam a serem usados por ferramentas, mas também auxiliar o leitor humano.
Nós não limitamos nosso comentário nas seções de **Imposição** às coisas que sabemos como impor; alguns comentários são simples desejos que talvez inspirem algum construtor de ferramenta.

Ferramentas que implementem essas regras devem respeitar a seguinte sintaxe para suprimir explicitamente uma regra:

    [[gsl::suppress(tag)]]

onde "tag" é nome da âncora do item onde a regra de Imposição aparece.

Exemplos:

- para [C.134](#Rh-public) é "Rh-public")
- o nome de um perfil de grupo de regras ("type", "bounds" ou "lifetime")
- uma regra específica em um perfil ([type.4](#Pro-type-cstylecast))
- [bounds.2](#Pro-bounds-arrayindex)

## <a name="SS-struct"></a>In.struct: Estrutura deste documento

Cada regra (diretriz, sugestão) pode ter várias partes:

* A regra propriamente dita -- exemplo: **nenhum `new` nu**.
* Um número de referência da regra -- exemplo: **C.7** (a sétima regra relativa à classes).
Dado que as seções principais não são inerentemente ordenadas, nós utilizamos letras como prefixo do "número" de referência de uma regra.
Nós deixamos lacunas nas numerações para minimizar "disrupções" quando adicionarmos ou removermos regras.
* **Razõe**s (lógica) -- porque programadores acham difícil seguir regras que eles não entendem
* **Exemplo**s -- porque regras são difíceis de entender de forma abstrata; pode ser positivo ou negativo
* **Alternativa**s -- para regras do tipo "não faça isso"
* **Exceções** -- nós preferimos regras simples e genéricas, entretanto, muitas regras são largamente aplicáveis, mas não universalmente, então exceções devem ser listadas
* **Imposição** -- idéias de como a regra pode ser verificada de forma "mecânica"
* **Veja também** -- fazem referência a regras correlacionadas e/ou discussões aprofundadas (neste documento ou em outro lugar)
* **Nota**s (comentários) -- algo que precisa ser dito, mas que não se encaixa em nenhuma outra classificação
* **Discussão** -- referências para explicações mais aprofundadas e/ou exemplos localizados fora das listas principais de regras

Algumas regras são difíceis de checar de forma mecânica, mas todas elas atendem o requisito mínimo de que um programador experiente pode notar várias violações sem grandes problemas.
Nós esperamos que ferramentas "mecânicas" sejam melhoradas com o tempo para notarem o que tal programador experiente notaria.
Também assumimos que as regras serão refinadas com o tempo para se tornarem mais precisas e checáveis.

Uma regra tem por objetivo ser simples, ao invés de cuidadosamente escrita de forma a mencionar todas alternativas e casos especiais.
Tais informações podem ser encontradas nos parágrafos de **Alternativa** e nas seções de [Discussão](#S-discussion).
Se você não entender uma regra ou de discordar dela, favor visitar sua **Discussão**.
Se achar que uma discussão está faltando ou está incompleta, crie uma [Issue](https://github.com/isocpp/CppCoreGuidelines/issues) (repo original em inglês) explicando suas preocupações e possivelmente um PR (Pull-Request) associado.

Isso não é um manual da linguagem.
Essas diretrizes tem por objetivo serem úteis, não serem completas, totalmente precisas em detalhes técnicos ou um guia para código existente.
Fontes de informação recomendadas podem ser encontradas nas [referências](#S-references).

## <a name="SS-sec"></a>In.sec: Principais seções

* [In: Introdução](#S-introduction)
* [P: Filosofia](#S-philosophy)
* [I: Interfaces](#S-interfaces)
* [F: Funções](#S-functions)
* [C: Classes e hierarquias de classes](#S-class)
* [Enum: Enumerações](#S-enum)
* [R: Gerência de recursos](#S-resource)
* [ES: Expressões e declarações](#S-expr)
* [Per: Performance](#S-performance)
* [CP: Concorrência e paralelismo](#S-concurrency)
* [E: Tratamento de erros](#S-errors)
* [Con: Constantes e imutabilidade](#S-const)
* [T: Templates e programação genérica](#S-templates)
* [CPL: Programação estilo C](#S-cpl)
* [SF: Arquivos fonte](#S-source)
* [SL: A Biblioteca Padrão](#S-stdlib)

Seções de suporte:

* [A: Idéias arquiteturais](#S-A)
* [NR: Não-Regras e mitos](#S-not)
* [RF: Referências](#S-references)
* [Pro: Perfis](#S-profile)
* [GSL: Guideline support library](#S-gsl)
* [NL: Nomenclatura e regras de layout](#S-naming)
* [FAQ: Respostas para perguntas frequentes](#S-faq)
* [Apêndice A: Bibliotecas](#S-libraries)
* [Apêndice B: Modernizando código](#S-modernizing)
* [Apêndice C: Discussão](#S-discussion)
* [Apêndice D: Ferramentas de suporte](#S-tools)
* [Glossário](#S-glossary)
* [Pendências: Proto-regras não-classificadas](#S-unclassified)

Essas seções não são ortogonais.

Cada seção (por exemplo, "P" -> "Filosofia") e cada subseção (exemplo: "C.hier" para hierarquias de classes) tem uma abreviação para facilitar buscas e referência.
Abreviações das seções principais também são utilizadas nos números das regras (exemplo: "C.11" para "Torne regular tipos concretos").
