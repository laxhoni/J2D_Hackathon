# J2D_Hackathon

## 1. Introducción
En primer lugar, describamos los datasets a unificar escogidos y razonamos el por qué de dicha decisión: Como dataset base hemos escogido el dataset "2019_censcomercialbcn_detall.csv" el cual hemos denominado como locals_dt. En él encontraremos la información más relevante sobre los comercios locales disponibles y ocupados en los diferentes distritos y barrios Barcelona en 2019. Por otra parte, nuestro dataset complementario se basa en una combinación de dos datasets los cuales almacenan el precio de alquiler mensual y venta de los distintos locales por barrios en Barcelona. Éstos son los siguientes: "loclloevolucio.csv" y "locveevolucio.csv" los cuales en conjunto serán nombrados como prices_dt.

Nuestro objetivo principal en esta primera parte del proyecto es preparar estos datos de manera que podamos identificar patrones significativos y tomar decisiones informadas para futuras expansiones de negocios. Todo ésto recogido en una aplicación que permita el influjo de comercios y consecuentemente, de clientes con el fin de potenciar el comercio local.


## 2. Prepocesamiento y depuración de datos
La fase de depuración y preprocesamiento de datos juega un papel crucial para garantizar la calidad y relevancia de la información que utilizaremos en nuestros análisis y clasificaciones. El conjunto de datos proporciona información valiosa sobre los locales, incluyendo detalles como la ubicación geográfica, el tipo de actividad y el distrito al que pertenecen. 
En nuestro caso hemos dividido la depuración de datos en dos procesos diferenciados: (2.1) Depuración previa a la unificación, en el cual hemos tratado los datasets por separado y hemos reducido las variables necesarias y (2.2) Depuración posterior a la unificación, donde hemos seleccionado las variables relevantes del dataset unificado para un fiable tratamiento de datos.

### 2.1. Depuración previa a la unificación
#### 2.1.1. Nuestro dataset base: locals_dt
En primer lugar limitaremos nuestro datasets a las variables necesarias para realizar un correcto preprocesamiento de datos:
* Nom_Local: Nombre del local.
* Nom_Via: Nombre de la vía donde se encuentra el local.
* Porta: Número de puerta del local.
* Codi_Districte: Código del distrito al que pertenece el local.
* Nom_Sector_Activitat: Nombre del sector de actividad al que pertenece el local.
* Codi_Barri: Código del barrio al que pertenece el local.
* Nom_Districte: Nombre del distrito al que pertenece el local.
* Nom_Activitat: Nombre de la actividad realizada en el local.
* Longitud: Coordenada de longitud donde se ubica el local.
* Latitud: Coordenada de latitud donde se ubica el local.

Seguidamente, suprimiremos las filas cuya información no esté detallada o en caso de que el nombre del local no esté definido. Al tener que trabajar con una muestra limitada estos datos no serás relevantes en el cómputo total de la muestra. En caso de poder aumentar nuestra muestra con datos actualizados podríamos explorar otras posibilidades para perder la menor información posible.

Por otra parte, crearemos una nueva columna 'isAvailability' la cual contenga True o False dependiendo de si la instancia del local está disponible o no. Para comprobarlo deberemos consultar la variable 'Nom_Sector_Activitat'. Por las restricciones propias de nuestros sistemas hemos decidido limitar nuestra muestra de locales disponibles a aquellos barrios que contengamos tanto información sobre precios de alquiler mensual como de venta.

Finalmente, agruparíamos en una misma columna 'addresses' la dirección del local concatenando las columnas 'Nom_Via' y 'Porta' y por último, reordenaremos y renombraremos las variables para una mejor visibilidad del dataset.

#### 2.1.2. Nuestro dataset complementario: prices_dt
En primer lugar castearemos las variables que contienen los valores de los precios por años a variables numéricas. Seguidamente unificaremos los dos datasets de precios de venta y alquiler y eliminaremos las variables duplicadas.

Finalmente, restringiremos nuestro dataset a los datos de los precios de un año en concreto: 2011 y seguidamente, suprimiremos las columnas innecesarias.

### 2.2. Depuración posterior a la unificación
Una vez realiza la depuración previa de los datasets unificaremos los dos datasets anteriores sobreescribiendo el dataset base: locals_dt a partir de nuestra columna común 'Codi_Barri' en locals_dt y 'ID_BARRIO' en prices_dt. Seguidamente eliminaremos éstas columnas. Ésto es debido a que únicamente las utilizaremos para que el precio de los locales concuerden según el barrio en el que se encuentran aunque nosotros analizaremos la muestra por distritos debido a la limitación de tiempo. 

*Disclaimer: En futuras implementaciones uno de los puntos clave será el de adaptar nuestros datasets a barrios y calles para una mayor precisión en las predicciones así como aumentar nuestros datos en la muestra utilizado, siendo además éstos actualizados puesto que los datos aportados son datos de años pasados.

Seguidamente, resetearemos el índice de nuestro dataset y añadiremos una columna tipo String que identifique de forma única los distintos locales existentes en nuestro dataset.

Finalmente, renombraremos las columnas tal que nuestro dataset final se componga de las siguientes variables:
* localID: Identificador único del local.
* localName: Nombre del local.
* address: Dirección del local.
* districtID: Identificador único del distrito al que pertenece el local.
* district: Nombre del distrito al que pertenece el local.
* isAvailability: Indicador de disponibilidad del local (puede ser binario, por ejemplo, disponible o no disponible).
* activityType: Tipo de actividad realizada en el local.
* totalDistrictStore: Número total de tiendas en el distrito.
* longitude: Coordenada de longitud donde se ubica el local.
* latitude: Coordenada de latitud donde se ubica el local.
* rentalPrice: Precio de alquiler del local.
* salePrice: Precio de venta del local.


## 3. Análisis de datos
Utilizando herramientas de visualización y técnicas estadísticas, exploraremos las características clave que definen la naturaleza de los locales comerciales y su entorno. A través de este análisis, buscamos revelar percepciones valiosas que respalden la toma de decisiones informada y proporcionen una visión clara de los factores que influyen en la dinámica de nuestro conjunto de datos.

### 3.1. Scatter plot: Relación entre logitud y latitud
* La extensión de los locales en activo en Barcelona es muy amplia. Este factor será un atenuante para el impacto de nuestro proyecto pues la competencia es la clave para situar un nuevo local en el mapa
* En determinadas coordenadas notamos una gran influencia de tipos de local según su actividad. Este hecho será crucial para nuestro estudio sobre donde posicionar un nuevo local estratégicamente.
  
### 3.2. Scatter plot: Comparación precios de alquiler y venta por distrito
* Como era de esperar, el precio de alquiler y venta está positivamente correlacionado, es decir, cuanto mayor sea el precio de alquiler, mayor será el de venta, y viceversa.
* Los distritos con mayor precio de alquiler son: 'Ciutat Vella', 'Sarrià-Sant Gervassi' y 'Eixample'.
* Los distritos con mayor precio de venta son: 'Sarrià-Sant Gervassi', 'Sant Marti', 'Ciutat Vella' y 'Eixample'.
* Existe una gran correlación entre el precio de alquiler/venta y el distrito. Hecho que deberemos de tener muy en cuenta para la posterior toma de decisiones.
  
### 3.3. Count plot: Visualización de la distribución de tipos de actividad
* Los tipos de negocio local más comunes son: Restaurants, Serveis a les empreses i oficines, Bars / CIBERCAFÈ, Vestir - Perruqueries
* Por tanto, éstos tienden a ser los más rentables en zonas de influecia, las cuales estudiaremos posteriormente


## 4. Muestreo de datos e implementación de la "Milla de oro"
### 4.1. Creación de una muestra
Debido a que nuestro dataset tiene un número muy elevado de instancias hemos decidido realizar el análisis de una muestra reducida con tal de probar el potencial de nuestro proyecto. 

Para ello, hemos seleccionado 3 de las actividades más influyentes en los distritos de Barcelona y hemos filtrado nuestro dataset por éstas.
Seguidamente hemos seleccionado un 30% de muestra de locales disponibles y un 70% de muestra de locales ocupados cuyo tipo de actividad es de una de las actividades filtradas.

### 4.2. Implementación de la Milla de Oro:

El análisis respalda la validez del concepto de "milla de oro" al revelar patrones que sugieren la existencia de áreas con una alta concentración de locales comerciales del mismo tipo. La estrategia de establecer un local cercano a lugares con un gran número de comercios similares muestra beneficios potenciales, ya que:

* Aumento de Visibilidad: La proximidad a una concentración de comercios similares aumenta la visibilidad de nuestro local entre clientes interesados en ese tipo de productos o servicios.

* Generación de Tráfico: La presencia de múltiples comercios del mismo tipo crea un flujo constante de clientes potenciales, lo que puede traducirse en un aumento en las ventas.

* Competencia y Colaboración: Estar en una "milla de oro" fomenta la competencia saludable y la colaboración entre negocios similares, lo que puede llevar a oportunidades de crecimiento mutuo.

* Sinergias Comerciales: La cercanía a otros comercios similares puede facilitar asociaciones estratégicas y sinergias comerciales, aprovechando la afluencia de clientes compartidos.

En resumen, la estrategia de establecer un local en una "milla de oro" emerge como una táctica efectiva respaldada por el análisis de datos, ofreciendo ventajas competitivas y oportunidades de crecimiento en el mercado local.

## 5. Conclusiones
