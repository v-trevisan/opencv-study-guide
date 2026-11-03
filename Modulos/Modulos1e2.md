
# ***Módulo 1.) Introdução***

O OpenCV é uma biblioteca open source (código fonte aberto) voltada para:
- **Visão computacional**;
- **Processamento de imagens;**
- **Processamento de vídeo**;
- Algumas ferramentas de **machine learning**.

Primeiro, é necessária a instalação no terminal da seguinte forma:
```
pip install opencv
```

Depois, importamos a biblioteca em Python da seguinte forma:
``` Python
import cv2
```

---
## ↪ 1.) Imagens

Uma imagem é formada por **pixels (caixas)**, e sua resolução depende da quantidade que utilizamos:
![Descrição da imagem](caminho/da/imagem.png)

```
        x
        0    1    2    3
     ┌────┬────┬────┬────┐
 y 0 │    │    │    │    │
     ├────┼────┼────┼────┤
   1 │    │    │    │    │
     ├────┼────┼────┼────┤
   2 │    │    │    │    │
     └────┴────┴────┴────┘
```

| TIPO | Horizontal(X) | Vertical(Y) |
| :--: | :-----------: | :---------: |
| VDA  |      640      |     480     |
|  HD  |     1280      |     720     |
| FHD  |     1920      |    1080     |
|  4k  |     3840      |    2160     |

Dessa forma, de acordo com o sistema de coloração da imagem, os pixels podem assumir valores distintos:
**(a) Imagem binária** - imagem preto e branco com 2 níveis de cor (0 ou 1);
```
┌────┬────┬────┐
│  0 │  0 │  1 │
├────┼────┼────┤
│  1 │  1 │  1 │
├────┼────┼────┤
│  0 │  0 │  0 │
└────┴────┴────┘
       ||
       \/
┌────┬────┬────┐
│    │    │ ██ │
├────┼────┼────┤
│ ██ │ ██ │ ██ │
├────┼────┼────┤
│    │    │    │
└────┴────┴────┘
```

**(b) Imagem em escala cinza** - se utilizar 8 bits, $2^8$, teremos 256 tonalidades de cinza entre o preto (0) e branco (255);
```
┌────┬────┬────┐
│ 20 │ 80 │ 200│
├────┼────┼────┤
│ 40 │120 │ 255│
├────┼────┼────┤
│  0 │ 70 │ 160│
└────┴────┴────┘
       ||
       \/
┌────┬────┬────┐
│ ▓▓ │ ▓▒ │ ░░ │
├────┼────┼────┤
│ ▓▒ │ ▒▒ │ ██ │
├────┼────┼────┤
│    │ ▓▒ │ ░▒ │
└────┴────┴────┘
```

**(c) Imagens coloridas** - em vez de **um número por pixel**, normalmente temos **três números por pixel**. Dessa forma, temos 3 imagens em escala cinza sobrepostas (Red, Blue, Green). Daí, origina-se a escala RGB de cores => $qtd.\ total\ de\ valores = Resolução [X \times Y] \times 3.$
```
┌─────────────────┬─────────────────┐
│ [255, 0, 0]     │ [0, 0, 255]     │
│                 │                 │
│ vermelho        │ verde           │
├─────────────────┼─────────────────┤
│ [0, 255, 0]     │ [255,255,255]   │
│                 │                 │
│ azul            │ branco          │
└─────────────────┴─────────────────┘
```

Para o OpenCV, o sistema de cores utilizado é **[B,G,R] (Blue, Green, Red)**, no padrão geral.

Agora, vamos ver alguns exemplos de funcionalidades com o OpenCV:

**1.) Criando uma variável imagem:**

```Python
# IMPORTANDO UMA IMAGEM
import cv2
#criando a variável imagem (array numpy):
img = cv2.imread("endereco_da_imagem.jpg/png")
#observamos as propriedades da imagem(altura,largura,canais)
print(img.shape)
#para mostrar a imagem
cv2.imshow("Output", img)
#devemos deixar um tempo de espera
cv2.waitKey(0) #0 deixa indefinido, outros valores deixam com X milliseg
```

**2.) Propriedades do img.shape:**
```Python
import cv2
img = cv2.imread("endereco_da_imagem.jpg/png")
#criando variáveis para receber cada propriedade da imagem (pois img.shape retorna um array numpy):
altura = img.shape[0]
largura = img.shape[1]
canais = img.shape[2]
#retornando apenas um pixel da imagem:
pixel = img[100, 200]


print(pixel) #retorna os valores BGR do pixel
print(altura)
print(largura)
print(canais)
```

O **sistema de coordenadas** utilizado, geralmente, é da seguinte forma:
```
          y
          ↑
          │
          │ (0,0)
──────────┼──────────→ x         [matemático]
          │
          │


(0,0) ─────────────────────→ x   [imagens]
  │
  │
  │
  │
  │
  ↓
  y

```

Por exemplo, suponha que temos uma imagem Full HD (1920x1080). Sua representação é:
```Python
print(img.shape) #(1080, 1920, 3)
              1920 pixels
         ←────── X ──────→

      ┌───────────────────────┐
      │(0,0)                  │
      │                       │
 1080 │          •            │
pixels│      (x=700,y=400)    │
  Y   │                       │
      │                       │
      └───────────────────────┘
```
Logo, para acessar o pixel(700,400), escrevemos img[400,700].

Exemplo para finalizar:
```Python
import cv2

img = cv2.imread("imagem.png")

print("Tipo:", type(img)) #array numpy
print("Shape:", img.shape) #(628,1200,3)
print("Dtype:", img.dtype) #uint8
print("Size:", img.size) #(628x1200x3) 

altura, largura, canais = img.shape

print("Altura:", altura) #628
print("Largura:", largura) #1200
print("Canais:", canais) # 3
  
pixel = img[100, 100]

print("Pixel:", pixel) #[4 1 3]
print("Blue:", pixel[0]) #4
print("Green:", pixel[1]) #1
print("Red:", pixel[2]) #3
```

---
## ↪ 2.) Leitura e escrita de imagens

Basicamente, uma imagem carregada no OpenCV é um **array numpy**, com valores representando os pixels. Agora, vamos ver como fazemos uma imagem que está salva no computador virar esse array, e como salvamos um array novamente como uma imagem:

>[!important]
> Utilizando os comandos **cv2.imread( )** e **cv2.imwrite( )**

## Imread( ):
```Python
import cv2
#Carregando uma imagem:
img = cv2.imread("imagem.jpg") #vira um array numpy
```

Agora, vamos ver a diferença entre **caminho relativo e caminho absoluto**:

(a) **Relativo**: é relativo ao diretório em que o código está sendo executado.
```Python
img = cv2.imread("imagens/gato.jpg") #ou
img = cv2.imread("./imagens/gato.jpg")
```
(b) **Absoluto**: é o caminho completo no computador.
```Python
img = cv2.imread(r"C:\Users\usuario\Imagens\gato.jpg")
#o 'r' antes do string evita que '\' seja lido como caractere
#ou
img = cv2.imread("C:/Users/Vinicius/Imagens/gato.jpg")
```

Descobrimos de onde o Python está procurando os arquivos utilizando:
```Python
import os #outra bilbioteca
print(os.getcwd()) #retorna o diretório atual
```

Portanto, a forma correta de carregar imagens é:
```Python
import cv2 
img = cv2.imread("gato.jpg") 
if img is None: 
	print("Erro ao carregar a imagem.") 
else: 
	print("Imagem carregada!") 
	print(img.shape)
```

>[!important]
>Podemos mandar um segundo parâmetro para o imread( ):
>** $img = cv2.imread(caminho, flag)$**

As **flags** indicam como queremos importar a imagem. As 3 principais são: 
**(a) cv2.IMREAD_COLOR** = comportamento padrão;
**(b) cv2.IMREAD_GRAYSCALE** = carrega a imagem em escala cinza;
**(c) cv2.IMREAD_UNCHANGED** = carrega a imagem preservando seus canais como estão no arquivo, quando suportados. Por exemplo, quando a imagem tem transparência.

>[!important]
>Podemos representar as escalas por números também:
>  1 → IMREAD_COLOR 
>  0 → IMREAD_GRAYSCALE 
>-1 → IMREAD_UNCHANGED

```Python
img = cv2.imread("gato.jpg", 0) #ou
img = cv2.imread("gato.jpg", cv2.IMREAD_GRAYSCALE)
```

Um exemplo para ilustrar todas essas propriedades é:
```Python
import cv2

img_color = cv2.imread("imagem.jpg", cv2.IMREAD_COLOR)
img_gray = cv2.imread("imagem.jpg", cv2.IMREAD_GRAYSCALE)
img_original = cv2.imread("imagem.jpg", cv2.IMREAD_UNCHANGED)

print("COLOR:", img_color.shape)
print("GRAY:", img_gray.shape)
print("UNCHANGED:", img_original.shape)

#Saídas:
#COLOR: (720, 1280, 3) 
#GRAY: (720, 1280) 
#UNCHANGED: (720, 1280, 3)
```

## Imshow( ):
```Python
import cv2
img = cv2.imread("imagem.jpg")
#Queremos escrever a imagem em um arquivo:
cv2.imwrite("nova_imagem.jpg", img) #cria um novo arquivo
```

Podemos mudar o formato do arquivo:
```Python
img = cv2.imread("gato.jpg") 
cv2.imwrite("gato.png", img) #ou
cv2.imwrite("gato.jpg", img) #ou
cv2.imwrite("gato.bmp", img)
```

O imwrite( ) retorna um valor: "True" se foi possível salvar e "False" se não foi. Logo, para uma escrita formal, temos:
```Python
if cv2.imwrite("saida.jpg", img):
    print("Imagem salva!")
else:
    print("Erro ao salvar.")
```

>[!important]
>Podemos fornecer um caminho para salvar a imagem. Mas, a pasta já precisa existir. Para garantir isso, podemos utilizar:

```Python
import os 
os.makedirs("resultados", exist_ok=True) cv2.imwrite("resultados/imagem.jpg", img)
```

Portanto, fechando um código formal completo:
```Python
import cv2

img = cv2.imread("imagem.jpg")

if img is None:
    print("Erro: imagem não encontrada.")

else:
    print("Imagem carregada com sucesso.")

    print("Shape:", img.shape)
    print("Tipo:", type(img))
    print("Dtype:", img.dtype)

    sucesso = cv2.imwrite("copia.jpg", img)

    if sucesso:
        print("Imagem salva com sucesso.")
    else:
        print("Erro ao salvar a imagem.")
```