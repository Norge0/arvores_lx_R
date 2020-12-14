<h3>Árvores em Lisboa</h3><p></p>
Choupos "Populus alba" e "Populus nigra"<br>
Aula prática de programação em R<br>
Unidade Curricular - Geocomputação 2020/21 do Mestrado SIGMTAO do IGOT-UL<p></p>
<img src="trees_r.png" alt="image" width="" height="300">


#### Code in R

#Instalar e carregar biblioteca para dados espaciais
```
install.packages("raster")
library(raster)
install.packages("rgdal")
library(rgdal
```

#Definir pasta de trabalho<br>
`setwd("G:/data5")`

#Importar shapefiles
```
concelhos = shapefile("concelhos.shp")
arvores_lisboa= shapefile("Arvoredo.shp")
```

#Ver os dados
```
plot(concelhos)
plot(arvores_lisboa)
```
#inspecionar campos
```
summary(concelhos)
summary(arvores_lisboa)
```

#Selecionar apenas choupos 'Populus alba' e 'Populus nigra’<br>
#Identificar valores únicos do campo espécie (ESPECIE_VA que é a coluna4 [,4])<p></p>
`unique(arvores_lisboa$ESPECIE_VA)`

