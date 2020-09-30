# Processamento Morfológico de Imagens

## Imagens e Conjuntos

Em algumas aplicações de Processamento Digital de Imagens podemos utilizar conceitos da Teoria dos conjuntos para realizarmos algumas análises e inferir certas informações em imagens. Por exemplo, podemos dizer que um objeto, representado em uma imagem, é formado pelo conjunto de pixels que o constituem.

<div align="center">
    <p align="center">
    <img src="../imagens/cap2/Quadra.jpg" width="250" height="200"/>
    </p>
    <p> <b>Figura 1:</b> Pista de Corrida </p>
</div>

Na Figura 1 você pode perceber facilmente algumas árvores e uma pista de corrida. Podemos dizer que somos capazes de determinar visualmente as diferenças entre as árvores e a pista de corrida apenas afirmando que o conjunto dos pixels azuis formam a pista de corrida, enquanto o conjunto dos pixels de cor verde-escura formam as árvores.

O que eu quero te mostrar neste capítulo é que podemos realizar algumas operações sobre estes conjuntos de pixels que representam objetos nas imagens! A estas operações sobre conjuntos de pixels chamamos de Processamento Morfológico de Imagens.


## Os Elementos Estruturantes

Já sabemos que podemos considerar objetos representados em imagens como conjuntos e que podemos também realizar operações sobre estes conjuntos. Bom, mas se vamos fazer operações sobre este conjunto de pixels, precisamos de um outro conjunto para realizar estas operações, não é mesmo? Muito bem! Para realizarmos estas operações precisamos dos <b>Elementos estruturantes</b>!

Observe a Figura 2, na qual temos uma imagem representando um objeto em tons de cinza ao lado esquerdo, enquanto que, ao lado direito, temos um pequeno conjunto de pixels que está percorrendo esta imagem (algo parecido com um filtro).

<div align="center">[INSERIR IMAGEM GIF AQUI]</div>


A Figura 2 mostra uma operação entre dois conjuntos (O elemento estruturante e o objeto representado na imagem) que resulta em uma redução da quantidade de pixels que presentam o objeto. Esta operação é chamada de <b>Erosão</b>.


### Mas o que foi feito na Figura 2? Como aplicar a Erosão em uma imagem?

Vamos considerar que o conjunto A é o objeto representado na imagem e que o conjunto B é o elemento estruturante, ambos apresentados na Figura 2.

1. Primeiro é feita uma "varredura" do conjunto B (Elemento estruturante) em A para que a origem de B passe por todos os elementos de A.

2. Depois é feita uma verificação: Para cada localização da origem de B, considera-se que o pixel de A é um membro do novo conjunto caso todos os elementos de B estejam contidos em A, caso contrário, descarta-se este pixel.

3. Depois de fazer essa varedura em toda a imagem, o resultado final é alcançado com o objeto tendo um conjunto menor de pixels conforme mostra a Figura 2.

<div align="center">
<img src="https://render.githubusercontent.com/render/math?math=e^{i \pi} = -1">