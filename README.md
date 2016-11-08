# TFM_Visivilidad
Scrip con R para calcular diferencias entre visivilidades 
#Paso 1: Directorio de trabajo
setwd('F:\\resultados_visivi')


#Paso 1: Cargar paquetes
library(raster)
library(rgdal)
library(tiff)




###########################################  AUTOMÁTICO
#Paso 1: Directorio de trabajo
setwd('F:\\resultados_visivi')

#Paso 2: Listas

lista_estandar = list.files('./atmosfera_estandar/', pattern='.tiff$')
lista_doble = list.files('./atmosfera_doble/', pattern='.tiff$')
lista_triple =list.files('./atmosfera_triple/', pattern='.tiff$')

#Paso 3: Sumar +1

## SUMAR 1 A LAS IMÁGENES ESTANDAR
for(i in 1:length(lista_estandar)){
  print(paste(i,'de',length(lista_estandar),'estandar'))
  a = raster(paste('./atmosfera_estandar/',lista_estandar[i],sep=''))
  b = a+1
  writeRaster(x = b,filename = paste('./Resultado_estandar/',lista_estandar[i],'_sum','.tif',sep=''), format = 'GTiff', overwrite = TRUE)
}

## SUMAR 1 A LAS IMÁGENES DOBLES
for(i in 1:length(lista_doble)){
  print(paste(i,'de',length(lista_doble),'dobles'))
  a = raster(paste('./atmosfera_doble/',lista_doble[i],sep=''))
  b = a+1
  writeRaster(x = b,filename = paste('./Resultado_doble/',lista_doble[i],'_sum','.tif',sep=''), format = 'GTiff', overwrite = TRUE)
}

## SUMAR 1 A LAS IMÁGENES TRIPLES
for(i in 1:length(lista_triple)){
  print(paste(i,'de',length(lista_triple),'triples'))
  a = raster(paste('./atmosfera_triple/',lista_triple[i],sep=''))
  b = a+1
  writeRaster(x = b,filename = paste('./Resultado_triple/',lista_triple[i],'_sum','.tif',sep=''), format = 'GTiff', overwrite = TRUE)
}


#Paso 4: Restar entre ellas
lista_estandar1 = list.files('./Resultado_estandar/', pattern=)
lista_doble1 = list.files('./Resultado_doble/', pattern=)
lista_triple1 = list.files('./Resultado_triple/', pattern=)

#Creamos carpeta "Diferencias" donde meteremos todas las restas
dir.create('./Diferencias1/',showWarnings = F)
dir.create('./Diferencias2/',showWarnings = F)
dir.create('./Diferencias3/',showWarnings = F)

for(i in 1:length(lista_estandar1)){
  print(paste(i,'de',length(lista_estandar1),'imagenes'))
  
  #creamos un objeto con cada tipo de atmosfera de cada fecha
  a = raster(paste('./Resultado_estandar/',lista_estandar1[i],sep=''))
  b = raster(paste('./Resultado_doble/',lista_doble1[i],sep=''))
  c = raster(paste('./Resultado_triple/',lista_triple1[i],sep=''))
  
  #creamos objetos para cada resta entre los tipos de atmosfera
  d = b-a #doble menos estandar
  e = c-a #triple menos estandar
  f = c-b #triple menos doble

  #generamos un raster de cada resultante de las restas en una carpeta nueva que llamamos: C/Pere/Diferencias
  #estos rasteres tendran como coletilla final "_2_1" si es doble-estandar; _3_1 si es triple-estandar, o _3_2 si es triple-doble, o 2_3 si es doble-triple
  writeRaster(x = d,filename = paste('./Diferencias1/',lista_doble1[i],'_',lista_estandar1[i],'_2_1.tif',sep=''), format = 'GTiff', overwrite = TRUE)
  writeRaster(x = e,filename = paste('./Diferencias2/',lista_triple1[i],'_',lista_estandar1[i],'_3_1.tif',sep=''), format = 'GTiff', overwrite = TRUE)
  writeRaster(x = f,filename = paste('./Diferencias3/',lista_triple1[i],'_',lista_doble1[i],'_3_2.tif',sep=''), format = 'GTiff', overwrite = TRUE)
}

#Paso 5: listar las imagenes por diferencias 
listade = list.files('./Diferencias1/', pattern=)
listate = list.files('./Diferencias2/', pattern=)
listadt= list.files('./Diferencias3/', pattern=)


