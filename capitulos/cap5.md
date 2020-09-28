# Detecção de objetos com o método Haar Cascade

Nesse capítulo você irá aprender uma maneira rápida e direta de como criar um classificador Haar Cascade. Além disso, no final do capítulo será disponibilizado um código para detecção de objetos com o classificador criado, que com pequenas alterações, pode se adequar a qualquer situação.

Nesse contexto, o método Haar Cascade, é um método de detecção de objetos proposto por Paul Viola e Michael Jones. É uma abordagem baseada em Machine Learning, em que uma função cascade é treinada com muitas imagens positivas e negativas. Logo, é usado para  detectar objetos em outras imagens.

## Preparando o ambiente

Para esse projeto é necessário que você tenha instalado em sua máquina apenas 3 itens.

1 - Editor de textos de sua preferência, eu particularmente uso o Visual Studio Code.

2 - Alguma versão Python de sua preferência, eu particularmente uso a  3.8.5

3 - Biblioteca OpenCV .

Aqui não irei explicar como você pode fazer o download e instalação desses itens, pois na internet existem diversos tutorias detalhados de como fazer isso.

## Passos

A criação de um classficador usando o HaarCascade pode ser descrita em um conjunto de 5 passos.
1 - Escolher o objeto
    * serão objetos rígidos como uma logo (nike) ou com variações (cadeira,copo)?

    * Objetos rigidos são mais eficientes e mais rápidos.

    * Ao treinar muitas variações pode ser que o classificador fique fraco, portanto, fique atento a isso.

    * Objetos que a cor é fundamental não são recomendados, pois as imagens serão passadas para a escala de cinza.


2 - Selecionar imagens negativas
    * Podem ser qualquer coisa, menos o objeto

    * Devem ser maiores que as positivas, pois o openCV vai colocar as imagens positivas dentro das negativas.

    * Se possível usar fotos de prováveis fundos onde o objeto é encontrado

        *Ex: Objeto = carro
         Usar imagens de asfalto e ruas vazias.


3 - Selecionar imagens positivas
    * Apenas o objeto

    * Quantas imagens?

        Depende da: Qualidade da imagem, tipo do objeto, poder computacional disponível.

    * As imagens devem ter o mesmo tamanho.

    * A proporção precisa ser a mesma, caso contrário a openCV faz isso automaticamente e gera problemas de distorção do objeto.

        * Ex: Uma imagem 100x50, passada pra 25x25, vai ter o objeto descaracterizado.

    * Imagens grandes podem gerar problemas, fazendo o treinamento durar até meses.



4 - Gerar um vetor de positivas


5 - Treinar o classificador
