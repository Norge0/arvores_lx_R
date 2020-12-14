<h3>Árvores em Lisboa</h3><p></p>
Choupos "Populus alba" e "Populus nigra"<br>
Aula prática de programação em R<br>
Unidade Curricular - Geocomputação 2020/21 do Mestrado SIGMTAO do IGOT-UL<p></p>
<img src="trees_r.png" alt="image" width="" height="300">

<h4>Code in R</h4><p></p>
<h1>
# -----------
# Aula Prática
# -----------

#Carregar biblioteca para dados espaciais
install.packages("raster")  #caso não esteja instalado o packages
library(raster)

install.packages("rgdal")  #caso não esteja instalado o packages
library(rgdal)

#Definir pasta de trabalho
setwd("G:/data5")

#Importar shapefiles
concelhos = shapefile("concelhos.shp")
arvores_lisboa= shapefile("Arvoredo.shp")

#Ver os dados
plot(concelhos)
plot(arvores_lisboa)

#inspecionar campos
summary(concelhos)
summary(arvores_lisboa)


#Selecionar apenas choupos 'Populus alba' e 'Populus nigra’
#----------------------------------------------------------

#Identificar valores únicos do campo espécie (ESPECIE_VA que é a coluna4 [,4] )
unique(arvores_lisboa$ESPECIE_VA)

[5] "Populus nigra"  
[30] "Populus alba"

#selecionar apenas('Populus alba' e 'Populus nigra’)
choupos_lisboa = subset(arvores_lisboa, ESPECIE_VA
== "Populus alba" | ESPECIE_VA == "Populus nigra")

plot(choupos_lisboa)


#Calcular densidade de choupos
#--------------------------------------------------

#Calcular dens. de kernel com recurso à biblioteca 'spatialEco'
install.packages("spatialEco")
library(spatialEco)

kd=sp.kde(choupos_lisboa, bw=0.01, nr=300, nc=300)
#bw = Largura de banda
#nr = numero linhas 
#nc = número colunas

#visualizar dados
plot(kd)
plot(choupos_lisboa, add=TRUE) 

#explorar dados
summary(concelhos)
head(concelhos)
unique(concelhos$NAME_2)
[170]"Lisboa" 


#visualizar resultados: densidade de kernels com sobreposição
#do limite de concelho e dos pontos de ocorrência de choupos
conc_lisboa = subset(concelhos, NAME_2 == "Lisboa")
plot(kd)
plot(conc_lisboa, add=TRUE)                   
plot(choupos_lisboa, pch=19, col=1, cex=0.1, add=TRUE)

#definir valores de densidade de kernel fora do limite de concelho como nulos
kd_lisboa = mask(kd, conc_lisboa)            
plot(kd_lisboa)
plot(conc_lisboa, add=TRUE)                   
plot(choupos_lisboa, pch=19, col=1, cex=0.1, add=TRUE)



#Criar mapa web interativo com a biblioteca mapview
#--------------------------------------------------

#Instalar biblioteca 'mapview'
install.packages("mapview")
library(mapview)

#Visualizar tema (abre o browser, ver o screenshot mapview1.PNG)
mapview(choupos_lisboa)   
 
#definir nome do tema (screenshot mapview2.PNG)
mapview(choupos_lisboa, layer.name ="Espécies de choupos")    

#definir cores (screenshot mapview3.PNG)
cores_escolhidas = colorRampPalette(c("darkgreen", "green"))
#a
mapview(choupos_lisboa, zcol = "ESPECIE_VA",
col.regions=cores_escolhidas, layer.name ="Espécies de Choupos")


#Mapa raster (screenshot mapview4.PNG)
cores_para_raster = colorRampPalette(c("darkred", "orange",
"yellow", "green"))
#b
mapview(kd_lisboa, col.regions=cores_para_raster,
alpha.regions = 0.5, layer.name ="Densidade de choupos")


#Juntar temas no mapa interativo (#a Espécies de Choupo e #b Densidade de choupos)
a= mapview(choupos_lisboa, zcol = "ESPECIE_VA",
col.regions=cores_escolhidas, layer.name ="Espécies de Choupos")

b= mapview(kd_lisboa, col.regions=cores_para_raster,
alpha.regions = 0.5, layer.name ="Densidade de choupos")

#a+b

#Criar mapa interativo com a biblioteca mapview
Meu_mapa_interativo = a+b

#salvar em html 
mapshot(Meu_mapa_interativo, selfcontained = FALSE, url = "G:/data5/exports/map.html")

#-----------------</h1>
