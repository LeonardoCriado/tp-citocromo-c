# ¿En qué se parecen una mosca y una gallina?

**Trabajo práctico — Introducción a la Bioinformática**  
**Autor:** Leonardo Criado  
**Fecha:** octubre de 2026  
**Consigna original:** [En qué se parecen una gallina y una mosca](https://github.com/AJVelezRueda/Introduccion_a_la_Bioinformatica/blob/master/Teorico_Practicos/Bioinform%C3%A1tica_Primeros_pasos/En_que_se_parece_una_mosca_y_una_gallina.md), Dra. Ana Julia Velez Rueda (CC BY-SA 4.0).

![Licencia: CC BY-SA 4.0](https://img.shields.io/badge/Licencia-CC%20BY--SA%204.0-lightgrey.svg)

---

## Resumen

El citocromo c es una proteína pequeña, esencial para la respiración aeróbica y muy conservada en la evolución. En este trabajo se compararon las secuencias de citocromo c de nueve organismos —cinco mamíferos, un ave, un anfibio, un pez y un insecto— para responder una pregunta sencilla: ¿en qué se parecen una mosca y una gallina? Las secuencias se obtuvieron de NCBI Protein, se alinearon con Clustal Omega y se analizaron mediante porcentajes de identidad, conservación por posición y un árbol filogenético con bootstrap.

La identidad entre pares varió entre 65,0 % y 100 %. Los tres primates (humano, chimpancé y gorila) resultaron idénticos en las posiciones comparables; el caballo y el perro compartieron el 95,2 %; la gallina y el humano, el 87,6 %; y la mosca fue el organismo más divergente (66,0 % respecto del humano). El árbol recuperó con buen soporte los grupos cercanos (primates, humano-chimpancé y caballo-perro) y ubicó a la mosca como grupo externo, pero los nodos profundos —la posición relativa de gallina, rana y salmón— tuvieron soporte bajo. Esa falta de resolución no invalida el ejercicio: muestra que una proteína corta y muy conservada contiene poca señal filogenética y que la elección de secuencias ortólogas es tan importante como el método de análisis.

**Palabras clave:** citocromo c, alineamiento múltiple, filogenia molecular, evolución, Clustal Omega, bootstrap.

---

## 1. Introducción

El citocromo c es una proteína de unos 104–105 aminoácidos que se encuentra en la mayoría de los organismos eucariotas. Cumple una función central en la cadena respiratoria: transporta electrones entre el complejo III (citocromo bc1) y el complejo IV (citocromo c oxidasa) en la membrana mitocondrial interna. Su centro activo es un grupo hemo unido covalentemente a la proteína a través de un motivo conservado **CXXCH** (dos cisteínas y una histidina), y el hierro del hemo se coordina además con una metionina axial. Estas restricciones funcionales hacen que el citocromo c sea una de las proteínas más conservadas de la evolución.

La comparación de secuencias es una de las herramientas centrales de la bioinformática evolutiva. Si dos organismos comparten un ancestro común reciente, sus secuencias deberían parecerse más que las de organismos con un ancestro más lejano; las diferencias se acumulan con el tiempo y con la tasa de mutación, mientras que la selección purificadora conserva los residuos críticos para la función. A partir de un alineamiento se puede cuantificar la similitud, identificar residuos conservados y reconstruir un árbol filogenético que represente una hipótesis sobre el orden de divergencia de los linajes.

En este trabajo se usan las nueve secuencias propuestas en la consigna para poner a prueba ese razonamiento: descargar secuencias, alinearlas, medir su similitud, inferir un árbol y discutir qué nos dice —y qué no nos dice— sobre la evolución de los organismos.

### 1.1 Objetivos

1. Obtener y organizar las secuencias de citocromo c de nueve organismos en formato FASTA.
2. Completar la tabla de nombres taxonómicos y nombres comunes.
3. Alinear las secuencias con Clustal Omega e interpretar el alineamiento.
4. Calcular la identidad entre pares y la conservación por posición.
5. Reconstruir e interpretar un árbol filogenético.
6. Responder las preguntas de la consigna y discutir las limitaciones del análisis.

---

## 2. Materiales y métodos

### 2.1 Secuencias

Las secuencias se descargaron de la base de datos **NCBI Protein** el 21 de octubre de 2026, usando los números de acceso indicados en la consigna. Se verificó que cada registro correspondiera al organismo y a la proteína esperados. La Tabla 1 resume la información.

Las secuencias se guardaron en formato **FASTA**, en el que cada registro tiene una línea de encabezado que comienza con `>` (con el identificador, el nombre de la proteína y el organismo) seguida por una o más líneas con la secuencia de aminoácidos. Este formato es texto plano, estándar y fácil de procesar con programas de bioinformática.

### 2.2 Alineamiento

El alineamiento múltiple se realizó con **Clustal Omega** a través del servidor del European Bioinformatics Institute (EBI), con los parámetros predeterminados para proteínas. Clustal Omega es un alineador progresivo que usa perfiles de HMM y guías de alineamiento para manejar conjuntos de secuencias de tamaño moderado. El alineamiento resultante tiene **107 columnas**: dos columnas son gaps en todas las secuencias de vertebrados (debido a una extensión N-terminal de la mosca) y 105 columnas contienen residuos en al menos una secuencia.

Para el análisis se usaron las siguientes convenciones:

- **Identidad:** porcentaje de posiciones iguales sobre posiciones comparables, es decir, excluyendo las columnas con gap en cualquiera de las dos secuencias.
- **Conservación por columna:** fracción de secuencias que comparten el residuo más frecuente (residuo modal), excluyendo gaps.
- **Sitios parsimoniosamente informativos:** columnas con al menos dos estados, cada uno presente en al menos dos secuencias.
- **Gaps:** se trataron como posiciones no comparables, no como aminoácidos.

### 2.3 Análisis filogenético

Se reconstruyó un árbol de distancias por **neighbor joining (NJ)** sobre el alineamiento, con corrección de Poisson para múltiples sustituciones por sitio (`d = -ln(1 - p)`, donde `p` es la distancia de identidad). Se realizaron **1000 réplicas de bootstrap** remuestreando columnas del alineamiento, y se calculó el soporte de cada clado como el porcentaje de réplicas que lo contenían. La topología se comparó con el árbol filogenético que provee Clustal Omega; ambos coinciden.

Como control adicional se realizó un análisis de máxima verosimilitud con **IQ-TREE 2** (modelo WAG+G4, elegido por ModelFinder). Ese análisis recuperó los grupos cercanos, pero mostró inestabilidad en los nodos profundos e incluso una agrupación perro-mosca, un patrón compatible con atracción de ramas largas. Por eso el árbol principal que se presenta es el de distancias, que coincide con el de Clustal Omega, y se interpreta con cautela.

### 2.4 Figuras y reproducibilidad

Las figuras se generaron con Python (Biopython, Matplotlib y NumPy) a partir del alineamiento y de la matriz de identidad. No se editaron secuencias ni posiciones manualmente. Los números de acceso de NCBI permiten reproducir la descarga; el alineamiento completo se incluye en el Anexo.

---

## 3. Resultados

### 3.1 Secuencias y nombres comunes

**Tabla 1.** Secuencias de citocromo c utilizadas. La identidad y las diferencias se calcularon respecto del citocromo c humano sobre posiciones comparables (sin gaps).

| Accesión | Especie | Nombre común | Grupo | Longitud (aa) | Identidad con humano (%) | Diferencias |
|---|---|---|---|---:|---:|---:|
| NP_061820.1 | *Homo sapiens* | Humano | Primates | 105 | 100,0 | 0 |
| NP_001065289.1 | *Pan troglodytes* | Chimpancé común | Primates | 105 | 100,0 | 0 |
| AEP27192.1 | *Gorilla gorilla* | Gorila occidental | Primates | 105 | 100,0 | 0 |
| NP_001157486.1 | *Equus caballus* | Caballo doméstico | Mamífero no primate | 105 | 89,5 | 11 |
| NP_001183974.1 | *Canis lupus familiaris* | Perro doméstico | Mamífero no primate | 105 | 89,5 | 11 |
| NP_001072946.1 | *Gallus gallus* | Gallina doméstica | Ave | 105 | 87,6 | 13 |
| NP_001086101.1 | *Xenopus laevis* | Rana africana de uñas | Anfibio | 105 | 84,8 | 16 |
| XP_024245566.1 | *Oncorhynchus tshawytscha* | Salmón chinook | Pez | 104 | 80,8 | 20 |
| NP_477164.1 | *Drosophila melanogaster* | Mosca de la fruta | Insecto | 105 | 66,0 | 35 |

*Nota:* el número de posiciones comparables es 105 para la mayoría de los pares, 104 para el salmón y 103 para la mosca, debido a gaps en el alineamiento. Por eso las diferencias absolutas no siempre se pueden comparar directamente entre filas; el porcentaje sí.

### 3.2 El alineamiento

![Alineamiento múltiple del citocromo c en nueve organismos](figuras/fig1_alineamiento.png)

**Figura 1.** Alineamiento múltiple de las nueve secuencias de citocromo c generado con Clustal Omega (107 columnas). Cada residuo se colorea según sus propiedades fisicoquímicas (leyenda inferior). El recuadro negro marca el motivo **CXXCH** de unión al hemo (columnas 17–21). El gráfico inferior muestra la conservación por columna: la altura es la fracción de secuencias que comparten el residuo modal. Se indica el número de columnas invariantes sobre las 105 columnas con residuos.

El alineamiento muestra una conservación notable. De las 105 columnas con residuos, **64 (61,0 %) son invariantes** y **41 (39,0 %) son variables**. Entre las variables, solo **15 (14,3 %)** son parsimoniosamente informativas; las otras 26 son singletons o autapomorfías, es decir, cambios presentes en una sola secuencia o en un solo linaje. Esa asimetría es clave para entender por qué el árbol tiene poca resolución en los nodos profundos.

El motivo **CXXCH** (Cys15, Cys18 e His19 en la numeración del humano, incluyendo la metionina iniciadora) está conservado en las nueve secuencias, y la metionina axial (Met81) también es invariante. La región central, que forma parte del plegamiento y del sitio de unión al hemo, concentra las posiciones conservadas. Las diferencias se distribuyen en los extremos N- y C-terminal y en posiciones dispersas, muchas de ellas con cambios conservadores.

La mosca es la secuencia más divergente: tiene dos residuos extra en el extremo N-terminal, un repertorio de sustituciones propio y gaps en el extremo C-terminal. El salmón también presenta un gap en el extremo C-terminal. Estas diferencias explican por qué la mosca aparece con una rama mucho más larga en el árbol.

### 3.3 Identidad entre pares

![Identidad entre pares y similitud con el humano](figuras/fig2_identidad.png)

**Figura 2.** (A) Matriz de identidad de secuencia (%) entre todos los pares de secuencias. (B) Identidad de cada secuencia con el citocromo c humano. Los valores se calcularon sobre posiciones comparables, excluyendo gaps.

Los resultados principales son:

- **Primates:** humano, chimpancé y gorila son **100 % idénticos** en las posiciones comparables (0 diferencias). Es el grupo más cohesionado.
- **Mamíferos no primates:** caballo y perro comparten el **95,2 %** (5 diferencias). Son el par más parecido después de los primates.
- **Aves y anfibios:** la gallina y la rana comparten el **90,5 %** (10 diferencias); la rana y el salmón, el **88,5 %** (12 diferencias); la gallina y el salmón, el **87,5 %** (13 diferencias).
- **Humano y gallina:** **87,6 %** de identidad (13 diferencias). Es una similitud alta para organismos separados por más de 300 millones de años.
- **Mosca:** es el organismo más divergente. Su identidad con el humano es **66,0 %** (35 diferencias), y con el resto de los organismos oscila entre 65,0 % y 69,9 %.

La identidad no es uniforme: hay pares muy parecidos (primates, caballo-perro) y un par claramente separado (mosca). Esa estructura ya sugiere la existencia de grupos.

### 3.4 El árbol filogenético

![Árbol filogenético inferido a partir del citocromo c](figuras/fig3_arbol.png)

**Figura 3.** Árbol filogenético inferido a partir del alineamiento de citocromo c. Se muestra el árbol de Clustal Omega (no enraizado, orientado con la mosca como grupo externo) con longitudes de rama en sustituciones por sitio. Los números en los nodos son porcentajes de bootstrap (1000 réplicas) de un análisis de distancias NJ con corrección de Poisson sobre el mismo alineamiento. La topología coincide con la del árbol de Clustal Omega.

El árbol recupera los siguientes grupos, con los soportes de bootstrap indicados:

- **Primates** (humano, chimpancé, gorila): **100 % de bootstrap**.
- **Humano y chimpancé**: **91 %**.
- **Caballo y perro**: **66 %**.
- **Mamíferos** (primates + caballo + perro): **60 %**.
- **Gallina, rana y salmón**: **49 %**.
- **Rana y salmón**: **54 %**.
- **Mosca** como grupo externo: **100 %** (el clado que agrupa a todos los demás).

El soporte es alto para los grupos cercanos y para la posición de la mosca como grupo externo, pero bajo para los nodos que definen las relaciones entre mamíferos, aves, anfibios y peces. En otras palabras: el árbol es confiable para decir que el humano, el chimpancé y el gorila están muy emparentados, y que la mosca es el linaje más alejado; no es confiable para resolver el orden exacto de divergencia entre gallina, rana y salmón.

---

## 4. Discusión: respuestas a las preguntas de la consigna

### 4.1 ¿Cuán sencillo será alinear dos o más secuencias a mano? ¿Cuánto influirán el número de secuencias, su longitud y su similitud?

Alinear dos secuencias cortas y muy parecidas a mano es posible, pero subjetivo. A medida que aumentan el número de secuencias, su longitud y su divergencia, el problema se vuelve inmanejable:

- **Número de secuencias:** las comparaciones crecen como `n(n-1)/2`. Con 9 secuencias hay 36 pares; con 20, 190. Además, hay que decidir un alineamiento global que sea consistente entre todos los pares, no solo entre dos.
- **Longitud:** cada posición adicional agrega posibilidades de gaps y de correspondencias alternativas. En secuencias largas, un desplazamiento de un solo residuo puede cambiar todo el alineamiento.
- **Similitud:** cuanto menor es la identidad, más ambiguo es decidir qué residuo corresponde a cuál. Con muchas sustituciones e indels, dos alineamientos distintos pueden ser casi igual de plausibles.
- **Gaps:** en regiones con inserciones o deleciones, la posición de los gaps es una hipótesis, no un dato. Un alineamiento manual tiende a favorecer la solución que “se ve bien”, no necesariamente la más probable.

Para nueve secuencias de ~105 aminoácidos con un 39 % de posiciones variables, el alineamiento manual sería lento, difícil de reproducir y probablemente inconsistente. Por eso se usan algoritmos: programación dinámica para pares de secuencias (Needleman-Wunsch, Smith-Waterman) y alineamiento múltiple progresivo o iterativo (Clustal Omega, MAFFT, MUSCLE). Aun así, el alineamiento es una hipótesis y conviene evaluar su calidad y su sensibilidad a los parámetros.

### 4.2 ¿Son parecidos los citocromos c de humano y gallo?

Sí, mucho. Comparten el **87,6 %** de las posiciones comparables: 92 aminoácidos idénticos y solo 13 diferencias sobre 105 posiciones. La similitud es especialmente alta en el núcleo funcional: el motivo CXXCH y la metionina axial están conservados, y muchas de las diferencias son sustituciones conservadoras en regiones periféricas.

Que dos organismos separados por más de 300 millones de años de evolución conserven casi el 88 % de una proteína es una evidencia fuerte de **constricción funcional**: la mayor parte de los cambios que alterarían la función fueron eliminados por selección purificadora. El citocromo c es, de hecho, uno de los ejemplos clásicos de proteína extremadamente conservada.

### 4.3 ¿Qué teorías subyacen a este análisis?

El análisis se apoya en varias ideas centrales de la biología evolutiva:

- **Descendencia común:** los organismos comparten ancestros; las secuencias homólogas descienden de una secuencia ancestral.
- **Evolución molecular:** las moléculas también documentan la historia evolutiva (Zuckerkandl y Pauling). Los cambios en el ADN y en las proteínas se acumulan a lo largo del tiempo.
- **Neutralismo y selección:** muchas sustituciones son neutras o casi neutras (teoría neutralista de Kimura), pero los residuos importantes para la función están bajo selección purificadora.
- **Homología, ortología y paralogía:** para comparar especies hay que usar **ortólogos** (genes que divergen por especiación), no parálogos (genes que divergen por duplicación). Un parálogo puede tener una historia evolutiva distinta y distorsionar el árbol.
- **Reloj molecular y filogenia:** la acumulación de cambios se usa para inferir relaciones, aunque las tasas varían entre linajes y entre genes.
- **Inferencia estadística:** un árbol filogenético es una hipótesis estimada a partir de los datos, no una observación directa; por eso se usan métodos como parsimonia, máxima verosimilitud, distancias y bootstrap.

### 4.4 ¿Cómo nos ayuda la evolución a explicar sus similitudes y diferencias?

La evolución explica ambos lados de la comparación:

- **Similitudes:** si dos especies comparten un ancestro reciente, heredan la misma secuencia ancestral y acumulan pocas diferencias. Por eso el humano, el chimpancé y el gorila son idénticos, y el caballo y el perro son muy parecidos.
- **Diferencias:** cuanto más tiempo pasó desde el ancestro común y mayor es la tasa de mutación, más cambios se acumulan. La mosca, separada de los vertebrados por cientos de millones de años, es la más divergente.
- **Conservación:** los residuos que cumplen una función crítica —unir el hemo, coordinar el hierro, interactuar con las enzimas de la cadena respiratoria— cambian muy poco porque las mutaciones que los afectan suelen ser deletéreas. Las regiones con menos restricciones funcionales acumulan más cambios.
- **Ritmos distintos:** no todos los linajes ni todos los genes evolucionan a la misma velocidad. Las isoformas testiculares o las duplicaciones génicas pueden evolucionar más rápido y no reflejar la filogenia de las especies.

En conjunto, el patrón de similitudes y diferencias es exactamente lo que se espera de un proceso de **descendencia con modificación** y de **selección** sobre una proteína esencial.

### 4.5 ¿Qué indican los colores?

En la vista coloreada de Clustal Omega (esquema tipo Clustal X), los colores no indican identidad exacta sino **propiedades fisicoquímicas** de los aminoácidos. Por ejemplo, los hidrofóbicos suelen colorearse en amarillo, los ácidos en rojo, los básicos en azul y los polares sin carga en verde. La paleta exacta puede variar entre versiones y programas.

Por eso, dos residuos del mismo color son **similares**, no necesariamente iguales: el color ayuda a ver sustituciones conservadoras (un ácido por otro ácido, un hidrofóbico por otro hidrofóbico) y a detectar regiones donde la composición se mantiene aunque la identidad no sea del 100 %. En la Figura 1 se usó una leyenda explícita con los grupos: hidrofóbicos, glicina/prolina, ácidos, básicos, polares sin carga y gaps.

### 4.6 ¿Qué indican el guión (`-`), los dos puntos (`:`) y el asterisco (`*`)?

En la línea de consenso de un alineamiento tipo Clustal:

- `*` (**asterisco**): todos los residuos de la columna son **idénticos**. Es una posición 100 % conservada.
- `:` (**dos puntos**): todos los residuos de la columna son **similares** según la matriz de sustitución; se trata de una sustitución conservadora (por ejemplo, un aminoácido ácido por otro ácido).
- `.` (**punto**): sustitución **semi-conservadora** o similitud débil. La consigna no lo menciona, pero aparece en muchos alineamientos.
- `-` (**guión**): **gap**, es decir, una posición que falta en esa secuencia respecto de las demás. No es un aminoácido: representa una inserción/deleción (indel) o una posición que el alineador decidió no asignar. Los gaps no se cuentan como diferencias en los porcentajes de identidad sobre posiciones comparables.

Conviene recordar que un gap puede reflejar un evento evolutivo real (una deleción) o una decisión del alineador; no siempre es un dato biológico directo.

### 4.7 A simple vista, ¿se conserva la secuencia del citocromo c en los organismos?

Sí, se conserva mucho. El alineamiento muestra largas franjas de residuos idénticos, especialmente en la región central. De las 105 columnas con residuos, **64 (61,0 %) son invariantes**; el motivo CXXCH y la metionina axial están conservados en las nueve secuencias. Las diferencias se concentran en los extremos y en posiciones dispersas, y muchas son cambios conservadores.

La conservación es menor en la mosca, que tiene una secuencia más divergente y varios gaps, y en las isoformas parálogas que se discuten en la sección 4.12. Aun así, el núcleo funcional se reconoce en todas las secuencias: eso es lo que permite alinearlas y compararlas.

### 4.8 ¿Todos los organismos se asemejan por igual al resto, o se pueden identificar grupos de mayor similitud? ¿Tienen sentido?

No se asemejan por igual. Se identifican grupos claros:

- **Primates:** humano, chimpancé y gorila, con 100 % de identidad.
- **Mamíferos:** primates + caballo + perro; el caballo y el perro comparten el 95,2 %.
- **Vertebrados no mamíferos:** gallina, rana y salmón, con identidades del 87–90 % entre sí y del 80–90 % con los mamíferos.
- **Insecto:** la mosca, con 65–70 % de identidad respecto de los vertebrados.

Los grupos más cercanos tienen sentido biológico: los primates comparten un ancestro común reciente, y el caballo y el perro son mamíferos estrechamente emparentados. El grupo gallina-rana-salmón es más problemático: por la taxonomía, la gallina (amniota) debería estar más cerca de los mamíferos que la rana (anfibio), y el salmón (pez) debería ser el grupo externo de los tetrápodos. El árbol no recupera ese orden, probablemente porque la señal de esta proteína es insuficiente y porque las secuencias de rana y mosca usadas en la consigna son isoformas parálogas (sección 4.12).

### 4.9 ¿Qué evidencias nos aportaría este análisis, a la luz de la evolución?

Este análisis aporta evidencia **molecular** independiente de la anatomía y de los fósiles:

1. **Patrón jerárquico de similitud:** los organismos se agrupan en niveles anidados (primates, mamíferos, vertebrados; insecto como grupo externo), como se espera de un proceso de ramificación.
2. **Conservación de residuos funcionales:** el motivo CXXCH y la metionina axial se mantienen, lo que refleja selección purificadora sobre la función.
3. **Congruencia con la taxonomía:** los grupos cercanos del árbol coinciden con la clasificación biológica; la mosca aparece como el linaje más divergente.
4. **Acumulación de diferencias con el tiempo:** las especies más alejadas tienen más diferencias, aunque la relación no es perfectamente lineal porque las tasas varían.

Ahora bien, una sola proteína corta no alcanza para reconstruir el árbol de la vida. La evidencia es fuerte para los grupos cercanos y para la posición de la mosca, pero débil para los nodos profundos. Para resolverlos harían falta más genes, genomas completos, modelos más realistas y una evaluación cuidadosa de ortología.

### 4.10 A juzgar por los organismos participantes, ¿cuáles deberían estar más agrupados en el árbol?

Según la taxonomía y la filogenia aceptada de los organismos incluidos:

- **Humano, chimpancé y gorila** deberían formar un grupo (primates).
- **Caballo y perro** deberían formar un grupo (mamíferos no primates), y ambos deberían agruparse con los primates.
- **Gallina** (ave) debería ser el linaje hermano de los mamíferos dentro de los amniotas.
- **Rana** (anfibio) debería ser el grupo externo de los amniotas (mamíferos + aves).
- **Salmón** (pez) debería ser el grupo externo de los tetrápodos.
- **Mosca** (insecto) debería ser el grupo externo de todos los vertebrados.

El árbol esperado, en forma esquemática, es:

```text
Mosca (insecto)
└── Salmón (pez)
    └── Rana (anfibio)
        └── Gallina (ave)
            └── Mamíferos
                ├── Caballo
                │   └── Perro
                └── Gorila
                    └── Humano
                        └── Chimpancé
```


### 4.11 Observemos el árbol. ¿Concuerda con lo esperado? ¿De qué organismos son los citocromos c más parecidos? ¿Cómo se explica?

**Concuerda parcialmente.** El árbol recupera:

- el grupo de los **primates** (100 % de bootstrap);
- el par **humano-chimpancé** (91 %);
- el par **caballo-perro** (66 %);
- el grupo de los **mamíferos** (60 %, soporte débil);
- la **mosca** como grupo externo (100 %).

No recupera, en cambio, la posición esperada de gallina, rana y salmón: el árbol inferido agrupa a la gallina con la rana y el salmón (49 % de bootstrap) y a la rana con el salmón (54 %), en lugar de ubicar a la gallina como hermana de los mamíferos, a la rana como grupo externo de los amniotas y al salmón como grupo externo de los tetrápodos. Los nodos profundos tienen soporte bajo, así que la topología no es robusta.

Los citocromos c **más parecidos** son:

1. **Humano, chimpancé y gorila:** 100 % de identidad, 0 diferencias.
2. **Caballo y perro:** 95,2 %, 5 diferencias.
3. **Gallina y rana:** 90,5 %, 10 diferencias.
4. **Rana y salmón:** 88,5 %, 12 diferencias.
5. **Humano y gallina:** 87,6 %, 13 diferencias.
6. **Gallina y salmón:** 87,5 %, 13 diferencias.

El más **distante** es la mosca: 66,0 % de identidad con el humano y 65–70 % con el resto.

¿Cómo se explica? Por un lado, la similitud refleja **ancestro común y constricción funcional**: los primates son los más cercanos, y todos los vertebrados comparten un citocromo c muy conservado. Por otro lado, las discrepancias en los nodos profundos se explican por las **limitaciones del conjunto de datos**:

- la proteína es corta (~105 aminoácidos) y muy conservada;
- solo hay **15 sitios parsimoniosamente informativos** y 26 singletons/autapomorfías;
- hay gaps y regiones de alineamiento ambiguo;
- las secuencias de rana y mosca son **isoformas parálogas** (testículo-específica y distal, respectivamente), que pueden evolucionar a ritmos distintos;
- con tan poca señal, fenómenos como la **atracción de ramas largas** pueden agrupar artificialmente linajes divergentes (de hecho, el análisis de máxima verosimilitud agrupó al perro con la mosca).

En resumen: el árbol es una **hipótesis** coherente con los grupos cercanos y con la posición de la mosca, pero no resuelve las relaciones profundas. Eso no es un fracaso del ejercicio: es una lección sobre los límites de los datos y de los métodos.

### 4.12 Limitaciones y una observación importante sobre las secuencias

Además de la escasa señal filogenética, hay un punto metodológico que conviene señalar: **no todas las secuencias de la consigna son ortólogas**. En particular:

- La secuencia de *Xenopus laevis* es **citocromo c testículo-específico** (NP_001086101.1), no la isoforma somática A (NP_001085462.1).
- La secuencia de *Drosophila melanogaster* es **citocromo c distal, isoforma A** (NP_477164.1), no la isoforma proximal (NP_477176.1), que es la isoforma de expresión mayoritaria durante el desarrollo.

Las isoformas parálogas pueden tener tasas evolutivas distintas y no reflejar la filogenia de las especies. Al repetir el análisis con las isoformas somáticas, la identidad de la mosca con el humano sube de **66,0 % a 76,9 %**, y el árbol de distancias se acerca a la topología esperada: mamíferos, luego rana, luego gallina, luego salmón y mosca. La posición relativa de ave y anfibio sigue siendo lábil, pero el efecto de elegir ortólogos es evidente. Con UPGMA, que asume un reloj molecular, se recupera exactamente el orden esperado (mamíferos, ave, anfibio, pez, insecto).

Otras limitaciones:

- **Longitud y conservación:** 105 aminoácidos y 61 % de columnas invariantes dejan pocos sitios informativos.
- **Alineamiento:** los gaps y las regiones terminales son ambiguos; distintos programas o parámetros pueden cambiar el alineamiento.
- **Modelo:** el árbol de distancias con Poisson es simple; la máxima verosimilitud con WAG+G4 tampoco estabilizó los nodos profundos.
- **Muestra:** nueve organismos y un solo gen no alcanzan para reconstruir relaciones profundas; harían falta más genes, genomas completos y una evaluación explícita de ortología.
- **Interpretación:** un árbol filogenético es una estimación estadística, no una verdad observable; el soporte de bootstrap es una medida de robustez, no una probabilidad de que el árbol sea “correcto”.

---

## 5. Conclusiones

1. El citocromo c es una proteína **muy conservada**: 64 de 105 columnas con residuos son invariantes, y el motivo CXXCH y la metionina axial se mantienen en las nueve secuencias.
2. La similitud refleja la cercanía evolutiva: humano, chimpancé y gorila son **idénticos**; caballo y perro comparten el **95,2 %**; humano y gallina, el **87,6 %**; la mosca es el organismo más divergente (**66,0 %** respecto del humano).
3. El árbol recupera con buen soporte los **grupos cercanos** (primates, humano-chimpancé, caballo-perro) y la posición de la **mosca como grupo externo**, pero no resuelve las relaciones profundas entre mamíferos, aves, anfibios y peces.
4. La baja resolución se explica por la **escasez de sitios informativos**, la alta conservación de la proteína, los gaps y la presencia de **isoformas parálogas** en rana y mosca. Elegir ortólogos somáticos mejora la congruencia con la filogenia esperada.
5. El ejercicio muestra el poder y los límites de la filogenia molecular: una proteína puede aportar evidencia valiosa sobre la evolución, pero un solo gen corto no basta para reconstruir toda la historia. El árbol es una hipótesis, y como tal debe interpretarse con cautela.

---

## 6. Referencias

- Velez Rueda, A. J. (s. f.). *En qué se parecen una gallina y una mosca*. Introducción a la Bioinformática. GitHub. https://github.com/AJVelezRueda/Introduccion_a_la_Bioinformatica
- NCBI Protein Database. National Center for Biotechnology Information. https://www.ncbi.nlm.nih.gov/protein
- Sievers, F., Wilm, A., Dineen, D., Gibson, T. J., Karplus, K., Li, W., Lopez, R., McWilliam, H., Remmert, M., Söding, J., Thompson, J. D., & Higgins, D. G. (2011). Fast, scalable generation of high-quality protein multiple sequence alignments using Clustal Omega. *Molecular Systems Biology*, 7, 539.
- Madeira, F., Pearce, M., Tivey, A. R. N., Basutkar, P., Lee, J., Edbali, O., Madhusoodanan, N., Kolesnikov, A., & Lopez, R. (2022). Search and sequence analysis tools services from EMBL-EBI in 2022. *Nucleic Acids Research*, 50(W1), W276–W279.
- Minh, B. Q., Schmidt, H. A., Chernomor, O., Schrempf, D., Woodhams, M. D., von Haeseler, A., & Lanfear, R. (2020). IQ-TREE 2: New models and efficient methods for phylogenetic inference in the genomic era. *Molecular Biology and Evolution*, 37(5), 1530–1534.
- Cock, P. J. A., Antao, T., Chang, J. T., Chapman, B. A., Cox, C. J., Dalke, A., Friedberg, I., Hamelryck, T., Kauff, F., Wilczynski, B., & de Hoon, M. J. L. (2009). Biopython: freely available Python tools for computational molecular biology and bioinformatics. *Bioinformatics*, 25(11), 1422–1423.
- Page, R. D. M., & Holmes, E. C. (2009). *Molecular Evolution: A Phylogenetic Approach*. Wiley-Blackwell.
- Zuckerkandl, E., & Pauling, L. (1965). Molecules as documents of evolutionary history. *Journal of Theoretical Biology*, 8(2), 357–366.
- Kimura, M. (1968). Evolutionary rate at the molecular level. *Nature*, 217, 624–626.

---

## 7. Anexo: alineamiento completo

<details>
<summary>Ver el alineamiento en formato FASTA (107 columnas)</summary>

```fasta
>NP_061820.1 cytochrome c [Homo sapiens]
--MGDVEKGKKIFIMKCSQCHTVEKGGKHKTGPNLHGLFGRKTGQAPGYSYTAANKNKGI
IWGEDTLMEYLENPKKYIPGTKMIFVGIKKKEERADLIAYLKKATNE
>NP_001072946.1 cytochrome c [Gallus gallus]
--MGDIEKGKKIFVQKCSQCHTVEKGGKHKTGPNLHGLFGRKTGQAEGFSYTDANKNKGI
TWGEDTLMEYLENPKKYIPGTKMIFAGIKKKSERVDLIAYLKDATSK
>NP_001065289.1 cytochrome c [Pan troglodytes]
--MGDVEKGKKIFIMKCSQCHTVEKGGKHKTGPNLHGLFGRKTGQAPGYSYTAANKNKGI
IWGEDTLMEYLENPKKYIPGTKMIFVGIKKKEERADLIAYLKKATNE
>NP_001157486.1 cytochrome c [Equus caballus]
--MGDVEKGKKIFVQKCAQCHTVEKGGKHKTGPNLHGLFGRKTGQAPGFSYTDANKNKGI
TWKEETLMEYLENPKKYIPGTKMIFAGIKKKTEREDLIAYLKKATNE
>NP_001183974.1 cytochrome c [Canis lupus familiaris]
--MGDVEKGKKIFVQKCAQCHTVEKGGKHKTGPNLHGLFGRKTGQAPGFSYTDANKNKGI
TWGEETLMEYLENPKKYIPGTKMIFAGIKKTGERADLIAYLKKATKE
>AEP27192.1 cytochrome c [Gorilla gorilla]
--MGDVEKGKKIFIMKCSQCHTVEKGGKHKTGPNLHGLFGRKTGQAPGYSYTAANKNKGI
IWGEDTLMEYLENPKKYIPGTKMIFVGIKKKEERADLIAYLKKATNE
>XP_024245566.1 cytochrome c [Oncorhynchus tshawytscha]
--MGDIEKGKKAFVQKCAQCHTVENGGKHKVGPNLWGLFGRKTGQAEGFSYTDANKAKGI
VWDTDTLMTYLENPKKYIPGTKMIFAGIKKKGERADLIAYLKSATS-
>NP_001086101.1 cytochrome c, testis-specific [Xenopus laevis]
--MGDVEKGKKVFVQKCSQCHTVEKGGKHKTGPNLHGLFGRKTGQAEGFSYTDANKNKGI
VWDEDTLMVYLENPKKYIPGTKMIFAGIKKKGERQDLIAYLKQSTSS
>NP_477164.1 cytochrome c distal, isoform A [Drosophila melanogaster]
MGSGDAENGKKIFVQKCAQCHTYEVGGKHKVGPNLGGVVGRKCGTAAGYKYTDANIKKGV
TWTEGNLDEYLKDPKKYIPGTKMVFAGLKKAEERADLIAFLKSNK--
```

</details>

---

## Licencia y atribución

Este informe es una obra derivada de la consigna de la Dra. Ana Julia Velez Rueda, publicada bajo la licencia [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/). Se distribuye bajo la misma licencia.

Las secuencias son datos públicos de NCBI Protein. Las figuras y el texto fueron elaborados por el autor a partir de esos datos.
