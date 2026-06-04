# Dataset

El archivo CSV utilizado en este proyecto no se incluye directamente en el repositorio.

El notebook descarga automáticamente mediante `gdown` un subconjunto preparado del dataset N-BaIoT.

Este subconjunto corresponde al dispositivo IoT **Provision PT-737E**, una cámara de vigilancia incluida en el dataset original.

Archivos originales utilizados:

- benign.csv
- gafgyt.scan.csv
- gafgyt.udp.csv
- mirai.scan.csv
- mirai.udp.csv

Registros utilizados:

- benign: 40,000
- gafgyt.scan: 10,000
- gafgyt.udp: 10,000
- mirai.scan: 10,000
- mirai.udp: 10,000

Total: 80,000 registros.

Variables seleccionadas:

- H_L0.1_weight
- H_L0.1_mean
- H_L0.1_variance
- HH_jit_L0.01_mean
- HpHp_L0.1_radius

El CSV descargado desde Google Drive contiene estas cinco variables y una columna adicional llamada `Etiqueta`.

Fuente original del dataset N-BaIoT:

- Kaggle: https://www.kaggle.com/datasets/mkashifn/nbaiot-dataset
- UCI Machine Learning Repository: https://doi.org/10.24432/C5RC8J
- Paper base: https://doi.org/10.48550/arXiv.1805.03409
