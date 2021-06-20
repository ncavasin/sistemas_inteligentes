# Parcial 2 - Continuación

Este documento es el análisis de los resultados obtenidos al ejecutar el código presente en la notebook [vgg16.ipynb](https://github.com/ncavasin/sistemas_inteligentes/blob/main/parcial_2/vgg/vgg16.ipynb).


A continuación se muestran las imágenes entregadas al modelo junto con la etiqueta y el grado de certeza que este les asignó.

## Una por una:

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/01_tomato.jpg)

  

> Clasificación: hip (cadera) 50.60%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/02_glasses.jpg)

  

> Clasificación: sunglasses (anteojos de sol) 48.85%

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/03_car.jpg)

  

> Clasificación: sports_car (auto_deportivo) 83.92%

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/04_person.jpg)

  

> Clasificación: torch (linterna) 48.57%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/05_house.jpg)

  

> Clasificación: boathouse (casa para guardar botes) 13.59%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/06_waves.jpg)

  

> Clasificación: spindle (*algo que gira*) 15.24%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/07_tower_bridge.jpg)

  

> Clasificación: suspension_bridge (puente colgante) 68.60%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/08_rhino.jpg)

  

> Clasificación: bison (bisonte) 94.79%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/09_downey_jr.jpg)

  

> Clasificación: suit (traje) 46.59%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/11_edsac_computer.jpg)

  

> Clasificación: turnstile (puerta giratoria) 50.11%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/12_notebook.png)

  

> Clasificación: notebook 76.99%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/13_jewels.jpg)

  
  

> Clasificación: band_aid (curita) 77.970%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/14_bike.jpg)

  

> Clasificación: mountain_bike (bicicleta de montaña) 28.02%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/15_robocop.png)

  

> Clasificación: fountain (fuente) 23.14%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/16_barbell.jpg)

  

> Clasificación: barbell (barra) 99.71%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/17_dog.jpg)

  

> Clasificación: golden_retriever 96.76%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/18_seat.jpeg)

  

> Clasificación: barber_chair (silla de peluquero) 48.17%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/19_himalaya.jpg)

  

> Clasificación: alp (alpe) 96.49%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/20_fighter_jet.jpg)

  

> Clasificación: warplane (avión de guerra) 98.38%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/21_songbird.jpg)

  

> Clasificación: house_finch (pinzón de casa, pájaro) 60.47%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/22_microphone.jpg)

  

> Clasificación: microphone (micrófono) 99.74%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/23_door.jpg)

  

> Clasificación: entertainment_center (centro de entretenimiento) 32.79%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/24_sushi.jpg)

  

> Clasificación: conch (concha de mar) 14.62%.

  

![](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/25_milanesa.jpg)

  

> Clasificación: corn (choclo) 50.50%.

# Conclusiones

### Sobre los animales:
- El rinoceronte fué erroneamente clasificado como Bisón con un grado de certeza del 94.79%.
- El golden retriever fué identificado correctamente con un 96.76% de seguridad.
- El pájaro fué identificado, llamativamente, como un pinzón de casa con un 60% de precisión. Digo llamativamente porque la imágen fué descargada sin saber cual era su especie y que lo clasifique de tal manera es sorprendente (desconozco si el modelo está equivocado o no).

**Conclusión 1:** el modelo *parece* ser confiable para clasificar animales.

- La confusión entre un rinoceronte y un bisón es "aceptable" ya que son anmales de características similares.
- Sería apresurado decir que *es* confiable, y por eso digo *parece*, ya que las imágenes de testing solo tenían 3 animales. 
___

### Sobre la comida:
- Es inevitable arrancar al revés y resaltar la *extraña* confusión que realiza el modelo al clasificar una milanesa con papas fritas como choclo (corn) y que además lo haga con un grado del 50.5% de certeza.
- Erróneamente se le asigna la etiqueta *hip* (cadera) al tomate con un 50.6% de precisión.
- El modelo etiqueta al sushi *¿correctamente?* como *conch* (concha de mar) con una certeza del 14.62% (ver predicciones de menor certeza en el colab).

**Conclusión 2:** dentro de las 3 imágenes de comida utilizadas, el modelo tiene *muchas* dificultades para clasificarlas correctamente (no acertó ninguna) y no lo consideraría nada preciso en *este* caso particular.
___

### Sobre los vehículos:
- Etiqueta correctamente, con 83.92% de precisión, al auto deportivo.
- La bicicleta se identifica bien como tal pero con una certeza baja, 28.02%.
- El jet es clasificado de manera adecuada con una seguridad total (98.38%).

**Conclusión 3:** es evidente la elevada precisión que tiene el modelo a la hora de identificar vehículos, al menos en este pequeño lote de imágenes.

---

### Sobre las personas:

- Diego Maradona es etiquetado como una linterna (torch) con un 48.57% de certeza.
- A Robert Downey Jr. se le asigna *¿correctamente?* el label traje (suit) con un 46.59% de precisión.

**Conclusión 4:** el modelo *pareciera* no reconocer personas como tal, sino que identifica las cosas a las que una persona puede parecerse (caso 1) o estar utilizando (caso 2).

---
### Sobre los paisajes:
- Tower Bridge es identificado correctamente como puente colgante. 
- El pico del Himalaya es etiquetado también de manera correcta como un alpe (alp).

**Conclusión 5:** vgg16 es preciso en el reconocimiento de paisajes u objetos de gran tamaño.

---

### Sobre los objetos / imágenes restantes :

- Identifica correctamente al par de  anteojos aunque los etiquete como *anteojos de sol* con un 48.85% de certeza.
- La casa es clasificada como casa para guardar botes (boathouse) con una baja confianza (13.59%).
- La notebook y la barra son correctamente identificados con un alto umbral de certeza (76.99%, y 99.71%).
- Los anillos son identificados erróneamente como curitas (band_aid) con un  77.97% de seguridad.
- Robocop es, para el modelo, una fuente (fountain) con un 23.14% probabilidades.
- El resto de las imágenes se clasifican de manera correcta (puerta, micrófono, sillón).
- Es *bastante sorprendente* que, a pesar de que no los etiquete bien per se, el modelo identifique que la imagen de la [ola](https://raw.githubusercontent.com/ncavasin/sistemas_inteligentes/main/parcial_2/vgg/images/06_waves.jpg) sea [*algo que gira*](https://www.google.com/search?q=spindle&client=firefox-b-d&sxsrf=ALeKk00GOIw_ozZByQ_dP_eQVQpdsDOllQ:1624219179834&source=lnms&tbm=isch&sa=X&ved=2ahUKEwj82ID6_6bxAhUNrJUCHSAVDEAQ_AUoAXoECAEQAw&biw=1366&bih=653) (spindle) y que la imagen de la computadora [EDSAC](https://en.wikipedia.org/wiki/EDSAC) sea etiquetada como [puerta giratoria](https://www.google.com/search?q=turnstile&client=firefox-b-d&sxsrf=ALeKk02ugr7uPkb8UlUsZkOBPbADxojvyQ:1624219138329&source=lnms&tbm=isch&sa=X&ved=2ahUKEwi56Jvm_6bxAhX-qJUCHdIzDIIQ_AUoAXoECAEQAw&biw=1366&bih=653#imgrc=X9P-4AC1OPaYcM) (turnstile) con un 15.24% y un 50.11% de seguridad respectivamente.

**Conclusión 6:** se observa poca regularidad en la precisión de las imágenes mencionadas anteriormente. Los anillos se confunden mucho con curitas mientras que el micrófono es perfectamente clasificado como tal.

## Conclusión final:

Queda en evidencia lo desbalanceado que se encuentra el modelo a la hora de clasificar las imagenes que se le entregaron. 

Por momento es muy preciso (caso auto deportivo, Tower Bridge, barra, golden retriever) mientras que por otros es muy impreciso (caso Diego Maradona, milanesa con papas fritas, tomate).

Lo que sí sorprende es que posea "cierta inteligencia" ¿o suerte? a la hora de clasificar el caso de las olas y el de la computadora EDSAC.

VGG16, a pesar de haberse descargado ya pre-entrenado, es por su arquitectura más costoso de entrenar y menos preciso que el seleccionado para el parcial 1 (YOLOv3). Además, es *algo viejo* en términos de estado del arte (año 2014/15) ya que el reconocimiento de objetos año a año produce nuevas innovaciones que dejan obsoletos (o muy atrás en términos de precisión) a modelos que antes *eran* el estado del arte.
