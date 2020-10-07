# Filtro de densidade usando Machine Learning e Clusterização

Nesse capítulo você irá aprender uma maneira rápida e fácil de utilizar Machine learning para remover ruídos em uma captura de objeto utilizando Range de cores.

Nesse contexto, iremos utilizar o método DBSCAN da Lib SKLEARN para reorganizar os pontos das cores selecionadas e identificar áreas de baixa densidade.

**Obs. Os códigos contidos neste capítulo foram foram desenvolvidos para serem rodados no Google Colab e pode sofrer algumas alterações para rodar fora do Colab (exemplo: a utilização da lib google.colab.patches para visualizar imagens).**

## Preparando o ambiente

Para esse projeto é necessário que você tenha instalado os seguintes itens em sua máquina.

1. Editor de textos de sua preferência.

2. Python 3.8.5 .

3. Bibliotecas (**OpenCV**, **Collections**, **Sklearn**, **Numpy**).

Caso queira, pode utilizar o Google Colab que tem o ambiente praticamente pronto.

## Passos

### Importar Libs

```python
import numpy as np
import urllib
import cv2
from google.colab.patches import cv2_imshow
from collections import Counter
from sklearn.cluster import DBSCAN
from sklearn import metrics
from sklearn.datasets import make_blobs
from sklearn.preprocessing import StandardScaler
```

### Download da imagem

**Como estamos utilizando o Google Colab, estaremos realizando o download e conversão das imagens diretamente do IMGUR**
```python
def url_to_image(url):
    resp = urllib.request.urlopen(url)
    image = np.asarray(bytearray(resp.read()), dtype="uint8")
    image = cv2.imdecode(image, cv2.IMREAD_COLOR)
    hsv = cv2.cvtColor(image, cv2.COLOR_BGR2HSV)
    return hsv

url = f'https://i.imgur.com/PLGlnWj.png'

img = url_to_image(url)
```
