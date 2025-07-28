# Seguimiento 1 _Lates Calcarifer_

## 1) Descripción del archivo
### **a) Describa los campos que se encontrará en este tipo de archivos. ¿Qué información está allí contenida? Proporcione ejemplos.**

### *Columna 1 (seqid)*

Es básicamente el id de la secuencia principal (cromosoma, contig, scaffold, etc.) que nos dice pues el sistema de coordenadas para la característica actual. Estos no pueden contener caracteres raros ni espacios.

La información que tiene es la referencia de la secuencia en donde esta la característica. Ej: chr1

### *Columna 2 (source)*

Es el nombre de la fuente de donde se genero la característica, o sea que puede ser de un software o una base de datos de nombre x. El proposito del source es extender la ontologia de caracteristicas añadiendo un calificador al type. 
Ej: Ensembl

### *Columna 3 (type)*

Es el tipo de característica biológica, este tiene que ser un termino del "Sequence Ontology" (SO).
Ej: gene, exon, CDS, mRNA

### *Columna 4 y 5 (start end)*

Son las coordenadas de inicio a fin de la característica en la secuencia, numerada desde 1 (la posición de inicio siempre tiene que ser menor o igual que la del final). O sea La ubicación geográfica precisa (posiciones en el ADN/ARN) de la característica dentro de la sequid.
Ej: 1000, 5000


### *Columna 6 (score)*

Es la puntuación de confianza de la característica que esta representada por un valor numérico. Para características de similitud de secuencia se usa valores-e y valores-p para características de predicción de genes ab initio, y si no hay una puntuación que aplique solo se pone un punto.
Ej: 0.95


### *Columna 7 (strand)*

Nos dice la hebra o la cadena de a secuencia en donde se encuentra la característica. Puede ser: "+" hebra positiva; "-" hebra inversa; "." característica indeterminada; "?" hebra relevante pero desconocida.
Ej: +, -, ., ?


### *Columna 8 (phase)*

Esta solo se usa para características del tipo CDS, y nos indica en donde comienza el siguiente codón en relación con el extremo 5' del CDS actual. Puede ser: "0" el codón empieza en el primer nucleótido; "1" el codón empieza en el 2ndo nucleótido; "2" el codón empieza en el 3er nucleótido; para todas las demás se usa el punto.
Ej: 0, 1, 2


### *Columna 9 (attributes)*

Es una lista de pares (tag=valor) separados por un punto y coma, que nos van a dar info adicional de la característica, lo cual es muy flexible para metadatos adicionales, y algunos tags tienen significados predefinidos:
* **ID:** Identificador de la característica (para características que son padre de otras).
* **Name:** Nombre para el usuario.
* **Alias:** Nombre secundario.
* **Parent:** Uno o más IDs de características padre a las que pertenece esta característica, deja que haya jerarquía (un ARNm tiene un gen como padre).
* **Target:** Indica el objetivo de una alineación.
* **Derives_from:** Relación temporal o de derivación (genes policistronicos).
* **Note:** Nota de texto cualquiera.
* Dbxref: Referencia cruzada a una base de datos externa.
* **Ontology_term:** Referencia a un termino de ontología.
* **Is_circular:** Indica si la secuencia es circular.

Ej: ID=gene001;Name=BRCA1;biotype=protein_coding;Note=Human gene for breast cancer susceptibility`


### **Ejemplo**

```
ctg123 . CDS             7000  7600  .  +  0  ID=cds00001;Parent=mRNA00001;Name=edenprotein.1
```

## 2) Descarga de archivos de trabajo


**a) Descarga del archivo por medio de la consola**

```
zofix2@SOFIA-CABEZAS ~/seguimiento1
$ wget http://ftp.ensembl.org/pub/current_gff3/lates_calcarifer/Lates_calcarifer.ASB_HGAPassembly_v1.114.gff3.gz
```

Se uso el comando _wget_ para poder descargar lo que está en el siguiente link.



**b) Descomprima el archivo usando la línea de comandos.**

```
gunzip Lates_calcarifer.ASB_HGAPassembly_v1.114.gff3.gz
```
Se usó el comando _gunzip_ para descomprimir el archivo.


## 3) Análisis del archivo

### **a) Proporcione una breve descripción del organismo asignado en el archivo README.**

_Lates Calcarifer_, también conocido como barramundi o lubina asiática, es una especie de pez catádromo de gran importancia económica y ecológica.



#### **Descripción general**

- **Clasificación:** Pertenece a la familia _Latidae_, dentro del orden Carangiformes.

- **Distribución:** Se encuentra ampliamente distribuido en la región del Indo-Pacífico Occidental, abarcando desde Oriente Medio, Asia del Sur y Sudeste, hasta Asia Oriental y Oceanía (incluyendo Australia).

- **Hábitat:** Es una especie eurihalina, lo que significa que puede vivir en una amplia gama de salinidades. 

- **Características Físicas:** Tiene un cuerpo alargado y comprimido, con una cabeza puntiaguda y una boca grande. Sus escamas son grandes y ctenoideas. Los adultos suelen ser de color plateado con el dorso más oscuro, mientras que los juveniles pueden presentar barras transversales oscuras que desaparecen con la madurez. Pueden alcanzar tamaños considerables, con una longitud máxima reportada de hasta 2 metros y un peso de alrededor de 60 kg.

- **Biología:** Son hermafroditas protándricos, lo que significa que nacen machos y cambian a hembras entre los 3 y 5 años de edad. Migran de aguas dulces a estuarios salobres para desovar.


### **b) Investigue, usando las herramientas de la línea de comandos de Unix, y conteste las siguientes preguntas:**

*i. ¿Cuántos features contiene el archivo?*

```
zofix2@SOFIA-CABEZAS ~/seguimiento1
$ cat Lates_calcarifer.ASB_HGAPassembly_v1.114.gff3 | grep -v "^#" Lates_calcarifer.ASB_HGAPassembly_v1.114.gff3 | wc -l
1728776
```

Aquí se usa el comando _grep -v_ para filtrar todo lo que **no** empieza por # (que normalmente en un archivo .gff es un titulo o comentario, lo cual no nos sirve para saber cuántos features hay), y ya el _wc -l_ cuenta las líneas que si nos importan.

* En el archivo hay 1728776 features.

*ii. ¿Cuántas regiones de la secuencia (cromosomas) contiene el archivo?*

```
zofix2@SOFIA-CABEZAS ~/seguimiento1
$ cat Lates_calcarifer.ASB_HGAPassembly_v1.114.gff3 | grep "^##sequence-regi
on" Lates_calcarifer.ASB_HGAPassembly_v1.114.gff3 | wc -l
3807
```

Aquí también usamos el _grep_, solo que ahora filtramos los que inician con ##sequence-region que normalmente están acompañados de un cromosoma en específico al lado, y ya con el _wc -l_ se cuentan las líneas.

* Hay 3807 regiones de la secuencia en el archivo.

*iii. ¿Cuántos genes están listados en el organismo?*

```
zofix2@SOFIA-CABEZAS ~/seguimiento1
$ cat Lates_calcarifer.ASB_HGAPassembly_v1.114.gff3 | cut -f 3 | grep -i "^g
ene$" | wc -l
25109
```

Aquí entonces lo que hice fue, ya que en las especificaciones de un archivo .gff la columna 3 tiene el "type" (que puede ser un exón, CDS, gen, etc.) corté la columna 3 con el comando _cut -f 3_ para que el _grep_ solo coja y filtre la info de allí, que en este caso seria la palabra "gene" y ya por ultimo use el comando _wc -l_ para que ya me cuente las líneas y me de el resultado.

* Hay 25109 genes listados del _Lates Calcarifer_ 

*iv. ¿Cuál es el top 10 de tipo de features (columna 3 más anotados en el genoma?*

```
zofix2@SOFIA-CABEZAS ~/seguimiento1
$ cat Lates_calcarifer.ASB_HGAPassembly_v1.114.gff3 | grep -v "^#" | cut -f
3 | sort | uniq -c | sort -rn | head -n 10
 787631 exon
 761857 CDS
  58165 mRNA
  44499 five_prime_UTR
  33044 three_prime_UTR
  25109 gene
  11309 biological_region
   3807 region
   1306 ncRNA_gene
    455 snRNA
```

Aquí entonces, de primeras me tocó poner un filtro para que no me mostrara cualquier cosa que empezara por #, ya que en el primer intento, el top en el puesto 6 me aparecía: 30568 ###. Ya después se corta solo para usar la columna 3 con el comando _cut -f 3_, con el comando _sort_ me ayuda a agrupar y ordenar los tipos, el _uniq -c_ ayuda a que no me aparezcan los tipos duplicados y además me ayuda a contar el tipo, ya con el _sort -rn_ ayuda a ordenar el tipo y hacerlo del mayor al menor, y por ultimo el _head -n 10_ me muestra las 10 primeras líneas que pues serían el top 10 que necesitamos. 
