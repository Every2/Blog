---
title: Como escrever C++ sem dar tiro no pé (ou arrancar sua perna)
date: 2024-08-03
---

Recentemente no twitter, me deparei com uma discussão sobre como usar C++ sem dar tiro no pé. Eu acho que lá me expressei mal e esse é um assunto complicado que envolve muito a opinião pessoal e contexto. Então vou dar minha opinião e listar alguns pontos onde eu acho que cada abordagem encaixa e listar os porquês discordo de algumas.


## Tenha um bom tooling


Esse ponto não tem nada a ver, mas eu sempre gosto de falar. Tenha um bom processo pro seu código de C++, independente se vai ser moderno ou não. Por exemplo, use sanitizer, uma ferramenta pra checagem de memory leak (valgrind por exemplo), debugguer e flags do seu compilador (gcc/g++ tem flag pra ativar bound check), build tools, package manager e tenha em mente, você é humano e você erra, não tente ser mais esperto que essas ferramentas, mas não confie 100% nelas o tempo todo. Sempre revise, observe, melhore seus processos e testes ferramentas diferentes. :)


[Exemplo: Nenhuma ferramenta pegando o objeto.c_str() saindo do escopo](https://x.com/kobi_ca/status/1803850295270871281)


Recomendações:


- Cmake/Meson
- Conan
- SE TIVER NO WINDOWS, POR FAVOR, USE O VISUAL STUDIO (ROXO) E EVITE O MINGW




# Evite usar C++ parecendo C


Absolutamente, evite isso. Por quẽ? Bom, acho que não preciso de motivo melhor. C++ não é C, o objetivo da linguagem mudou a quase uma década e quanto mais tempo você ficar dependendo da "retrocompatibilidade" com C mais é capaz do seu código ser deprecado pela ISO misteriosamente e seu código quebrar, e o que vem built in na linguagem melhorar, por exemplo, std::find ter um algoritmo bosta e ter sido arrumado no C++17. Tirando as trocentas implicações de segurança que as coisa de C tem, não que C++ seja santo, mas tem melhoras perante a isso. Então evite.


## Evita, mas pode usar se necessário.


Então... esse aqui é um caso um pouco nebuloso. Digamos que:
Você está interagindo com uma API de C. De fato, pode usar C++ moderno, mas por N motivos, eu recomendo que você use um misto. Use as abstrações de C++ quando convém, ou seja, tá passando uma c-string de uint8? Ótimo, use isso. Tem algum lugar que dá pra usar destructors para garantir que o RAII ajude? Ótimo, cria um objeto e passa no lugar dos ponteiros. Em vez de malloc e free, usa new e delete. Isso vai evitar muita dor de cabeça que pode rolar. C++ te dá essa flexibilidade, mas ainda sim, usaria só de última opção e nesse caso específico.


## Evite usar diretamente raw pointers (sem RAII)


Usar ponteiros pode te deixar maluco e ser uma dor de cabeça e introduzir um monte de problemas que todo mundo já sabe, não preciso nem falar, né? Enfim, C++ te dá ótimas abstrações pra isso. Não vou falar de smart pointers porque acho que em C++ eles têm um papel bem específico e vou explicar mais pra frente e um monte de gente confunde as coisas. O que na verdade é pra você usar no lugar de ponteiros em C++ são referências, use e abuse delas. Sua vida vai ser melhor e mais segura, mas lembre-se, pra usar referências o objeto tem que existir anteriormente. Epa!!!! Referências não servem pro meu caso, preciso de ponteiros!!! Tudo bem, amigo... crie uma classe com seus ponteiros e crie um destrutor para eles, assim você terá uma vida tranquila e com um problema a menos pra esquentar a cabeça. Em ordem do que você deveria escolher:


- Referências
- Smart Pointers
- Criar um objeto (usar RAII)


## Evite template para coisas complexas


Templates são muito legais, mas é muito fácil fazer algo que só você entende. Do mesmo jeito que existe Zé Macro, existe o Zé template e ambos adoram fazer coisas que só ele lembra como funciona. Use os templates apenas para criar tipos muito complexos, abstrair tipos complexos ou dar um ar genérico pro seu código. Se for algo complexo, aposte em especialização ou em técnicas mais simples que não são as melhores, mas estão longe de serem as piores.




## Use uma versão LTS de C++


Sempre mantenha a versão do C++ no seu computador na LTS. Assim, você evita futuros bugs ou alguma coisa não funcionando porque não implementaram na versão nova. Atualmente a versão LTS é a 17. Só use uma versão mais nova, se você quer algo muito específico e tem certeza que não vai dar problema.


## Use containers da STL


Boa parte dos containers da STL tem boas implementações e são tranquilos de usar e mesmo os que têm alocação dinâmica tem gerenciamento de memória automático. Manipular objetos fica bem mais fácil com eles, precisa de uma stack que a já implementada não se encaixa? Tudo bem, você pode usar o std::vector e ter uma abstração segura e fácil de manipular com vários métodos já implementados pra você/você pode dar overload. Eu diria que a exceção seria std::map e std::tuple, mas são facilmente substituíveis por uma struct. Boa parte dos containers são cache friendly e podem dar um boost de performance no seu código.


Um caso especial, recomendo sempre o uso de std::array em vez de c-style array. Se precisar de algo genérico pode usar template, se isso for um impeditivo.




## Use smart pointers em casos específicos


Uma coisa que eu vejo muita gente comendo bola em C++ é que Smart pointers não são a abstração direta para ponteiros. O que na verdade, foi projetado para lidar com isso foi RAII. Smart pointers foram concebidos no Boost e adicionados no C++11, e tem usos bem específicos. Uma coisa que comentei ali em cima, smart pointers não necessariamente são abstrações feitas para "substituir" ponteiros e sim alocação dinâmica de forma unsafe.


Unique_ptr: Só use se você tem um caso claro para ownership. No caso, se você precisa usar std::move. Fora isso, em 99% dos casos uma referência ou RAII vai resolver seus problemas.


Veja em: [CPPCON BACK TO BASICS: MOVE SEMANTICS](https://www.youtube.com/watch?v=knEaMpytRMA)


Shared_ptr: Vocẽ tem múltiplas referências e vale o custo de usar reference counting (cópia). Lembrando, não é thread safe, confira operações atômicas pra isso.


## Use sempre constexpr em vez de macros


Não existe um mundo onde você queira usar macros em vez de constexpr. Constexpr são mais seguras, menos passíveis de erros e mais legíveis que macros. Use elas sempre e nunca macros.


## Não use múltipla herança


É uma feature da linguagem, mas o código fica extremamente complexo e é muito fácil de se perder no que tá fazendo. Sempre use composição caso o seu objeto tenha um relacionamento (tem um) ou herança (é um). Não mais que isso.


## Tenha um bom guia de boas práticas de C++


Bom, se você tem problemas em definir design do seu código em C++ e acho que ser consistente nisso ajuda bastante a reduzir a complexidade. Vou recomendar alguns que eu acho bons:


[Google](https://google.github.io/styleguide/cppguide.html#Ownership_and_Smart_Pointers)


Eu tinha um monte de problemas com ele, mas vejo que eles melhoraram e seguem um padrão "bom" de C++ moderno, não o melhor, mas bom.


[ISO](https://isocpp.org/wiki/faq/coding-standards)


Nenhum desses caras são santos, mas não posso negar que os standards deles são bons.


Você pode forçar esses padrões no seu código com clang-tidy, clang-format, cpplint e etc...




## Conclusão


Essas são minhas opiniões sobre como fazer o uso de C++ agradável e menos complexo, cobrir coisas básicas, mas acho que pra maioria dos softwares isso cobre as principais coisas que eles vão usar.
