# <a name="main"></a>Diretrizes Centrais para C++

#### Tradução de um snapshot de 02 de Maio de 2019

***!!! tradução em progresso !!!***

Editores:

* [Bjarne Stroustrup](http://www.stroustrup.com)
* [Herb Sutter](http://herbsutter.com/)

Este é um documento vivo que sofre melhorias constantemente.
Se fosse um projeto open source (código), esta seria a versão 0.8.
Cópia, utilização, modificação e criação de trabalhos derivados deste projeto está licenciada sob uma licença estilo MIT.
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
* [E: Tratamento de erros](#S-errors)
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
* [GSL: Guidelines support library](#S-gsl)
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
[move](#Rc-move-semantics) --
[outras operações](#Rc-matched) --
[default](#Rc-eqdefault)
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
* destrutor:
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
Por "C++ moderno" queremos dizer o uso efetivo do padrão ISO do C++ (atualmente, C++17, mas quase todas nossas recomendações também se aplicam a C++14 e C++11).
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

Este é um conjunto de diretrizes para C++ moderno (atualmente C++17), absorvendo melhorias futuras e levando em consideração Especificações Técnicas ISO (TSs) (*ISO Technical Specifications*).
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

A proposta deste documento é auxiliar desenvolvedores a adotar C++ moderno (atualmente C++17) e para atingirmos um estilo mais uniforme através das bases de código.

Nós não temos a ilusão de que cada uma dessas regras possam ser efetivamente aplicadas a todas as bases de código. Atualizar sistemas antigos é difícil. Entretanto, acreditamos que um programa que utilize uma regra é menos propenso a erros e mais manutenível que um que não utilize. Frequentemente, as regras levam a um desenvolvimento inicial mais rápido/fácil.
Até onde sabemos, essas regras levam a código que rodam tão bem ou melhor que técnicas antigas ou mais convencionais; são regras que seguem o princípio de zero custo adicional (*zero-overhead*) (*"O que vc não utiliza, você não irá pagar por isso"* ou *"quando você utiliza um mecanismo de abstração de forma correta, você obtém performance pelo menos tão boa quanto se você tivesse escrito o código a mão utilizando construções de baixo-nível da linguagem"*).
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
As diretrizes destinadas a prevenir acidentes frequentemente banem trechos de código C++ perfeitamente legais.
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

As regras não são precisas a um ponto tal que uma pessoa (ou máquina) possa seguí-las às cegas.
As partes sobre execução das regras tentam ser isso, mas preferimos deixar uma regra ou definição um pouco vagas e abertas à interpretação do que especificar algo de forma precisa e errada.
Algumas vezes, precisão vem somente com o tempo e experiência.
Design (ainda) não é uma forma de Matemática.

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

onde "tag" é nome âncora do item onde a regra de Imposição aparece.

Exemplos:

- para [C.134](#Rh-public) é "Rh-public")
- o nome de um perfil de grupo de regras ("type", "bounds" ou "lifetime")
- uma regra específica em um perfil ([type.4](#Pro-type-cstylecast), ou [bounds.2](#Pro-bounds-arrayindex)

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

# <a name="S-philosophy"></a>P: Filosofia

As regras nessa seção são bem genéricas.

Lista de regras filosóficas:

* [P.1: Expresse idéias diretamente no código](#Rp-direct)
* [P.2: Programe em C++ padrão (ISO)](#Rp-Cplusplus)
* [P.3: Expresse intenção](#Rp-what)
* [P.4: Idealmente, um programa deve ser staticamente *type safe*](#Rp-typesafe)
* [P.5: Prefira checagem em tempo de compilação em detrimento das em tempo de execução](#Rp-compile-time)
* [P.6: O que não pode ser checado em tempo de compilação deve ser checável em tempo de execução](#Rp-run-time)
* [P.7: Capture erros em tempo de execução cedo](#Rp-early)
* [P.8: Não vaze recursos](#Rp-leak)
* [P.9: Não desperdice tempo ou espaço](#Rp-waste)
* [P.10: Prefira dados imutáveis](#Rp-mutable)
* [P.11: Encapsule conceitos complicados, ao invés de espalhar pelo código](#Rp-library)
* [P.12: Use ferramentas de suporte quando apropriado](#Rp-tools)
* [P.13: Use bibliotecas de suporte quando apropriado](#Rp-lib)

Regras filosóficas em geral não são mecanicamente verificáveis.
Entretanto, regras individuais refletindo esses temas filosóficos são.
Sem uma base filosófica, faltaria embasamento às regras mais concretas/específicas/checáveis.

### <a name="Rp-direct"></a>P.1: Expresse idéias diretamente no código

##### Razão

Compiladores não lêem comentários (ou documentos de design) e nem muitos programadores (no geral).
O que é expresso em código tem semântica definida e pode (a princípio) ser checado por compiladores e outras ferramentas.

##### Exemplo

```cpp
class Date {
    // ...
public:
    Month month() const;  // faça
    int month();          // não faça
    // ...
};
```

A primeira declaração do método `month` é explicita quanto a retornar um Mês (`Month`) e sobre não modificar o estado de um objeto `Date`.
A segunda versão deixa o leitor tentar adivinhar como o método funciona e abre mais possibilidades em relação a bugs não-notados.

##### Exemplo; ruim

Esse loop é uma forma restrita de `std::find`:

```cpp
void f(vector<string>& v)
{
    string val;
    cin >> val;
    // ...
    int index = -1;                    // ruim, e poderia utilizar gsl::index
    for (int i = 0; i < v.size(); ++i) {
        if (v[i] == val) {
            index = i;
            break;
        }
    }
    // ...
}
```

##### Exemplo; bom

Uma forma muito mais clara de expressar intenção seria:

```cpp
void f(vector<string>& v)
{
    string val;
    cin >> val;
    // ...
    auto p = find(begin(v), end(v), val);  // melhor
    // ...
}
```

Uma biblioteca bem projetada expressa intenção (o que deve ser feito, ao invés de apenas como algo está sendo feito) muito melhor que o uso direto de recursos da linguagem.

Um programador C++ deve saber o básico da biblioteca padrão e usá-la quando apropriado.
Qualquer programador deve saber o básico das bibliotecas do projeto no qual trabalha e usá-las apropriadamente.
Qualquer programador utilizando essas diretrizes devem conhecer a [biblioteca de suporte das diretrizes (GSL)](#S-gsl) e usá-la apropriadamente.

##### Exemplo

```cpp
change_speed(double s);   // ruim: o que s significa?
// ...
change_speed(2.3);
```

Uma abordagem melhor é ser explícito quanto ao significado do parâmetro `double` (é o novo valor da velocidade ou uma diferença em relação ao valor antigo?) e a unidade utilizada:

```cpp
change_speed(Speed s);    // melhor: o significado de s é explícito
// ...
change_speed(2.3);        // erro: sem unidade
change_speed(23m / 10s);  // metros por segundo
```

Até poderíamos aceitar um simples `double` sem unidade como um delta, mas seria propenso a erros.
Se quiséssemos ambos velocidade absoluta e deltas, poderíamos ter definido um tipo `Delta`.

##### Imposição

Bem difícil no geral.

* utilizar `const` de forma consistente (cheque se os métodos alteram seu objeto; cheque se as funções modificam argumentos passados por ponteiro ou referência)
* sinalizar o uso de casts (casts neutralizam o sistema de tipos)
* detectar código que imite a biblioteca padrão (difícil)

### <a name="Rp-Cplusplus"></a>P.2: Programe em C++ padrão (ISO)

##### Razão

Esse é um conjunto de diretrizes para escrever C++ padrão ISO.

##### Nota

Existem ambientes onde extensões são necessárias, por exemplo, acessar recursos do sistema.
Em tais casos, localize o uso das extensões necessárias e controle seu uso com diretrizes de programação "não-core" (ou seja, diferente das apresentadas aqui). Se possível, construa interfaces que encapsulem as extensões de forma que estas possam ser desligadas ou removidas da compilação em sistemas que não suportem essas extensões.

Extensões costumam não ter semânticas rigorosamente definidas. Até extensões que são comuns e implementadas por diversos compiladores podem ter comportamentos levemente diferentes e casos de contorno como um resultado direto de *não* ter um padrão rigorosamente definido. Quanto maior for o uso de tais extensões, mais a portabilidade do código sofrerá impactos.

##### Nota

Utilizar C++ padrão ISO válido não garante portabilidade (quanto menos corretude).
Evite depender de comportamento indefinido (exemplo: [ordem de avaliação indefinida](#Res-order)) e esteja alerta quanto à construções cujo significado dependa de implementação (exemplo: `sizeof(int)`).

##### Nota

Existem ambientes em que as restrições ao uso da linguagem C++ ou de recursos de biblioteca são necessários, por exemplo:
evitar alocação dinâmica, algo que é requerido por padrões de software de controle aéreo.
Em tais casos, controle seu (des)uso com uma extensão *destas diretrizes de codificação* adaptadas a este ambiente específico.

##### Imposição

Utilize um compilador C++ atualizado (atualmente C++17, C++14 ou C++11) com um conjunto de opções que não aceite extensões.

### <a name="Rp-what"></a>P.3: Expresse intenção

##### Razão

A menos que a intenção de um determinado trecho de código esteja explícita (por exemplo utilizando nomes legíveis ou comentários), é impossível dizer se o código está fazendo o que deveria fazer.

##### Exemplo

```cpp

gsl::index i = 0;
while (i < v.size()) {
    // ... faz algo com v[i] ...
}

```

A intenção de "apenas" iterar pelos elementos de `v` não está expressada aqui. O detalhe de implementação de um índice está exposto (então pode ser utilizado incorretamente) e `i` continua existindo fora do escopo do laço, o que pode ou não ser intencional. O leitor não consegue saber somente com esse trecho de código.

Melhor:

```cpp

for (const auto& x : v) { /* faz algo com o valor de x */ }

```

Agora não existe menção explícita ao mecanismo de iteração e o laço opera com referências contantes a cada elemento de forma que uma alteração acidental não possa ocorrer. Caso se deseje modificar, basta fazer:

```cpp

for (auto& x : v) { /* modifica x */ }

```

Para mais detalhes sobre laços for, veja [ES.71](#Res-for-range).

Melhor ainda, utilize um algoritmo nomeado. Esse exemplo utiliza o `for_each` da *Ranges TS* porque ele expressa diretamente a intenção:

```cpp

for_each(v, [](int x) { /* faz algo com o valor de x */ });
for_each(par, v, [](int x) { /* faz algo com o valor de x */ });

```

A última opção deixa claro que não estamos interessados na ordem em que os elementos de `v` são manipulados.

Um programador deve se familiarizar com

* [A biblioteca de suporte às diretrizes](#S-gsl)
* [A biblioteca padrão ISO C++](#S-stdlib)
* Quaisquer bibliotecas base utilizadas no projeto atual

##### Nota

Descrição alternativa: Diga *o que* deve ser feito e não apenas *como* deve ser feito.

##### Nota

Algumas construções da linguagem expressam intenção melhor que outras.

##### Exemplo

Se dois `int`s têm por objetivo serem coordenadas de um ponto 2D, por exemplo:

```cpp

draw_line(int, int, int, int);  // obscuro
draw_line(Point, Point);        // mais claro

```

##### Imposição

Procure por padrões comuns para os quais existem alternativas melhores

* laços `for` simples vs `range-for`
* interfaces `f(T*, int)` vs `f(span<T>)`
* variáveis de controle de laço em escopos muito grandes
* `new` e `delete` nus
* funções com muitos parâmetros de tipos básicos

Existem muitas oportunidades para esperteza e transformação semi-automática do programa.

### <a name="Rp-typesafe"></a>P.4: Idealmente, um programa deve ser staticamente *type safe*

##### Razão

Idealmente, um programa deve ser completamente e estaticamente (em tempo de compilação) *type safe*.
Infelizmente, isso não é possível. Áreas problemáticas:

* `union`s
* casts (conversões de tipo)
* decaimento de array
* erros de coleção (*range*) (por exemplo, acessar posição fora de um array)
* conversões truncantes

##### Nota

Essas áreas são fonte se problemas sérios. Por exemplo, encerramento inesperado do programa e violações de segurança.
Nós tentamos prover técnicas alternativas.

##### Imposição

Podemos banir, restringir or detectar cada categoria problemática separadamente, tal qual seja requerido ou factível em determinados programas.
Sempre sugira uma alternativa.
Por exemplo:

* `union`s -- use `variant` (introduzido no C++17)
* casts -- minimize seu uso; templates podem ajudar
* decaimento de array -- utilize `span` (da GSL)
* erros de coleção (*range*) -- utilize `span`
* conversões truncantes -- minimize seu uso ou utilize `narrow` ou `narrow_cast` (da GSL) onde forem necessárias

### <a name="Rp-compile-time"></a>P.5: Prefira checagem em tempo de compilação em detrimento das em tempo de execução

##### Razão

Clareza no código e performance.
Você não precisa tratar erros que possam ser pegos em tempo de compilação.

##### Exemplo

```cpp
int bits = 0;               // não faça: código evitável
// Nota: Int é um apelido usado para inteiros
for (Int i = 1; i; i <<= 1)
    ++bits;
if (bits < 32)
    cerr << "Int muito pequeno\n";
```

Esse exemplo falha no que ele está tentando fazer (devido a overflow ser indefinido) e deveria ser trocado por um simples `static_assert`:

```cpp
// Int é um apelido usado para inteiros
static_assert(sizeof(Int) >= 4);    // faça: checagem em tempo de compilação
```

Ou melhor ainda: simplesmente utilize o sistema de tipos e troque `int` por `int32_t`, por exemplo.

##### Exemplo

```cpp
void read(int* p, int n);   // ler no máximo n inteiros e armazenar em p

int a[100];
read(a, 1000);    // ruim, extrapola o tamanho do array
```

melhor:

```cpp
void read(span<int> r); // ler para a coleção de inteiros r

int a[100];
read(a);        // melhor: deixe o compilador definir a quantidade de elementos
```

**Justificativa**: Não deixe para tempo de execução o que pode ser feito bem em tempo de compilação

##### Imposição

* Procure por argumentos do tipo ponteiro
* Procure por checagens em tempo de execução de violações de coleção (*range*)

### <a name="Rp-run-time"></a>P.6: O que não pode ser checado em tempo de compilação deve ser checável em tempo de execução

##### Razão

Deixar erros difíceis de detectar em um programa é pedir por interrupção prematura e resultados ruins.

##### Nota

Idealmente, nós capturamos todos os erros (que não sejam erros de lógica) ou em tempo de compilação ou em tempo de execução. É impossível capturar todos os erros em tempo de compilação e normalmente não é barato capturar os erros remanescentes em tempo de execução. Entretanto, devemos nos esforçar em escrever programas que a princípio possam ser checados, dados recursos suficientes (ferramentas de análise, checagens em tempo de execução, recursos da máquina, tempo, ...).

##### Exemplo, ruim

```cpp
// compilado separadamente, possivelmente carregado dinamicamente
extern void f(int* p);

void g(int n)
{
    // ruim: quantidade de elementos não é passada para f()
    f(new int[n]);
}
```

Aqui uma parte crucial da informação (o número de elementos) foi tão "ocultada" que análise estática provavelmente se tornou impossível de ser feita e checagem dinâmica pode ser muito difícil quando `f()` é parte de uma `ABI` (*Application Binary Interface*) que não nos permite "instrumentar" esse ponteiro. Nós poderíamos anexar informação útil à `free store`, mas isso iria requerer mudanças globais em um sistema e talvez no compilador. O que temos aqui é um design que torna detecção de erro muito difícil.

##### Exemplo, ruim

Claro, poderíamos passar o número de elementos junto ao ponteiro:

```cpp
// compilado separadamente, possivelmente carregado dinamicamente
extern void f2(int* p, int n);

void g2(int n)
{
    f2(new int[n], m);  // ruim: uma quantidade errada de elementos pode ser passada a f()
}
```

Passar o número de elementos como argumento é melhor (e bem mais comum) que passar somente o ponteiro e depender de alguma convenção implícita em relação à quantidade de elementos ou como descobrir tal informação. Entretanto, como demonstrado, um simples erro de digitação pode introduzir um erro sério. A conexão entre os dois argumentos de `f2()` é convencional e não explícita.

Outra coisa, será que também está implícito que `f2()` deve fazer o `delete` desse ponteiro ou será que quem chamou a função cometeu um segundo erro?

##### Exemplo, ruim

Os ponteiros para gerência de recurso da biblioteca padrão falham em passar o tamanho quando eles apontam para um objeto:

```cpp
// compilado separadamente, possivelmente carregado dinamicamente
// NB: assume-se que o código executor seja ABI-compatible, utilizando
// um compilador C++ compatível e a mesma implementação da stdlib
extern void f3(unique_ptr<int[]>, int n);

void g3(int n)
{
    f3(make_unique<int[]>(n), m);    // ruim: passa posse e tamanho separadamente
}
```

##### Exemplo

Precisamos passar o ponteiro e a quantidade de elementos como um objeto só:

```cpp
extern void f4(vector<int>&);   // compilado separadamente, possivelmente carregado dinamicamente
extern void f4(span<int>);      // compilado separadamente, possivelmente carregado dinamicamente
                                // NB: assume-se que o código executor seja ABI-compatible, utilizando
                                // um compilador C++ compatível e a mesma implementação da stdlib

void g3(int n)
{
    vector<int> v(n);
    f4(v);                     // passa uma referência, mantém posse
    f4(span<int>{v});          // passa uma visualização, mantém posse
}
```

Esse design carrega a quantidade de elementos junto como parte de um objeto, assim erros são improváveis e checagem dinâmica (em tempo de execução) são sempre possíveis de serem feitas, ainda que nem sempre se possa pagar pelos custos.

##### Exemplo

Como podemos transferir tanto posse quanto todas as informações para validar o uso?

```cpp
vector<int> f5(int n)    // OK: move
{
    vector<int> v(n);
    // ... inicializa v ...
    return v;
}

unique_ptr<int[]> f6(int n)    // ruim: perde o 'n'
{
    auto p = make_unique<int[]>(n);
    // ... inicializa *p ...
    return p;
}

owner<int*> f7(int n)    // ruim: perde o 'n' e podemos esquecer de executar o `delete`
{
    owner<int*> p = new int[n];
    // ... inicializa *p ...
    return p;
}
```


##### Exemplo

* ???
* mostrar como checagens possíveis são evitadas por interfaces que trocam classes base polimórficas, quando elas sabem do que precisam?
  Ou strings como opções "free-style"

##### Imposição

* Sinalize interfaces do tipo (ponteiro, contador) (isso irá sinalizar vários exemplos que não podem ser corrigidos por questões de compatibilidade)
* ???

### <a name="Rp-early"></a>P.7: Capture erros em tempo de execução cedo

##### Razão

Evite quebras "misteriosas".
Evite erros que levem a resultados errados (potencialmente não percebidos).

##### Exemplo

```cpp
void increment1(int* p, int n)    // ruim: propenso a erro
{
    for (int i = 0; i < n; ++i) ++p[i];
}

void use1(int m)
{
    const int n = 10;
    int a[n] = {};
    // ...
    increment1(a, m);   // talvez erro de digitação, talvez assume-se m <= n
                        // mas imagine que m == 20
    // ...
}
```

Aqui cometemos um pequeno erro em `use1` que irá implicar em corrupção de dados ou quebra do programa.
O estilo de interface (ponteiro, tamanho) deixa `increment1` sem nenhuma forma real de se defender de erros de acesso fora da coleção (*range*).
Se pudéssemos checar os índices para detectar um acesso fora da coleção (*range*), então o erro não seria descoberto até que acessássemos `p[10]`.
Poderíamos checar antecipadamente e melhorar o código:

```cpp
void increment2(span<int> p)
{
    for (int& x : p) ++x;
}

void use2(int m)
{
    const int n = 10;
    int a[n] = {};
    // ...
    increment2({a, m});    // talvez erro de digitação, talvez assume-se m <= n
    // ...
}
```

Agora `m <= n` pode ser checado no ponto de chamada (cedo) e não após algum potencial uso errado.
Caso seja somente um erro de digitação e quiséssemos utilizar `n` como limite, o código pode ser simplificado ainda mais (eliminando a possibilidade de erros):

```cpp
void use3(int m)
{
    const int n = 10;
    int a[n] = {};
    // ...
    increment2(a);   // não é necessário repetir a quantidade de elementos de 'a'
    // ...
}
```

##### Exemplo, ruim

Não cheque o mesmo valor repetidas vezes. Não passe dados estruturados como strings:

```cpp
Date read_date(istream& is);    // lê data por meio da istream

Date extract_date(const string& s);    // extrai data da string

void user1(const string& date)    // manipula data
{
    auto d = extract_date(date);
    // ...
}

void user2()
{
    Date d = read_date(cin);
    // ...
    user1(d.to_string());
    // ...
}
```

A data é validada duas vezes (pelo construtor de `Date`) e passada como uma string (dados não-estruturados).

##### Exemplo

Checagens excessivas podem custar muito.
Há casos onde checagem prematura é sem sentido porque talvez vc nunca precise do valor, ou talvez só precise de parte do valor que é mais facilmente checável que o valor inteiro. Similarmente, não adicione checagens de validade que alterem o comportamento assintótico de sua interface (exemplo: não adicione uma checagem `O(n)` para uma interface com complexidade média de `O(1)`).

```cpp
class Jet {    // Física diz: e * e < x * x + y * y + z * z
    float x;
    float y;
    float z;
    float e;
public:
    Jet(float x, float y, float z, float e)
        :x(x), y(y), z(z), e(e)
    {
        // Devo checar aqui se os valores têm significado físico?
    }

    float m() const
    {
        // Devo tratar o caso inválido aqui?
        return sqrt(x * x + y * y + z * z - e * e);
    }

    // ???
};
```

A lei física para um jet (`e * e < x * x + y * y + z * z`) não é uma invariante devido a possibilidade de erros de medida.

???

##### Imposição

* Procure por ponteiros e arrays: Cheque o tamanho cedo e não repetidamente
* Procure por conversões: Elimine ou marque conversões truncantes
* Procure por valores não checados que venham de alguma entrada
* Procure por dados estruturados (objetos de classes com invariantes) sendo convertidos em strings
* ???

### <a name="Rp-leak"></a>P.8: Não vaze recursos

##### Razão

Mesmo um crescimento devagar em uso recursos irá (com o tempo) exaurir a disponibilidade desses recursos.
Isso é particularmente importante para programas que rodem por um longo tempo, mas é uma parte essencial de um comportamento de programação responsável.

##### Exemplo, ruim

```cpp
void f(char* name)
{
    FILE* input = fopen(name, "r");
    // ...
    if (something) return;   // ruim: se something == true, um descritor de arquivo é vazado
    // ...
    fclose(input);
}
```

Prefira [RAII](#Rr-raii):

```cpp
void f(char* name)
{
    ifstream input {name};
    // ...
    if (something) return;   // OK: sem vazamento
    // ...
}
```

**Veja também**: [A seção de gerência de recursos](#S-resource)

##### Nota

Um vazamento é coloquialmente chamado "qualquer coisa que não é limpa".
A classificação mais importante é "qualquer coisa que não pode ser mais limpa".
Por exemplo, alocar um objeto na heap e depois perder o último ponteiro que apontava para aquela alocação.
Essa regra não deve ser considerada como requerer que alocações em objetos de longa vida devem ser retornadas ao final do programa.
Por exemplo, depender na limpeza garantida pelo sistema, como fechar arquivos e desalocação de memória ao final do processo podem simplificar o código.
Entretanto, depender em abstrações que fazem limpeza implícita podem ser igualmente simples e normalmente mais seguras.

##### Nota

Impor [o perfil de segurança de duração](#SS-lifetime) elimina vazamentos.
Quando combinado com segurança de recursos provida pela [RAII](#Rr-raii), elimina a necessidade de "coleção de lixo" (por não gerar lixo).
Combine isso com a imposição de [perfis de tipo e limites](#SS-force) e você obtém segurança completa de tipos e recursos, garantida por ferramentas.

##### Imposição

* Procure por ponteiros: Classifique-os em não-donos (o padrão) e donos.
  Quando possível, troque os donos por manipuladores de recursos da biblioteca padrão (como no exemplo acima).
  Alternativamente, marque um dono por meio do uso de `owner` da [GSL](#S-gsl).
* Procure por `new` e `delete`
* Procure por funções de alocação de recursos conhecidas que retornem ponteiros simples (tais como `fopen`, `malloc`, e `strdup`)

### <a name="Rp-waste"></a>P.9: Não desperdice tempo ou espaço

##### Razão

Isso é C++.

##### Nota

Tempo e espaço que você gasta bem para atingir um objetivo (exemplo: velocidade de desenvolvimento, segurança de recursos ou simplificação de teste) não é desperdiçado.

"Another benefit of striving for efficiency is that the process forces you to understand the problem in more depth." - Alex Stepanov

Tradução: *Outro benefício de se esforçar por eficiência é que o processo te força a entender o problema mais profundamente.*

##### Exemplo, ruim

```cpp
struct X {
    char ch;
    int i;
    string s;
    char ch2;

    X& operator=(const X& a);
    X(const X&);
};

X waste(const char* p)
{
    if (!p) throw Nullptr_error{};
    int n = strlen(p);
    auto buf = new char[n];
    if (!buf) throw Allocation_error{};
    for (int i = 0; i < n; ++i) buf[i] = p[i];
    // ... manipula o buffer ...
    X x;
    x.ch = 'a';
    x.s = string(n);    // dá a x.s espaço para *p
    for (gsl::index i = 0; i < x.s.size(); ++i) x.s[i] = buf[i];  // copia 'buf' pra x.s
    delete[] buf;
    return x;
}

void driver()
{
    X x = waste("Argumento típico");
    // ...
}
```

Sim, isso é uma caricatura, mas nós vimos cada erro em código de produção e pior.
Note que o layout de `X` garante que pelo menos 6 bytes (e provavelmente mais) são desperdiçados.
A definição enganosa de operações de cópia desabilitam as semânticas de transferência tal que a operação de return se torna lenta (note que a otimização de valor de retorno, RVO, não é garantida aqui).
O uso de `new` e `delete` para `buf` é redundante. Se realmente precisássemos de uma string local, deveríamos utilizar um objeto `string` local.
Tem ainda mais bugs de performance e complicação gratuita.

##### Exemplo, ruim

```cpp
void lower(zstring s)
{
    for (int i = 0; i < strlen(s); ++i) s[i] = tolower(s[i]);
}
```

Sim, esse é um exemplo de código de produção.
Deixamos por conta do leitor descobrir o que está sendo desperdiçado.

##### Nota

Um exemplo individual de desperdício raramente é significativo e quando é significativo, costuma ser facilmente eliminado por um expert.
Entretanto, desperdício espalhado através da base de código pode se tornar significativo e os experts nem sempre estão disponíveis como gostaríamos.
O objetivo desta regra (e das regras mais específicas que a dão suporte) é eliminar a maior parte dos desperdícios relacionados ao uso do C++ antes que eles aconteçam.
Após isso, podemos analizar o desperdício relacionado a algoritmos e requisitos, mas isso está além do escopo dessas diretrizes.

##### Imposição

Várias outras regras mais específicas tem por objetivos simplicidade e eliminação de desperdício gratuito em geral.

* Marque um valor de retorno não utilizado de uma `operator++` ou `operator--` pós-fixo não-default definido pelo usuário. Prefira utilizar a forma pré-fixa. (Nota: "definido pelo usuário" e "não-default" são para reduzir ruído. Revise esta imposição caso ainda produza muito ruído na prática.)

### <a name="Rp-mutable"></a>P.10: Prefira dados imutáveis

##### Razão

É mais fácil raciocinar sobre constantes do que variáveis.
Algo imutável não irá mudar de forma inexperada.
Algumas vezes imutabilidade permite realizar otimizações melhores.
Você não pode ter uma condição de concorrência em uma constante.

Veja [Con: Constantes e imutabilidade](#S-const)

### <a name="Rp-library"></a>P.11: Encapsule conceitos complicados, ao invés de espalhar pelo código

##### Razão

Código bagunçado tem mais chances de esconder bugs e mais difícil de escrever.
Uma boa interface é mais fácil e segura de usar.
Código baixo-nível e bagunçado dá a luz a mais códigos do mesmo tipo.

##### Exemplo

```cpp
int sz = 100;
int* p = (int*) malloc(sizeof(int) * sz);
int count = 0;
// ...
for (;;) {
    // ... ler um inteiro para 'x', sair do laço caso alcance o fim do arquivo ...
    // ... checa se 'x' é válido ...
    if (count == sz)
        p = (int*) realloc(p, sizeof(int) * sz * 2);
    p[count++] = x;
    // ...
}
```

Isso é baixo-nível, verboso e propenso a erro.
Por exemplo, "esquecemos" de testar exaustão da memória.
No lugar, podemos utilizar `vector`:

```cpp
vector<int> v;
v.reserve(100);
// ...
for (int x; cin >> x; ) {
    // ... checa se x é válido ...
    v.push_back(x);
}
```

##### Nota

A biblioteca padrão e a GSL são exemplos dessa filosofia.
Por exemplo, ao invés de interagir com arrays, unions, cast, problemas de tempo de vida, `gsl::owner` e etc., que são necessários para implementar abstrações chave tais como `vector`, `span`, `lock_guard` e `future`, utilizamos bibliotecas desenvolvidas e implementadas por pessoas com mais tempo e conhecimento do que normalmente temos.
Similarmente, nós podemos e devemos desenvolver e implementar bibliotecas especializadas, ao invés de deixar os usuários (com frequência, nós mesmos) com o desafio repetitivo de escrever bem código baixo-nível.
Essa é uma variante do [princípio do subset do superset](#R0) que constitui a base dessas diretrizes.

##### Imposição

* Procure por "código bagunçado" tais como manipulaçao complexa de ponteiros e casting fora da implementação de abstrações.

### <a name="Rp-tools"></a>P.12: Use ferramentas de suporte quando apropriado

##### Razão

Existem muitas coisas que são melhor feitas "por máquina".
Computadores não se cansam ou se entediam com tarefas repetitivas.
Nós normalmente temos coisas melhores a fazer do que realizar tarefas rotineiras repetidamente.

##### Exemplo

Executar um analizador estático para verificar se seu código segue as diretrizes que você quer que ele siga.

##### Nota

Veja

* [Ferramentas de análise estática](???)
* [Ferramentas de concorrência](#Rconc-tools)
* [Ferramentas de teste](???)

Existem diversos outros tipos de ferramentas, tais como repositórios de código, ferramentas de build, etc., mas estes estão fora do escopo dessas diretrizes.

##### Nota

Tome o cuidado de não se tornar dependente de tool chains elaboradas ou ultra-especializadas.
Essas podem tornar seu código não-portável.

### <a name="Rp-lib"></a>P.13: Use bibliotecas de suporte quando apropriado

##### Razão

Utilizar uma biblioteca bem projetada, bem documentada e com amplo suporte economiza tempo e esforço.
Sua qualidade e documentação muito provavelmente serão melhores do que você faria se a maior parte do seu tempo for reservada para uma implementação específica.
O custo (tempo, esforço, dinheiro, etc.) de uma biblioteca pode ser compartilhado entre vários usuários.
Uma biblioteca amplamente utilizada, tem mais chances de ser mantida atualizada e portada para novos sistemas.
Conhecimento de uma biblioteca amplamente utilizada pode economizar tempo em outros projetos e projetos futuros.
Então, se uma biblioteca afim existir para o domínio da sua aplicação, utilize-a.

##### Exemplo

```cpp
std::sort(begin(v), end(v), std::greater<>());
```

A menos que você seja um especialista em algoritmos de ordenação e tenha bastante tempo livre, esse código tem mais chances de ser correto e rodar mais rápido do que qualquer outro que você escreva para uma aplicação específica.
Você precisa de uma razão para não utilizar a biblioteca padrão (ou qualquer outra biblioteca base que sua aplicação utilize) do que uma razão para utilizá-la.

##### Nota

Por padrão utilize:

* A [Biblioteca Padrão ISO C++](#S-stdlib)
* A [Guidelines Support Library](#S-gsl)

##### Nota

Se não existir uma biblioteca bem projetada, bem documentada e amplamente suportada para um domínio importante, talvez você deva projetar e implementá-la e então utilizá-la.

# <a name="S-interfaces"></a>I: Interfaces

Uma interface é um contrato entre duas partes de um programa. Definindo precisamente o que é esperado do provedor de um serviço e de um usuário desse serviço é essencial.
Ter boas interfaces (fáceis de entender, que encorajam uso eficiente, não propensa a erro, que suporte testes, etc.) é provavelmente o aspecto mais importante da organização de código.

Índice das regras de interface:

* [I.1: Crie interfaces explícitas](#Ri-explicit)
* [I.2: Evite variáveis globais não-`const`](#Ri-global)
* [I.3: Evite singletons](#Ri-singleton)
* [I.4: Crie interfaces precisas e fortemente tipadas](#Ri-typed)
* [I.5: Estabeleça pré-condições (se existirem)](#Ri-pre)
* [I.6: Prefira `Expects()` para expressar pré-condições](#Ri-expects)
* [I.7: Estabeleça pós-condições](#Ri-post)
* [I.8: Prefira `Ensures()` para expressar pós-condições](#Ri-ensures)
* [I.9: Se uma interface for um template, documente seus parâmetros utilizando concepts](#Ri-concepts)
* [I.10: Use exceções para sinalizar a falha ao executar uma tarefa](#Ri-except)
* [I.11: Nunca transfira posse por um ponteiro simples (`T*`) ou referência (`T&`)](#Ri-raw)
* [I.12: Declare um ponteiro que não pode ser nulo como `not_null`](#Ri-nullptr)
* [I.13: Não passe um array com um ponteiro simples](#Ri-array)
* [I.22: Evite inicialização complexa de objetos globais](#Ri-global-init)
* [I.23: Mantenha baixa a quantidade de argumentos para uma função](#Ri-nargs)
* [I.24: Evite parâmetros adjacentes do mesmo tipo que não tenham relação entre si](#Ri-unrelated)
* [I.25: Prefira classes abstratas como interfaces para hierarquia de classes](#Ri-abstract)
* [I.26: Se você quer uma ABI cross-compiler, utilize o subset estilo C](#Ri-abi)
* [I.27: Para uma ABI estável de biblioteca, considere o idioma Pimpl](#Ri-pimpl)
* [I.30: Encapsule violações de regra](#Ri-encapsulate)

**Veja também**:

* [F: Funções](#S-functions)
* [C.concrete: Tipos concretos](#SS-concrete)
* [C.hier: Hierarquias de classes](#SS-hier)
* [C.over: Sobrecarga e sobrecarga de operadores](#SS-overload)
* [C.con: Contêiners e outros manipuladores de recursos](#SS-containers)
* [E: Tratamento de erros](#S-errors)
* [T: Templates e programação genérica](#S-templates)
