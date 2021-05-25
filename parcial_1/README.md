

# Parcial 1: YOLOv3 + Darknet

Se decidió utilizar un sistema de detección de objetos en tiempo real llamado YOLO (*You Only Look Once*) en su versión 3 con una red pre-entrenada llamada Darknet.

A continuación se presenta una demo para mostrar el procedimiento seguido.

## Introducción
YOLOv3 fué presentado en 2019 por [Joseph Redmon][jr] y [Ali Farhadi][af] en la Universidad de Washington y es una actualización a la versión inicial creada en 2016: [You-Only-Look-Once][yolov1].

[jr]: https://scholar.google.com/citations?user=TDk_NfkAAAAJ&hl=en
[af]: https://scholar.google.com/citations?user=jeOFRDsAAAAJ&hl=en
[yolov1]: https://arxiv.org/pdf/1506.02640.pdf


## ¿Cómo funciona?
Se utiliza una única red neuronal para analizar las imágenes. El análisis consiste en:
- Sub-dividir cada imagen en regiones.
- Identificar los contornos (*bounding boxes*) de cada región.
- Predecir las probabilidades de las dimensiones de cada contorno. 

![calculo_bbox](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/img_demo/calculo_contornos.png)

> El cálculo de las probabilidades intenta obtener el ancho y el alto del contorno en base a los centroides devueltos por kMeans en la etapa de entrenamiento.

Finalmente, cada contorno encontrado es pesado usando las dimensiones calculadas por probabilidad y luego es asignado a una clase determinada que lo identifica con un objeto que YOLOv3 ya conoce.

Para más información revisar su [paper][link_paper].

[link_paper]: https://pjreddie.com/media/files/papers/YOLOv3.pdf

## Modo de uso

Debido a la falta de capacidad de cómputo, se optó por [Darknet][dn]. Un modelo open-source y pre-entrenado de red neuronal desarrollado en C y CUDA que soporta tanto ejecución por GPU como por CPU (con algunas demoras).

[dn]: https://github.com/pjreddie/darknet

# Inicialización
Para poder utilizar YOLOv3 es necesario primero descargar y compilar Darknet desde cero y en modo CPU con las siguientes instrucciones:

**Descarga:**
```bash
[nico@archlinux parcial_1]$ git clone https://github.com/pjreddie/darknet.git
Cloning into 'darknet'...
remote: Enumerating objects: 5934, done.
remote: Total 5934 (delta 0), reused 0 (delta 0), pack-reused 5934
Receiving objects: 100% (5934/5934), 6.35 MiB | 2.67 MiB/s, done.
Resolving deltas: 100% (3925/3925), done.
```

**Compilación:**

```bash
[nico@archlinux parcial_1]$ cd darknet
[nico@archlinux darknet]$ make
```
Última línea de compilación:
```bash
obj/captcha.o obj/lsd.o obj/super.o obj/art.o obj/tag.o obj/cifar.o obj/go.o obj/rnn.o obj/segmenter.o obj/regressor.o obj/classifier.o obj/coco.o obj/yolo.o obj/detector.o obj/nightmare.o obj/instance-segmenter.o obj/darknet.o libdarknet.a -o darknet -lm -pthread  libdarknet.a
```

> No se compiló con OpenCV para evitar demoras significativas que se miden en la magnitud de horas y no minutos/segundos.

**Verificación:**

```bash
[nico@archlinux darknet]$ ./darknet 
usage: ./darknet <function>
```

Compilado Darknet correctamente, solo resta descargar los pesos obtenidos de YOLOv3 con el siguiente comando:

**Descarga:**

```bash
[nico@archlinux darknet]$ wget https://pjreddie.com/media/files/yolov3.weights
--2021-05-24 17:19:30--  https://pjreddie.com/media/files/yolov3.weights
Loaded CA certificate '/etc/ssl/certs/ca-certificates.crt'
Resolving pjreddie.com (pjreddie.com)... 128.208.4.108
Connecting to pjreddie.com (pjreddie.com)|128.208.4.108|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 248007048 (237M) [application/octet-stream]
Saving to: 'yolov3.weights'

yolov3.weights       100%[====================>] 236.52M  2.65MB/s    in 89s     

2021-05-24 17:21:00 (2.67 MB/s) - 'yolov3.weights' saved [248007048/248007048]
```

**Verificación:**

Se ejecuta el detector con una de los samples incluidos solo para verificar su correcto funcionamiento de la siguiente manera:
```bash
[nico@archlinux darknet]$ ./darknet detect cfg/yolov3.cfg yolov3.weights data/dog.jpg
layer     filters    size              input                output
    0 conv     32  3 x 3 / 1   608 x 608 x   3   ->   608 x 608 x  32  0.639 BFLOPs
    1 conv     64  3 x 3 / 2   608 x 608 x  32   ->   304 x 304 x  64  3.407 BFLOPs
    2 conv     32  1 x 1 / 1   304 x 304 x  64   ->   304 x 304 x  32  0.379 BFLOPs
.
.
.
  105 conv    255  1 x 1 / 1    76 x  76 x 256   ->    76 x  76 x 255  0.754 BFLOPs
Loading weights from yolov3.weights...Done!
data/dog.jpg: Predicted in 30.380682 seconds.
dog: 100%
truck: 92%
bicycle: 99%
```

**Observaciones:**
- Se demoró unos 30 segundos la predicción por estar compilado en modo CPU.
- Se informa por consola la etiqueta de clase del objeto reconocido junto con el grado de certeza de las predicciones.
- Además, se guarda una nueva imagen *solo* con los contornos detectados y su etiqueta de clase.

![imagen procesada](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/img_demo/predictions.jpg)

# Testeo

Finalizada la incialización de YOLOv3 y de Darknet, se procederá a testear el sistema con 25 imágenes diferentes para observar su rendimiento. 

Estos son los resultados obtenidos:

## Imagen 01:

```bash
Enter Image Path: imagen01.jpg
data/to_predict/imagen01.jpg: Predicted in 29.885567 seconds.
tie: 93%
person: 100%
person: 100%
person: 99%
```
![img1](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen01.jpg)
![img1](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen01.jpg)

## Imagen 02:
```bash
Enter Image Path: imagen02.jpg
data/to_predict/imagen02.jpg: Predicted in 30.331623 seconds.
sports ball: 60%
person: 100%
person: 100%
person: 100%
person: 100%
person: 100%
person: 100%
person: 100%
person: 99%
person: 92%
person: 84%
person: 76%
person: 64%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen02.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen02.jpg)

## Imagen 03:
```bash
Enter Image Path: imagen03.jpg
imagen03.jpg: Predicted in 31.881146 seconds.
handbag: 95%
handbag: 86%
handbag: 67%
handbag: 56%
backpack: 97%
traffic light: 88%
traffic light: 87%
traffic light: 84%
traffic light: 84%
traffic light: 59%
car: 71%
car: 70%
car: 67%
person: 100%
person: 100%
person: 100%
person: 100%
person: 100%
person: 99%
person: 99%
person: 99%
person: 93%
person: 81%
person: 74%
person: 74%
person: 71%
person: 69%
person: 58%
person: 51%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen03.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen03.jpg)

## Imagen 04:
```bash
Enter Image Path: imagen04.jpg
imagen04.jpg: Predicted in 31.011223 seconds.
car: 100%
car: 99%
car: 98%
car: 92%
car: 86%
car: 68%
person: 100%
person: 100%
person: 99%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen04.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen04.jpg)

## Imagen 05:
```bash
Enter Image Path: imagen05.jpg
imagen05.jpg: Predicted in 30.160746 seconds.
sheep: 82%
horse: 98%
horse: 88%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen05.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen05.jpg)

## Imagen 06:
```bash
Enter Image Path: imagen06.jpg
imagen06.jpg: Predicted in 28.938016 seconds.
dog: 55%
bird: 100%
bird: 74%
bird: 59%
bird: 50%
person: 62%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen06.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen06.jpg)

## Imagen 07:
```bash
Enter Image Path: imagen07.jpg
imagen07.jpg: Predicted in 30.669087 seconds.
bear: 70%
bear: 55%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen07.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen07.jpg)

## Imagen 08:
```bash
Enter Image Path: imagen08.jpg
imagen08.jpg: Predicted in 30.241037 seconds.
cat: 89%
bench: 55%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen08.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen08.jpg)

## Imagen 09:
```bash
Enter Image Path: imagen09.jpg
imagen09.jpg: Predicted in 28.651153 seconds.
elephant: 99%
dog: 74%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen09.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen09.jpg)

## Imagen 10:
```bash
Enter Image Path: imagen10.jpg
imagen10.jpg: Predicted in 30.886841 seconds.
sports ball: 89%
person: 100%
person: 100%
person: 88%
person: 86%
person: 85%
person: 75%
person: 72%
person: 58%
person: 54%
person: 53%
person: 51%
person: 51%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen10.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen10.jpg)

## Imagen 11:
```bash
Enter Image Path: imagen11.jpg
imagen11.jpg: Predicted in 30.105740 seconds.
backpack: 75%
person: 99%
person: 79%
person: 61%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen11.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen11.jpg)

## Imagen 12:
```bash
Enter Image Path: imagen12.jpg
imagen12.jpg: Predicted in 29.638949 seconds.
sports ball: 93%
person: 100%
person: 100%
person: 100%
person: 100%
person: 100%
person: 100%
person: 99%
person: 97%
person: 92%
person: 87%
person: 73%
person: 64%
person: 52%
person: 52%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen12.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen12.jpg)

## Imagen 13:
```bash
Enter Image Path: imagen13.jpg
imagen13.jpg: Predicted in 31.164297 seconds.
person: 100%
person: 100%
person: 99%
person: 98%
person: 82%
person: 50%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen13.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen13.jpg)

## Imagen 14:
```bash
Enter Image Path: imagen14.jpeg
imagen14.jpeg: Predicted in 28.291737 seconds.
mouse: 100%
mouse: 90%
laptop: 87%
cup: 87%
```

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen14.jpeg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen14.jpg)

## Imagen 15:
```bash
Enter Image Path: imagen15.jpg
imagen15.jpg: Predicted in 31.162719 seconds.
traffic light: 51%
car: 100%
car: 100%
car: 100%
car: 99%
car: 99%
car: 98%
car: 89%
car: 79%
car: 59%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen15.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen15.jpg)

## Imagen 16:
```bash
Enter Image Path: imagen16.jpg
imagen16.jpg: Predicted in 31.395162 seconds.
person: 99%
person: 92%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen16.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen16.jpg)

## Imagen 17:
```bash
Enter Image Path: imagen17.jpg
imagen17.jpg: Predicted in 31.251893 seconds.
keyboard: 65%
laptop: 97%
tvmonitor: 81%
tvmonitor: 69%
chair: 100%
chair: 100%
chair: 99%
chair: 58%
chair: 56%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen17.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen17.jpg)

## Imagen 18:
```bash
Enter Image Path: imagen18.jpg
imagen18.jpg: Predicted in 29.161856 seconds.
mouse: 66%
mouse: 58%
tvmonitor: 99%
pottedplant: 100%
pottedplant: 98%
pottedplant: 98%
pottedplant: 80%
pottedplant: 67%
chair: 100%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen18.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen18.jpg)

## Imagen 19:
```bash
Enter Image Path: imagen19.jpg
imagen19.jpg: Predicted in 29.513150 seconds.
chair: 97%
orange: 68%
orange: 65%
apple: 70%
person: 100%
person: 93%
person: 91%
person: 70%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen19.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen19.jpg)

## Imagen 20:
```bash
Enter Image Path: imagen20.jpg
imagen20.jpg: Predicted in 30.619464 seconds.
bottle: 88%
bottle: 83%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen20.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen20.jpg)

## Imagen 21:
```bash
Enter Image Path: imagen21.jpg
imagen21.jpg: Predicted in 31.009748 seconds.
refrigerator: 89%
donut: 93%
donut: 84%
donut: 84%
donut: 83%
donut: 83%
donut: 82%
donut: 75%
donut: 74%
donut: 72%
donut: 68%
donut: 66%
donut: 57%
donut: 54%
donut: 54%
donut: 52%
donut: 51%
hot dog: 65%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen21.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen21.jpg)

## Imagen 22:
```bash
Enter Image Path: imagen22.jpg
imagen22.jpg: Predicted in 28.968046 seconds.
person: 100%
person: 100%
person: 99%
person: 99%
person: 94%
person: 87%
person: 86%
person: 80%
person: 71%
person: 70%
person: 53%
person: 52%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen22.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen22.jpg)

## Imagen 23:
```bash
Enter Image Path: imagen23.jpg
imagen23.jpg: Predicted in 32.250999 seconds.
person: 79%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen23.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen23.jpg)

## Imagen 24:
```bash
imagen24.jpg: Predicted in 29.079004 seconds.
chair: 97%
chair: 96%
cup: 64%
person: 100%
person: 98%
person: 79%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen24.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen24.jpg)

## Imagen 25:
```bash
Enter Image Path: imagen25.jpg
imagen25.jpg: Predicted in 29.554896 seconds.
person: 100%
person: 100%
person: 99%
person: 99%
person: 97%
```
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/to_predict/imagen25.jpg)
![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_1/predicted/imagen25.jpg)


# Conclusiones

### A tener en cuenta:
- Los creadores informan un rendimiento del 51.5% de precisión sobre [COCO][coco].
- [COCO][coco] es el dataset utilizado para entrenar YOLOv3 y posee +330K imágenes, 80 categorías de objetos y 91 categorías de *cosas* (definido así por sus creadores). Se puede observar que posee pocos animales.
- Las imágenes a procesar fueron tomadas de internet sin seguir ningún patrón, buscando alcanzar la mayor varianza posible y por ende no se especializan en un tópico determinado.

___

### Sobre los animales:
- Toma a los tres camellos (imagen 05) por dos caballos + una oveja con un nivel de confianza por encima del 82%.
- Considera que los canguros (imagen 07) son osos con una confianza del 70% y 55% para cada caso.
- Clasifica al zorro (imagen 08) como un gato con un 89% de confianza al respecto.
- Reconoce a todas las personas correctamente pero no identifica a ningun camello (imagen 25).

**Conclusión 1:** el modelo no solo no es efectivo para reconocer animales por confundirlos, sino que en algunos casos directamente no detecta en la imagen al contorno ocupado por un animal.
___

### Sobre la comida:
- Erróneamente etiqueta manzanas como naranjas (imagen 19) con un 68% de confianza.
- Clasifica un carrito de supermercado como silla teniendo tan solo un 3% de desconfianza.
- No reconoce paltas, tomates, cebollas ni bananas.
- No detecta al pepino ni la berenjena (imagen 20) cuando resaltan bastante dentro del carrito y confunde un frasco con una botella.
- Fracasa rotundamente en la detección de alimentos pertenecientes a la panadería (imagen 21); etiqueta a todos mal -hasta no reconoce algunas bandejas- a excepción del refrigerador.

**Conclusión 2:** el modelo tiene dificultades al detectar contornos de alimentos que están pegados o muy cerca entre sí. De aquí se desprende su incapacidad para reconocer verduras como pepino o berenjena así como también su confusión entre: manzanas por naranjas, duranznos por manzanas, baguettes con *hot-dogs* y facturas por donuts.

___

### Sobre las personas, autos y paisajes:
- Varios superhéroes de la imagen 17 no son detectados como personas.
- No detecta elementos sostenidos por personas cuando identifica a las personas como tales. Por ejemplo, en la imagen 13 no detecta la guitarra que sostiene Mick Jagger pero sí lo reconoce a él como persona.
- Reconoce muy bien a los individuos realizando diferentes deportes y hasta detecta la presencia de una pelota. 
- Identifica con mucha precisión autos aun así estos no se vean por completo.
- Detecta personas hasta en pinturas renacentistas con un elevado nivel de confianza.

**Conclusión 3:** es evidente la elevada precisión que tiene el modelo a la hora de identificar autos, personas y contornos de tamaño mediano-grande, sobretodo cuando estos no están superpuestos o muy cercanos. 

## Conclusión final:

El dataset utilizado para entrenar al modelo influye directamente sobre la forma en al que este realizará predicciones. Es por esto que se debe buscar un dataset lo más genérico posible para evitar así que el modelo se vuelva *overfitted* o demasiado específico en algunos tópicos y que fracase en otros.

[coco]: [mscoco.org/dataset/#overview](https://cocodataset.org/#home)
