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

Primeiro vamos pensar apenas na linha selecionada imagens, podem representa-la por uma função i(x) cujo dominio é uma lista [254,254,173,138,79,44,45,53]

como nossa função é dicreta (só admite valor inteiro) não podemos calcular diretamente a derivada dessa função mais podemo obter uma boa aproximação.

A derivada de uma função é dada por.
<div align="center">
         <p align="center">
         <img src="https://render.githubusercontent.com/render/math?math=\huge{\frac{df}{dx}=\lim_{h\to 0} \frac{f(x %2B h)- f(x)}{h}} ">
         </p>
 </div>
Assim podemos fazer uma aproximação e quanto mede uma vairação pontual.

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/gride1d.png" width="300" height="50"/>
    </p> <p align="center"> <b>Figura 2: Derivada aproximada </b> . </p>
</div>

<div align="center">
         <p align="center">
         <img src="https://render.githubusercontent.com/render/math?math=\huge{\frac{dI}{dx}=I(x %2B 1)-I(x-1)} ">
         </p>
 </div>
 Essa equação pode ser descrita por kernel.
 
 <div align="center">
    <p align="center">
    <img src="../imagens/cap1/kernel1d.png" width="300" height="50"/>
    </p> <p align="center"> <b>Figura 3: </b> Kernel para calculo de derivada. </p>
</div>

Na Figua 2 calculamos isso para a linha da imagem, tente identificar onde está a regição que selecionamos.

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/df.png" width="500" height="350"/>
    </p> <p align="center"> <b>Figura 4: </b> Grafico de linha da selecionada na figura 1. </p>
</div>

Se expandirmos esse ideia para o plano da imagem 2D nossa função anterir pode ser decrita da seguinte forma.


<div align="center">
    <p align="center">
    <img src="../imagens/cap1/grade2d.png" width="300" height="300"/>
    </p> <p align="center"> <b>Figura 5: </b> Aproximação de derivada. </p>
</div>

Da mesma forma podemos rescrever isso com um kernel

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/kernel2D.png" width="500" height="200"/>
    </p> <p align="center"> <b>Figura 6: </b> Kernel para derivada parcial. </p>
</div>

Esse par de kernel na Figura 6 tem o nome de operador Sobel. ![cv2.sobel](https://docs.opencv.org/3.4/d4/d86/group__imgproc__filter.html#gacea54f142e81b6758cb6f375ce782c8d)

```python
import cv2
ddepth = cv2.CV_16S
# Carrega imagen "frame.png" em escala de cinza
gray = cv2.imread("frame.png",cv2.IMREAD_GRAYSCALE)
# calcula derivada de primeira ordem na direção x 
grad_x = cv2.Sobel(gray, ddepth, 1, 0, ksize=3)
# calcula derivada de primieira ordem na direção y
grad_y = cv2.Sobel(gray, ddepth, 0, 1, ksize=3)

# calcula valor absoluto e converte para uint8 
abs_grad_x = cv2.convertScaleAbs(grad_x)
abs_grad_y = cv2.convertScaleAbs(grad_y)

# Calcula gradiente
grad = cv2.addWeighted(abs_grad_x, 0.5, abs_grad_y, 0.5, 0)
# concatena image gerada com a original
saida=cv2.hconcat((grad,gray))
# redimenciona em 60%
saida=cv2.resize(saida,None,None,0.4,0.4)
#salva imagem
cv2.imwrite("saida.png",saida)
cv2.imshow("janela", saida)
cv2.waitKey(0)
```

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/saida_sobel.png" width="600" height="300"/>
    </p> <p align="center"> <b>Figura 7: </b> Resultado do Sobel. </p>
</div>


<div align="center">
         <p align="center">
         <img src="https://render.githubusercontent.com/render/math?math=G =\sqrt{{(G_x)^2} %2B {(G_y)^2}}">
         </p>
 </div>





Nós vamos usar uma imagem em escala de cinza para facilitar a copreenção, mas 
isso pode ser aplicado em uma imagem de cor. Em imagem de cor o calculo tem como base a distância elclidiana dos
pixels.


# Algoritmo de Canny

O algortimo de Canny executa varios estágio para detectar uma contorno.

#### 1. remoção de ruídos.

Na figura 4, o grafico da derivada apresenta bastante ruido, isso acontece porque pegamos micros variações locais. Canny usa um filtro gaussiano para resolver isso. Seja como o filtro afeta a derivada no Figura 8.

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/gauss.gif" width="500" height="350"/>
    </p> <p align="center"> <b>Figura 8: </b> Efeito de filtro gaussiano.</p>
</div>  

#### 2. Calcular gradientes.
O filtro Sobel discutido no tópico anterir é usada para calcular o gradinentes.

#### 3. Máximos locais.
Nessa etapa uma varredura completa é realizada na imagem em busca de gradientes máximo locais, esse processo elemina bordas largas ou duplicadas.

#### 4 Limiar de hesterese.

Tudo que esta abaixo de minVal é descartado. o que esta entre minVal e maxVal é mantido apenas se parte do contorno estiver acima de maxVal. Na Figura 9, A é mantido porque esta acima de maxval, C é mantido embora esteja abaixo de maxVal ele esta conectado a A. Já o B é removido, pois esta totalmente dentro da área delimintada.

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/gauss.gif" width="500" height="350"/>
    </p> <p align="center"> <b>Figura 9: </b>Região delimitada pelos liminar maxVal e minVal. <h5>Fonte: <url>https://docs.opencv.org/master/da/d22/tutorial_py_canny.html</url></h5>.</p>
</div>  

### Usando Canny
No OpnCV temos uma implementação do algoritmo de Canny, 

```python
import cv2
gray = cv2.imread('frame.png',cv2.IMREAD_GRAYSCALE)
edges = cv2.Canny(gray,100,200)
saida=cv2.hconcat((gray,edges))
saida=cv2.resize(saida,None,None,0.4,0.4)
cv2.imshow("janela",saida)
cv2.waitKey()
```
<div align="center">
    <p align="center">
    <img src="../imagens/cap1/saida_canny.png" width="600" height="300"/>
    </p> <p align="center"> <b>Figura 9: </b> Resultado do Canny. </p>
</div>
