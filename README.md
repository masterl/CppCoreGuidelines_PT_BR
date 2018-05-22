# Diretrizes Centrais para C++

***Tradução em português do Brasil***

Repositório original (**em inglês**): [C++ Core Guidelines](http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines)

>"Within C++ is a smaller, simpler, safer language struggling to get out."
>-- <cite>Bjarne Stroustrup</cite>

Tradução: *Dentro do C++ existe uma linguagem menor, mais simples e mais segura lutando para sair.*

As [Diretrizes Centrais para C++](CppCoreGuidelines_pt_BR.md) são um esforço colaborativo encabeçado por Bjarne Stroustrup, assim como a linguagem C++. São o resultado de muitos anos de discussão e projetos através de diversas organizações. O projeto dessas diretrizes encoraja aplicabilidade de forma genérica e ampla adoção, mas podem ser copiadas e alteradas para se adequarem às necessidades de sua organização.

## Começando

As diretrizes podem ser acessadas em [Diretrizes Centrais para C++](CppCoreGuidelines_pt_BR.md).
O documento é escrito em [GitHub-flavored MarkDown](https://github.github.com/gfm/).

- Existe também uma versão para browser (**em inglês**) (http://isocpp.github.io/CppCoreGuidelines/CppCoreGuidelines) que é manualmente atualizada então ***normalmente está um pouco atrasada em relação ao texto em MarkDown***

Boa parte das diretrizes fazem uso da biblioteca *header-only* **Guideline Support Library**. Uma implementação dessa biblioteca está disponível em [GSL: Guideline Support Library](https://github.com/Microsoft/GSL).

## Experiência e escopo

O objetivo das diretrizes é auxiliar as pessoas a utilizarem C++ moderno de forma efetiva. Por "C++ moderno" nos referimos a C++11, C++14 e C++17. Em outras palavras, como você gostaria que seu código fosse visto em 5 anos, dado que você pode iniciar agora? Em 10 anos?

As diretrizes focam em problemas de alto-nível, tais como interfaces, gerência de recursos, gerência de memória e concorrência. Tais regras afetam arquitetura da aplicação e projeto de bibliotecas. Seguir as diretrizes irá proporcionar código com segurança estática de tipos, sem vazamento de memória, além de pegar muitos dos erros de lógica que são comums hoje em dia e irá rodar rápido -- você pode fazer as coisas da forma correta.

Estamos menos preocupados com problemas de baixo-nível, tais como convenções de nome e estilo de indentação, entretanto nenhum tópico que possa auxiliar um programador está fora dos limites.

Nosso conjunto inicial de regras enfatizam segurança (de diversas formas) e simplicidade. Elas podem até ser muito estritas. Esperamos ter que introduzir exceções para melhor acomodar necessidades do mundo real. Também precisamos de mais regras.

Você irá considerar algumas das regras contrárias às suas expectativas ou mesmo contrárias à sua experiência. Se não tivéssemos sugerido que você mudasse seu estilo de codificação de alguma forma, então falhamos! Por favor teste para verificar ou negar as regras! Adoraríamos ter algumas de nossas regras melhor embasadas por medidas ou melhores exemplos.

Você irá achar algumas das regras muito óbvias ou mesmo triviais. Tente lembrar que um dos propósitos de uma diretriz é ajudar alguém que tenha pouca experiência ou que venha de um ambiente ou linguagem diferente a se nivelarem.

As diretrizes são projetadas para terem suporte de uma ferramenta de análise. Violações das regras serão indicadas com referências (ou links) para a regra em questão.
Não esperamos que você memorize todas as regras antes de começar a escrever código.

As diretrizes se destinam a serem introduzidas de forma gradual à uma base de código. Planejamos construir ferramentas para isso e esperamos que outros façam o mesmo.

## Contribuições e LICENÇA

Comentários e sugestões para melhorias são bem-vindas. Planejamos modificar e extender este documento conforme nosso entendimento melhorar e a linguagem e o conjunto de bibliotecas disponíveis melhorem também.

***Falta criar um documento para guiar contribuições com esta tradução***

Licença: [LICENSE](./LICENSE).

Para contribuir diretamente no repositório original, ver (***em inglês***) [CONTRIBUTING](https://github.com/isocpp/CppCoreGuidelines/blob/master/CONTRIBUTING.md).
