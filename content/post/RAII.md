---
title: Explicando (tentando) RAII
date: 2024-06-24
---

Bom, hoje venho explicar (tentar) a vocês o que é RAII. Um conceito de gerenciamento de memória muito importante que foi inventado no C++, mas é bem pouco conhecido e também pouco falados, mas que é importante até hoje e também é usado no Rust. (Fica até o final que vai ter uns extras)


Mas antes, uma breve introdução de como C entende sua memória. Pera, mas você não tá falando de C++ e de Rust, por que C? Porque é mais simples de explicar e entender também. Também irei focar bastante na ideia de malloc e delete, apesar de que vou usar new e delete pra ficar mais claro. Então sim, vai ser uma bagunça, mas você vai entender.


![Memoria](/images/Memory-Layout-of-C-1.webp)

Resumindo: 

Data Segment: É onde seu arrays são alocados, sim, seus arrays vão primeiramente aqui. Ele reserva essa parte inicialmente pra ser eficiente de lidar com isso. (Não rola nenhum gerenciamento de memória por parte sua, o compilador faz tudo)

```c
//exemplo de ds
int array[3] = {1, 2, 3};
```

Stack: Onde suas variaveis que não são dinamicamente alocadas vão (você não usou malloc), semelhante ao data segment nada acontece aqui sobre escopo, o compilador faz tudo. Um adendo, isso funciona igual a estrutura de dados com mesmo nome. O último a entrar é o primeiro a sair. :)

```c
//exemplo de stack (pensando em desenhar)
int n = 10;
char c = 'c';
```

Até aqui tudo bem, certo? O compilador faz tudo, por que eu preciso de RAII? É muito fácil já que o compilador faz, né? O problema é que a sua stack e data segment tem tamanhos limitados, então, você não pode simplesmente jogar tudo aqui. Aqui entra o grande herói que pode virar vilão.


Heap: A heap é onde você aloca a memória dinamicamente (usa malloc/new), ela é bem maior e não sofre do problema da stack e data segment, só que tem um problema, você precisa sempre monitorar porque se não, você pode causar vazamento de memória, já que essa varíavel vai ficar rodando pra sempre. Então o gerenciamento da memória fica por sua conta, você é responsável por destruir a variavel (usando free/delete).

```C++
//Exemplo sem leak

void exemplo() {
    int* array = new int[10];

    array[5] = 25;

    delete[] array; //array morre aqui.
} //Fora do escopo
```

```C++
//Exemplo com leak
void exemplo() {
    int* array = new int[10];

    array[5] = 25;
} // Array nunca morre e consequentemente vive fora do escopo causando memory leak.
```


Agora com essa introdução, podemos ir direto ao tópico que é RAII, então vamos começar com a melhor pergunta possível:
o que significa RAII? Traduzindo ao pé da letra "aquisição de recurso é inicialização". É... um nome que não diz nada. Eu particularmente gosto de chamar "construtor e destrutor". Eu conheci RAII em C++, e RAII em C++ está totalmente relacionado com Orientação a Objetos e esses dois conceitos. Apesar disso, acho que o nome Scope-Based Resource Management (SBRM) encaixa mais no contexto, já que RAII não tem a ver com orientação a objetos e Rust, por exemplo, não tem classes e usa RAII.


E quando o RAII? Foi inventado. Bom, eu sinceramente tentei achar algo sobre e o mais próximo que cheguei foi que o termo já foi pensado pelo Bjarne Stroustrup em 84, então digamos que foi aí. Bom, então desde da primeira versão do padrão de C++ (98) o RAII está presente, só ver os containers padrões da STL, como por exemplo Vector. Bom, agora chega de historia e vamos pra prática.


Bom, lembra que eu falei que RAII estava ligado a orientação a objetos? Então, agora vou mostrar como você poderia resolver o problema de new e delete do Array com RAII. Primeiramente, vamos abstrair o array para uma classe.

```cpp
template<typename T, std::size_t size>
class Array {

public:
    Array(T array[size]) {
        for (int i = 0; i < size; ++i) {
            _array[i] = array[i];
        }
    }

private:
    T _array[size];

};
```

Nossa classe agora temos um construtor com array de C. Para implementarmos RAII na nossa classe, só temos que implementar um destrutor que garante que toda vez que nosso objeto sair do escopo, ele seja deletado. 

```cpp

template<typename T, std::size_t size>
class Array {

public:
    Array(T array[size]) {
        for (int i = 0; i < size; ++i) {
            _array[i] = array[i];
        }
    }

    ~Array() {
        delete[] _array;
    }
private:
    T _array[size];

};
```

Pronto, agora é garantido que toda vez que nosso objeto Array sair do escopo, ele vai ser deletado evitando memory leak. 


Bem simples, né? Agora temos um container que se auto gerencia. Outra vantagem do RAII é que ele é thread safe, então você pode criar containers com Mutex que ele também vai gerenciar a memória pra você. 


Até agora, tudo lindo, né? Mas e se acontecer um erro, o que vai acontecer? RAII originalmente foi inventado pra ser exception safe, então seu codigo deve soltar uma exception ou erro. Normalmente na sua linguagem isso já é implementado nos operadores, mas você pode jogar a sua personalizada se quiser (recomendado). Um caso que pode acontecer é por exemplo: Dar algum erro enquanto você escreve num arquivo. Então nesse caso, como o RAII faria? Bom, ele só fecharia o arquivo, liberando a memória em questão e o arquivo pode ser corrompido e a exception seria solta. Obviamente, esse é no pior dos casos, mas é um caso interessante a se pensar.


Bom, já que falamos de um problema que pode ocorrer, que tal falarmos de algumas limitações do RAII? Você já pensou do por quê C++ moderno pede pra você usar smart pointers ao em vez de new e delete pra alocação dinâmica? Do ponto de vista do RAII é bem mais simples eliminar objetos na stack já que é mais facil de achar o ciclo de vida delas, já que a stack é uma porção da memória bem mais organizada. Já na heap, é bem complicado já que a memória nessa parte não é organizada. Pra resolver esses problemas foram criados os smart pointers.


O que são smart pointers? Bom, eu não vou entrar em muitos detalhes. Porém, imaginem Então é isso, espero que a thread tenha ficado legal e obrigado por ler até aqui. :)que na memória existem endereços e você precisa de um especifico pra você. Então você vai dizer pro sistema operacional que precisa daquele endereço e ele vai te dar, e aí você armazena o que quer nesse endereço. Isso é um ponteiro. Termo "smart" (inteligente) vem porque ele não precisa ser liberado da memória, ele faz isso pra você. Agora vamos ver os tipos de smart pointers


Basicamente temos dois tipos de smart pointers. Um que vai ser baseado em ownership (unique_ptr em C++ e box em Rust). O que seria ownership? Resumidamente, seria que um valor pertence aquele objeto ou seja, no contexto de RAII, se aquele valor não pertence ao objeto ele deve ser destruido (não está mais no escopo), dessa forma você pode evitar muitos ciclos ou ficar redeclarando, você pode só mover a ownership pra onde você quer. No entanto, concorda comigo que isso é limitado? Já que pra isso, teriamos que mover toda vez a ownership de um objeto para o outro? E se eu quiser manter o anterior vivo por mais tempo, como eu faria? Bom, aí que entra o segundo tipo de smart pointer.


Esse segundo tipo se baseia numa tecnica chamada "Reference Counting" (shared_ptr em C++, Rc em Rust) basicamente, esse tipo de smart pointer vai guardar as referências dos seus objetos num contador como diz no nome, então cada vez que você cria um novo objeto esse contador aumenta, assim fica mais fácil de manter varios objetos vivo e tira-los do escopo após o uso. O ruim dessa abordagem é que ela não é thread safe. 


Mas espera, RAII não era thread safe? Sim, ele é. O problema é que a implementação dos smart pointers não é. Porém, é um problema que as linguagens normalmente resolvem usando Mutex, Atomic operations e etc... em C++ por exemplo, são implementadas alternativas com "shared" no nome pra fazer referência a solução com shared_ptr.


Extras:

![RAII pra C no GCC](http://echorand.me/site/notes/articles/c_cleanup/cleanup_attribute_c.html)


![Palestra da CPPCON sobre RAII e Smart Pointers](https://www.youtube.com/watch?v=07rJOzFRs6M)


![Palestra da CPPCON sobre shared_ptr Atomic](https://www.youtube.com/watch?v=lNPZV9Iqo3U)


![Video sobre reference couting](https://www.youtube.com/watch?v=2JasKMJonaQ)