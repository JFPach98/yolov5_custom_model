# yolov5

Pasos:

1. Crear ambiente virtual
2. Instalar pytorch 1.7 y cuda 1.0.1
```
pip install torch==1.7.0+cu101 torchvision==0.8.1+cu101 torchaudio===0.7.0 -f https://download.pytorch.org/whl/torch_stable.html
```

3. Clonar repositorio y descargar dependencias:

```
git clone https://github.com/ultralytics/yolov5
cd yolov5
pip install -r requirements.txt
cd ..
```

4. Descomprimir las carpetas de labels y de imágenes.

```
unzip labels
unzip dataset
```

- Si se cuenta con las imágenes de forma local, se puede crear una carpeta 'dataset' y se pueden insertar las imagenes en esta. Se puede hacer, de igual manera, para las labels.
- Las labels tienen que estar en formato yolo darknet. Para el formato de ternium, se coloca el archivo .csv en este repositorio y se corre en la consola el siguiente script:

```
convert_labels.py
```

5. Crear carpetas de test y de train, crear para cada carpeta subcarpetas de images y labels:
```
mkdir data
cd data
mkdir train,test
cd train
mkdir images,labels
cd ..
cd test
mkdir images,labels
cd ../..
```

6. Correr setup.py para crear datasets de entrenamiento y de prueba.

 ```
 python setup.py
 ```

7. Mover archivos 'classes.yaml' y 'yolov5.pt:

 ```
 mv classes.yaml ./yolov5
 mv yolov5.pt ./yolov5
 ```

8. Para entrenar de cero, utilizando los pesos de yolov5, se corre el siguiente script:
 
 ```
 cd yolov5
 python train.py --img 640 --batch 8 --epochs 30 --data classes.yaml --cfg ./yolov5/models/yolov5s.yaml --weights 'yolov5.pt' --name yolov5s_ternium
 ```

- Nótese que 'yolov5_ternium' es el nombre con el que se guardan los pesos. 

- Si se desea correr con los pesos ya entrenados para Ternium, se corre el siguiente script:

```
python train_mod.py --img 640 --batch 8 --epochs 30 --data ../data/classes.yaml --cfg yolov5s.yaml --weights /data/yolov5_ternium.pt --name yolov5s_ternium2 --nosave --cache
```

9. Para hacer la identificación de objetos, se corre el script detect.py usando los siguientes parámetros:
```
python detect.py --weights weights/last_yolo5s_ternium.pt --img 640 --conf 0.4 --source 
```

- Delante de source se introduce el path a las imágenes a procesar por el modelo.
