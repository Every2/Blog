---
title: Meu setup para 2025
date: 2024-12-15
---

Olá, meu caro amigo leitor. Primeiramente, boas festas para você. Neste fim de ano, a partir das minhas férias, decidi mudar meu setup e fazer novas escolhas pessoais. Eu decidi optar por um setup que eu possa programar e colocar "minha cara". Apesar de não customizar tanto minha distro, eu achava chato perder tempo baixando coisas para ela e ela não vir com as coisas "padrão". E se eu trocasse de distro, teria que fazer um esforço enorme para voltar ao "padrão".

Dessa forma, acabei me deparando com algo que já estava de olho há um tempo, mas sempre tive preguiça de aprender a usar: os WM (Window Managers). Basicamente, diferente do GNOME ou KDE, os window managers só cuidam das partes das janelas e são bem mais voltados para o teclado. Todavia, são bem mais customizáveis, algo que eu inicialmente não queria, mas é o preço a se pagar por ter mais "controle" do meu ambiente.

Sendo sincero, o resultado ficou até legal com o meu tema e não foi tão "doloroso" assim. Não customizei muito e ficou bem legal o esquema de cores. Ainda estou aprendendo a lidar com as keybinds que fiz. No entanto, vamos falar das minhas escolhas.


# Fastfetch

Acho que, como a maioria de vocês sabe, o `neofetch` foi descontinuado. Então, optei por uma nova alternativa chamada `fastfetch`, que faz a mesma coisa, mas é feita em C e vem com mais "opções" por padrão. A única coisa que me desagradou é que, para alterar a imagem, caso ela não seja uma arte ASCII, é necessário fazer um alias via shell para ele entender que é para ser passada uma imagem. É só um detalhe, mas que é meio chatinho de configurar.


![imagem do fastfetch](https://raw.githubusercontent.com/Every2/blog/main/images/fastfetch.png)


# Sway

Como eu havia dito, troquei o GNOME por um window manager, e esse window manager foi o Sway. Inicialmente, troquei por conta da possibilidade de configurar tudo em `Guile` (um dialeto de LISP), mas apesar de conseguir usar algumas das vantagens, as coisas que mais me interessavam não funcionaram. Então, optei por configurar na linguagem padrão mesmo. Outra coisa que me fez escolher o Sway foi o fato (apesar de controverso) de funcionar no Wayland. Eu não sei vocês, mas eu tenho um setup todo AMD e provavelmente não irei trocar tão cedo, então ficar no Wayland não é um problema. Além disso, o Sway é altamente compatível com o `i3WM`, caso eu mude para NVIDIA. Não seria problema migrar para o Xorg com i3.


![imagem do sway](https://raw.githubusercontent.com/Every2/blog/main/images/sway.png)


## Diferenças

Bom, eu queria um setup configurável, certo? Com grandes poderes, vêm grandes responsabilidades... Como o Sway é um window manager, várias coisas que você teria como padrão em uma DE (Desktop Environment) não vêm configuradas e você tem que adicioná-las, como, por exemplo, um sistema de notificação. Aqui, por exemplo, eu uso o `Mako notifier`, que está me atendendo bem, nada a reclamar. Porém, entendo que para algumas pessoas isso pode ser um contra. Normalmente, para mim seria, mas como eu só vou configurar isso uma vez em muito tempo, achei bem prático. Abaixo, vou citar as coisas que acabei colocando e que viriam normalmente para você.


## Regulador de volume

Aqui estou usando o pacote `pavucontrol` para gerenciar meu volume e algumas hotkeys para abaixar e aumentar o volume. Normalmente, isso já vem configurado por padrão para você.


## Controle de brilho

Apesar de eu configurar isso manualmente no meu monitor, achei interessante ter via o pacote `brightnessctl`, que controla o brilho da minha tela.


## Barra/Painel superior

Acho que muita gente acaba usando isso, mas eu não tenho um painel interativo há alguns anos, já que geralmente uso uma DOC e mexia via a interface do GNOME mesmo. Agora eu uso a `waybar`, que tem algumas informações a mais, mas achei bem bonita e deixei combinando com meu Sway.


## Lançador de aplicativos

Normalmente, a gente não tem lançador de aplicativos da forma que é usado aqui, mas como o design de um window manager é focado no teclado e não temos ícones na área de trabalho, essa é a forma de abrirmos as coisas fora do terminal. Eu estou usando o `ulauncher` (como você pode ver na foto) para abrir meus aplicativos sem um terminal. Achei bem mais prático criar atalhos para lançar meu Emacs (que é gerenciado via Guix, então não tem formas de abrir fora do terminal, a menos que eu crie manualmente), por exemplo, do que criar todo um arquivo de ícone para ele.


# Distro

Aqui vai uma das mais polêmicas: Eu troquei o Arch Linux pelo Kubuntu (Ubuntu com KDE). A decisão envolveu o fato de que eu já estava tendo problemas com distros rolling release, com drivers de áudio simplesmente crashando e eu tendo que desligar meu computador via o botão de poweroff. Minha namorada também estava querendo mudar de distro, eu também estava, então decidi mudar para uma LTS.

Apesar disso tudo, estou usando o Ubuntu sem snaps (como visto na print do Fastfetch) e até agora o problema de áudio não apareceu. Só notei um crescimento bem alto nos pacotes, já que aqui uso bastante `.deb` em vez de AppImages ou compilar manualmente, como fazia no Arch. Até agora, nada a reclamar, só algumas coisas que ficam bloqueadas por conta da imposição do snap e acabo tendo que usar um monte de PPAs.


# Conclusão

Bom, acredito que não devo mudar meu setup por um bom tempo. Setar meu ambiente foi bem mais rápido do que era com o GNOME, só demorando mesmo por conta da instalação de pacotes grandes, como o `texlive-full`, que demoraria bastante de qualquer forma. Sendo sincero, essa foi a parte que mais demorou, já que configurar o ambiente foi apenas copiar arquivos do meu GitHub para certas pastas após a instalação do ambiente.

Após isso, acredito que é bem interessante ter um setup programável, já que em questão de minutos você tem o seu setup completo e não precisa de muito esforço ou ficar logando em contas para ter as configurações salvas. Se você não deu uma chance por qualquer razão que seja, configure aos poucos em uma máquina virtual no seu tempo livre e, depois disso, migre. Você verá que seu esforço para ter um setup bom em poucos minutos vai ter valido a pena.

Por hoje é isso, me despeço de vocês por este artigo. Novamente, boas festas e nos vemos por aí!
