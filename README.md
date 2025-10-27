# S.O
Es de Sistema Operativo
Estudiante: Franco Reyes

#!/bin/bash

opcion=0

while [ "$opcion" -lt 5 ]; do
echo "---------Menu---------"
echo " 1 - Agregar pagina (es necesario la URL)"
echo " 2 - Mostrar las paginas"
echo " 3 - Buscar una palabra en las URL cargadas"
echo " 4 - Ver el resultado de la busqueda"
echo " 5 - Salir del menu"
echo "----------------------"
read -p "Eliga una opcion: " opcion

if [ "$opcion" -eq 1 ];then
read -p "Coloque la URL de la pagina para guardarla: " pagina
touch PAGINAS.txt
echo "$pagina" >> PAGINAS.txt
echo "Pagina guardada"
elif [ "$opcion" -eq 2 ];then
echo "Mostrando las paginas cargadas:"
cat PAGINAS.txt
elif [ "$opcion" -eq 3 ];then
read -p "Ingrese la palabra que quiere buscar: " palabra
while read url;do
resultado=$(curl -L -s "$url" | grep -i -c "$palabra" )
touch RESULTADO.txt
echo "$url -> $resultado" >> RESULTADO.txt
done
elif [ "$opcion" -eq 4 ];then
echo "Mostrando los resultados guardados: "
cat RESULTADO.txt
elif [ "$opcion" -eq 5 ];then
echo "Advertencia si coloca 'yes' los archivos se vaciaran"
read -p "Quiere guardar una copia de seguridad de los archivos? (yes/no): " copia
if [ "$copia" = "yes" ];then
fecha=$(date +%d-%m-%Y)

Copia_Seguridad="Copion"
PAGINAS="PAGINAS.txt"
RESULTADO="RESULTADO.txt"

mkdir -p "$Copia_Seguridad"

archivo_Seguridad1="${Copia_Seguridad}/$(basename "$PAGINAS" .txt)_${fecha}.txt"
archivo_Seguridad2="${Copia_Seguridad}/$(basename "$RESULTADO" .txt)_${fecha}.txt"

truncate -s 0 PAGINAS.txt
truncate -s 0 RESULTADO.txt

echo "La copia de seguridad esta hecha"
echo "Saliendo del menu"
elif [ "$copia" = "no" ];then
echo "Saliendo del menu"
else
echo "Tiene que poner 'yes' o 'no'..."
fi
else
echo "Opcion no valida..."
fi
done
