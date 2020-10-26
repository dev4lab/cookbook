# Detecção de Bordas
A detecção de objeto em imagem é um dos problemas classicos de visão computacional. Muitos métodos já foi desenvolvido para resolver esse problema e eles recorrem a uma analogia de como nós detectamos e reconhecemos objtos. Um objeto é caracterizado por um conjunto de atributos, como cor, texturas e forma geométrica. sendo assim obter os contorno de objeto em imagem pode ser útil para muitas coisa. Por exemplo podemos identificar diversas figuras geométricas como retangulo circulo, triangulos, linhas e outros.

Nessa capítulo vamos discutir como a base de como um algoritmo de deteção de borda funciona e implementar alguns deles para verificar os resultados. 


### O que é uma borda?

Em uma imagem uma borda á caracteriza por uma variação abrupta entre os piexels vizinhos.

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/img_linha.png" width="400" height="250"/>
    </p> <p align="center"> <b>Figura 1:</b>  Pista de corrida </p>
</div>

Primeiro vamos pensar apenas na linha selecionada imagens, podem representa-la por uma função i(x) cujo dominio é uma lista [254,254,173,138,79,45,53]

como nossa função é dicreta (só admite valor inteiro) não podemos calcular diretamente a derivada dessa função mais podemo obter uma boa aproximação.

A derivada de uma função é dada por.
<div align="center">
         <p align="center">
         <img src="https://render.githubusercontent.com/render/math?math=\large{\frac{df}{dx}=\lim_{h\to 0} \frac{f(x %2B h)- f(x)}{h}} ">
         </p>
 </div>


<div align="center">
    <p align="center">
    <img src="../imagens/cap1/df.png" width="500" height="350"/>
    </p> <p align="center"> <b>Figura 2: </b> Grafico de linha da selecionada na figura 1. </p>
</div>

existem vários operadores para detecção de bordas,, Sobel, Robinson
baseado em operações de convoluções simples.

Encontrar contornos em imagem se resume em obter os gradiente de maior intensidade.

### Sobel
O Sobel é algotritmo que calcula os gradiente aproximado de uma imagem. Porem o sobel usa um grupo de pixeis a

         [-1 -2  1]
    dx = [0   0  0]
         [-1  2  1] 

fdasfd

         [-1 -2 -1]
    dy = [ 0  0  0]
         [ 1  2  1] 
Nós vamos usar uma imagem em escala de cinza para facilitar a copreenção, mas 
isso pode ser aplicado em uma imagem de cor. Em imagem de cor o calculo tem como base a distância elclidiana dos
pixels.
<div align="center">
    <p align="center">
    <img src="../imagens/cap1/gauss.gif" width="500" height="350"/>
    </p>
</div>  
mostrar como o sobel melhora com bur       
# Algoritmo de Canny
convolução de gaus
<div align="center">
         <p align="center">
         <img src="https://render.githubusercontent.com/render/math?math=G =\sqrt{{(G_x)^2} %2B {(G_y)^2}}">
         </p>
 </div>


insert
<div align="center">
    <p align="center">
    <img src="../imagens/cap1/eq_grad.png" width="180" height="35"/>
    </p>
</div>
ds

Estruturas salientes.
