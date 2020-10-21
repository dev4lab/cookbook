# Detecção de Bordas
A detecção de objeto em imagem é um dos problemas bastante discutido em visão computacional,
existe varios método para detectar um objeto. O método adequado para determinada situação consite naquele que melhor consilia velocidade e acurácia.


### O que é uma borda
Em uma imagem uma borda á caracteriza por uma variação abrupta entreo os piexels vizinho.

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/img_linha.png" width="400" height="250"/>
    </p>
</div>

matematimente uma borda é um gradiente ou seja, uma variação de intensidade.
<div align="center">
    <p align="center">
    <img src="../imagens/cap1/df.png" width="500" height="350"/>
    </p>
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