
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
- Predice las probabilidades de las dimensiones de cada contorno. 


IMAGEN






> El cálculo de las probabilidades intenta obtener el ancho y el alto del contorno en base a los centroides devueltos por kMeans.

Finalmente, cada contorno encontrado es pesado usando las dimensiones predictas y luego es asignado a una clase determinada que lo identifica con un objeto que YOLOv3 ya conoce.

Para más información revisar su [paper][link_paper].

[link_paper]: https://pjreddie.com/media/files/papers/YOLOv3.pdf

## Modo de uso

Debido a la falta de capacidad de cómputo, se optó por [Darknet][dn]. Un modelo open-source y pre-entrenado de red neuronal desarrollado en C y CUDA que soporta tanto ejecución por GPU como por CPU (con algunas demoras).

[dn]: https://github.com/pjreddie/darknet

# Inicialización
Para poder utilizar YOLOv3 es necesario primero descargar y compilar Darknet desde cero y en modo CPU con las siguientes instrucciones:

## Darknet

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

**Verificación:**

```bash
[nico@archlinux darknet]$ ./darknet 
usage: ./darknet <function>
```

## YOLOv3

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
Se ejecuta el detector con la imagen por defecto para verificar su correcto funcionamiento de la siguiente manera:
```bash
[nico@archlinux darknet]$ ./darknet detect cfg/yolov3.cfg yolov3.weights data/dog.jpg
layer     filters    size              input                output
    0 conv     32  3 x 3 / 1   608 x 608 x   3   ->   608 x 608 x  32  0.639 BFLOPs
    1 conv     64  3 x 3 / 2   608 x 608 x  32   ->   304 x 304 x  64  3.407 BFLOPs
    2 conv     32  1 x 1 / 1   304 x 304 x  64   ->   304 x 304 x  32  0.379 BFLOPs
.
.
.
  104 conv    256  3 x 3 / 1    76 x  76 x 128   ->    76 x  76 x 256  3.407 BFLOPs
  105 conv    255  1 x 1 / 1    76 x  76 x 256   ->    76 x  76 x 255  0.754 BFLOPs
  106 yolo
Loading weights from yolov3.weights...Done!
data/dog.jpg: Predicted in 30.380682 seconds.
dog: 100%
truck: 92%
bicycle: 99%
```
**Observaciones:**
- Se demoró unos 30 segundos la predicción por estar compilado en modo CPU.
- Se informa por consola el grado de certeza de las predicciones.
- Se muestran en la imagen los contornos detectados y asociados a un objeto.

IMAGEN PREDECIDA.

# Testeo

Finalizada la incialización de YOLOv3 y de Darknet, se procederá a testear el sistema con 20 imágenes diferentes para observar su rendimiento. Aquí se adjuntan algunas, si desea verlas todas puede hacerlo en el directorio
``/darknet/to_predict ``.

Imagen 1: 

Imagen 2:


Imagen 3:


