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

O primeiro passo é escolher o objeto que será identificado, para isso você deverá pensar nos seguintes aspectos:

    * Serão objetos rígidos como uma logo (nike) ou com variações (cadeira,copo)?

    * Objetos rigidos são mais eficientes e mais rápidos.

    * Ao treinar muitas variações pode ser que o classificador fique fraco, portanto, fique atento a isso.

    * Objetos que a cor é fundamental não são recomendados, pois as imagens serão passadas para a escala de cinza.

Para esse projeto, escolhi o objeto faca para ser detectado.

### 2 - Selecionar imagens negativas.

Para selecionar as imagens negativas, você deve ficar atento aos seguintes aspectos:

    * Podem ser qualquer coisa, menos o objeto.

    * Devem ser maiores que as positivas, pois a openCV vai colocar as imagens positivas dentro das negativas.

    * Se possível usar fotos de prováveis fundos onde o objeto é encontrado.

        *Ex: Objeto = carro
         Usar imagens de asfalto e ruas vazias.

Logo, você deve ficar atento as iamgens escolhidas, pois como dito elas podem ser qualquer coisa, exceto o objeto escolhido, como escolhemos facas como objeto de detecção devemos, iremos precisar de imagens que não tenham facas.

Quantas imagens negativas?
É relativo, para esse projeto eu conto com 3000 imagens negativas, com diversas variações de fundo. Entretanto, isso vai depender dos resultados obtidos no treinamento, pode ser que eu precise de mais imagens ou não, isso será explicado mais a frente com mais detalhes.

Ex:

<div align="center">
    <p align="center">
    <img src="../imagens/cap2/86.jpg" width="100" height="100"/>
    </p>
    <p> <b>Figura 1</b>  </p>
</div>

<div align="center">
    <p align="center">
    <img src="../imagens/cap2/2833.jpg" width="100" height="100"/>
    </p>
    <p> <b>Figura 2</b>  </p>
</div>

<div align="center">
    <p align="center">
    <img src="../imagens/cap2/43.jpg" width="100" height="100"/>
    </p>
    <p> <b>Figura 3</b>  </p>
</div>

OBS: Todas essas imagens tem dimensões 100x100, essa informação será importante para futuras explicações.

Aqui é importante mencionar que você deve criar uma pasta (ex: projeto) onde estarão outras duas pastas, uma com as imagens negativas e outra com as imagens positivas.
Na pasta das imagens negativas você deve colocar esse arquivo:
<div align="center">
    <p align="center">
    <img src="../imagens/cap2/criar_lista.bat" width="100" height="100"/>
    </p>
    <p> <b>Figura 3</b>  </p>
</div>
E deve executá-lo ao final da escolha das imagens negativas, pois ele vai gerar uma lista com as imagens negativas.

### 3 -Selecionar imagens positivas.

Para selecionar as imagens positivas, você deve ficar atento aos seguintes aspectos:

    * Apenas o objeto.

    * Quantas imagens?

        * Depende da: Qualidade da imagem, tipo do objeto, poder computacional disponível.

    * As imagens devem ter o mesmo tamanho e a proporção precisa ser a mesma, caso contrário a openCV faz isso 
    automaticamente e gera problemas de distorção do objeto.

        * Ex: Uma imagem 100x50, passada pra 25x25, vai ter o objeto descaracterizado.

    * Imagens grandes podem gerar problemas, fazendo o treinamento durar até meses.

    * Sempre que possível usar imagens com fundo branco.

Como dito no primeiro passo, você deve tomar cuidado com as variações do objeto, caso você queira realizar a detecção de um objeto em diferentes ângulos é sugerido que você faça diferentes classificadores. Para exemplificar isso, cito o classificador haarcascade frontalface, que realiza a detecção frontal da face (esse classificador inclusive é de fácil acesso, até a openCV disponibiliza ele pra você) e caso você queira a detecção lateral da face, você deve usar outro classificador, esses cuidados devem ser tomados para que você tenha um bom classificador.

Em relação a quantidade, alguns estudos sugerem que um bom classificador deve ter no mínimo 5000 mil imagens como entrada para o treinamento.

Exemplos de imagens positivas que irei usar para o treinamento do classificador:

<div align="center">
    <p align="center">
    <img src="../imagens/cap2/faca_9.png" width="100" height="50"/>
    </p>
    <p> <b>Figura 1</b>  </p>
</div>


<div align="center">
    <p align="center">
    <img src="../imagens/cap2/faca_4.png" width="100" height="50"/>
    </p>
    <p> <b>Figura 2</b>  </p>
</div>


<div align="center">
    <p align="center">
    <img src="../imagens/cap2/faca_10.png" width="100" height="50"/>
    </p>
    <p> <b>Figura 3</b>  </p>
</div>

OBS: Todas essas imagens tem dimensões 100x50, essa informação será importante para explicações futuras.

Aqui temos o pulo do gato, é possível criar mais imagens positivas a partir das imagens que você já tem, para isso basta abrir o CMD, entrar na pasta onde estão suas imagens positivas e digitar esse comando:

‘’’
opencv_createsamples -img faca_1.png -bg negativas/bg.txt -info positivas/positivas.lst -maxxangle 0.5 -maxyangle 0.5 -maxzangle 0.5 -num 1800 -bgcolor 255 -bgthresh 10
‘’’

Parâmetros:

-img = Nome da imagem base.

-bg = Diretório e nome do arquivo com as informações das imagens negativas.

-maxangle (x,y,z) =  Variação de rotação que a imagem terá.

-num = Número de imagens que serão criadas.

-bgtresh = parâmetro que permite a retirada do fundo da imagem, deixando apenas o objeto de interesse (aqui se justifica o fundo branco). Esse parâmetro deve ser analisado com cuidado, pois:
<div align="center">
    <p align="center">
    <img src="../imagens/cap2/1.jpg" width="100" height="50"/>
    </p>
    <p> <b>bgtresh 10</b>  </p>
</div>


<div align="center">
    <p align="center">
    <img src="../imagens/cap2/0001_0010_0010_0060_0060.jpg" width="100" height="50"/>
    </p>
    <p> <b>bgtresh 100</b>  </p>
</div>


