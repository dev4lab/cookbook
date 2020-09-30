# Detecção de objetos usando o método Haar Cascade

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

1 - Escolher o objeto.


2 - Selecionar imagens negativas.


3 - Selecionar imagens positivas.


4 - Gerar um vetor de positivas.


5 - Treinar o classificador.

### 1 - Escolher o objeto.

