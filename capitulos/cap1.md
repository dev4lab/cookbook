# Detecção de Bordas
A visão computacional é uma área da ceiência que desenvolve teorias e tecnologia tendo com objetivo extrair informações de dados multimencionais. Quase sempre, recorremos a uma analogia de como nós detectamos e reconhecemos objetos. Um objeto é caracterizado por um conjunto de atributos como: cor, texturas e forma geométrica. A extração de contorno poder forncer informções sobre a geometria. Por exemplo podemos identificar diversas formas geométricas como retangulo circulo, triangulos, linhas e outros.

Nessa capítulo vamos conhecer a base dos algoritmos de deteção de borda e implementar o algoritmo de Canny.

para executar os scripts mostrado aqui você precisará ter em sua máquina uma versão do python 3 e o OpenCV instalados.

### O que é uma borda?

Uma borda á caracteriza por uma variação abrupta entre os piexels vizinhos de uma imagem.

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/img_linha.png" width="400" height="250"/>
    </p> <p align="center"> <b>Figura 1:</b>  Pista de corrida </p>
</div>

Primeiro vamos pensar apenas na linha selecionada Figura 1, podem representa-la por uma função I(x) cujo dominio é uma lista [254,254,173,138,79,44,45,53]

Como nossa função é dicreta (só admite valor inteiro) não podemos calcular diretamente a derivada dessa função mais podemos fazer uma boa aproximação.

A derivada é uma operação matemática que permite calculara a taxa de variação de uma função ou de dois pontos muito próximos. Ela é definida pela equação 1.
<div align="center">
         <p align="center">
         <img src="https://render.githubusercontent.com/render/math?math=\Large{\frac{df}{dx}=\lim_{h\to 0} \frac{f(x %2B h)- f(x)}{h}} ">
         </p>  <p align="center"> <b>Equção 1: </b> Derivada. </p>
 </div>
 Supondo que x seja a posição que estamos na lista, então f(x) é o valor do pixel e f(x+h) é próximo pixel. Acontece que quando o intervalo h for muito pequeno vamos pegar variações decorrete de ruídos na imagem. Sem assim, não deverimos nos procupar em fazer pequenos ajustes nesse sentido. Por exmplo, podemo dizer que nossa derivada no ponto x é dada por f(x+h)-f(x-h), ou seja a diferença do pixel próximo pixel pelo pixel anterir ou ponto x.

<div align="center">
         <p align="center">
         <img src="https://render.githubusercontent.com/render/math?math=\Large{\frac{dI}{dx}=I(x %2B 1)-I(x-1)} ">
         </p> <p align="center"> <b>Equação 2: </b> Derivada aproximada. </p>
 </div>

Desconcideramos a divisão por h como na Equação 1, porque nesse contexto ele é apenas um normalizador da função, ou seja ele será um parâmetro que vamos passar al realizar os calculos.

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/gride1d.png" width="300" height="50"/>
    </p> <p align="center"> <b>Figura 2: </b> Derivada aproximada para 1. </p>
</div>

Um dos motivos da aproximação de fizemos é por conta dos ruídos, porém essa nova equação pode ser representada por um kernel. Computacionalmente é mais interessante convolver um kernel por uma imagem do que aplicar uma função. 

 <div align="center">
    <p align="center">
    <img src="../imagens/cap1/kernel1d.png" width="300" height="50"/>
    </p> <p align="center"> <b>Figura 3: </b> Kernel para calculo de derivada. </p>
</div>

Na Figua 4 realizamos esse operação para toda a linha da imagem 1, tente identificar onde está a região que selecionamos.

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/df.png" width="500" height="350"/>
    </p> <p align="center"> <b>Figura 4: </b> Grafico de linha da selecionada na figura 1. </p>
</div>

A lista começa com valor alto, 254 decai ate 44 e sobe novamente para 53. Essa variação acontece no intervarlo 160 à 178 (aproximado) do eixo x.

Se expandirmos esse ideia para um plano 2D nossa função anterir pode ser decrita da seguinte forma.

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/grade2d.png" width="300" height="200"/>
    </p> <p align="center"> <b>Figura 5: </b> Aproximação de derivada. </p>
</div>

Agora temos uma derivada parcial. Da mesma forma, podemos rescrever isso com um kernel

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/kernel2D.png" width="450" height="180"/>
    </p> <p align="center"> <b>Figura 6: </b> Kernel para derivada parcial. </p>
</div>

Esse par de kernel na Figura 6 tem o nome de operador Sobel. O OpenCV tem esse operador implementado aqui [cv2.sobel](https://docs.opencv.org/3.4/d4/d86/group__imgproc__filter.html#gacea54f142e81b6758cb6f375ce782c8d), então vamos usar.

```python
import cv2
ddepth = cv2.CV_16S
# Carrega imagen "frame.png" em escala de cinza
gray = cv2.imread("frame.png",cv2.IMREAD_GRAYSCALE)
# calcula derivada de primeira ordem na direção x 
grad_x = cv2.Sobel(gray, ddepth, 1, 0, ksize=3,scale = 1)
# calcula derivada de primieira ordem na direção y
grad_y = cv2.Sobel(gray, ddepth, 0, 1, ksize=3,scale = 1)

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
    <img src="../imagens/cap1/saida_sobel.png" width="500" height="250"/>
    </p> <p align="center"> <b>Figura 7: </b> Resultado do Sobel. </p>
</div>

Percebe que aplicamos o operador Sobel duas vezes, primeiro na direção x e depois na direção y. A composição dessas derivada é matematicamente conhecida com gradiente. O gradiente é um vetor que aponta na direção onde a função tem a maior variação. No entanto, o que nos interessa aqui é magnitude desse gradiente, ou seja o quão abrupta é essa variação. O múdulo do gradiente poder ser encontrado usando a Equação 2 (calculamos com a função cv2.addWeighted).

<div align="center">
         <p align="center">
         <img src="https://render.githubusercontent.com/render/math?math=G =\sqrt{{(G_x)^2} %2B {(G_y)^2}}">
         </p> <p align="center"> <b>Eqiação 3: </b> Magnitude do gradiente. </p>
 </div>

O Sobel é uma das operações mais relevantes para detectar contorno em imagans. Embora exista alternaticas como cv2.Scharr que tem uma aproximação melhor da derivada. O Sobel ainda é um dos principais métodos empregado nos algoritmo de detecção de borda. 

# Algoritmo de Canny

O algortimo de Canny executa varios estágio para detectar uma borda.

#### 1. remoção de ruídos.

Na Figura 4, o grafico da derivada apresenta bastante ruido, isso acontece porque pegamos micros variações locais. Canny usa um filtro gaussiano para resolver isso. Veja como o filtro afeta a derivada na Figura 8.

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/gauss.gif" width="500" height="350"/>
    </p> <p align="center"> <b>Figura 8: </b> Efeito de filtro gaussiano.</p>
</div>  

#### 2. Calcular gradientes.
O filtro Sobel discutido no tópico anterir é usado aqui para calcular os gradientes.

#### 3. Máximos locais.
Nessa etapa uma varredura completa é realizada na imagem em busca de gradientes máximo locais, esse processo elemina bordas largas ou duplicadas.

#### 4 Limiar de hesterese.

Tudo que esta abaixo de minVal é descartado. o que esta entre minVal e maxVal é mantido apenas se parte do contorno estiver acima de maxVal. Na Figura 9, A é mantido porque esta acima de maxVal, C é mantido, embora esteja abaixo de maxVal ele esta conectado a A. Já o B é removido, pois esta totalmente dentro da área delimintada.

<div align="center">
    <p align="center">
    <img src="../imagens/cap1/grafico_maxval.png" width="500" height="350"/>
    </p> <p align="center"> <b>Figura 9: </b>Região delimitada pelos liminar maxVal e minVal. <h5>Fonte: <url>https://docs.opencv.org/master/da/d22/tutorial_py_canny.html</url></h5>.</p>
</div>  

### Usando Canny
No OpenCV temos uma implementação do algoritmo de Canny, o segundo e o terceiro parâmetro passados são minVal e maxVal.

```python
import cv2
# Carrega imagem em escala de cinza
gray = cv2.imread('frame.png',cv2.IMREAD_GRAYSCALE)
# aplica algoritmo de Canny com minVal=100 e maxVal=200
edges = cv2.Canny(gray,100,200)
# concatena imagem original com o resultado
saida=cv2.hconcat((gray,edges))
#redimenciona 
saida=cv2.resize(saida,None,None,0.4,0.4)
#Mostra saida
cv2.imshow("janela",saida)
cv2.waitKey()
```
<div align="center">
    <p align="center">
    <img src="../imagens/cap1/saida_canny.png" width="500" height="250"/>
    </p> <p align="center"> <b>Figura 9: </b> Resultado do Canny. </p>
</div>

<div align="center">
    <p align="center">
    <iframe width="560" height="315" src="https://www.youtube.com/embed/DNIQdUHHB7A" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
    </p> <p align="center"> <b>Figura 9: </b> Resultado do Canny. </p>
</div>

[![Everything Is AWESOME](../imagens/cap1/yutub.png)](https://www.youtube.com/watch?v=DNIQdUHHB7A&feature=youtu.be&ab_channel=eltonfernando")


## Conclusão
De fato a aréa de visão computacional é permeada por aplicação matemáticas de alta complexidade. No entanto, bibliotecas como OpenCV tem simplificado, permitindo que pessoas de divesas áreas desenvolva suas própias aplicações. Se você gostou desse assunto, junte-se a nós no grupo do telegram.

**Atenciosamente**

Elton fernandes dos Santos

Engenheiro eletricista e mestrando em Zootecnia na Universidade Federal do Mato Grosso.

Autor do blog [visioncompy](www.visioncompy.com)

## Referências

<table>
     <tr>
        <td> Documentação oficial OpenCV v 4.5.0: Sobel. fonte https://docs.opencv.org/master/d2/d2c/tutorial_sobel_derivatives.html</td>
    </tr>
    <tr>
        <td> Documentação oficial OpenCV v 4.5.0: Algoritmo de Canny. fonte https://docs.opencv.org/master/da/d22/tutorial_py_canny.html</td>
    </tr>
</table>


