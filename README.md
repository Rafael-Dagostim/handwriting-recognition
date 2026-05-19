# projeto-ia

> Reconhecimento de caracteres manuscritos em tempo real usando uma rede neural convolucional treinada em TensorFlow. Projeto final da matéria de Inteligência Artificial.

O usuário desenha letras em um canvas no navegador e o sistema identifica os caracteres usando técnicas de visão computacional (OpenCV) para segmentação e uma CNN para classificação. Funciona tanto via backend Python quanto inferência direta no browser via TensorFlow.js.

## Pipeline

1. **Captura** — usuário desenha em um canvas (`react-sketch-canvas`).
2. **Pré-processamento** — OpenCV faz binarização, dilatação e segmentação por contornos.
3. **Segmentação** — extrai cada caractere individual e redimensiona para 32×32 em escala de cinza.
4. **Classificação** — CNN (`model.h5`) prevê a letra; `LabelBinarizer` (`lb.pkl`) decodifica o índice em caractere.
5. **Saída** — texto reconhecido exibido no frontend.

## Stack

### Modelo (`example-code.ipynb` / `final-code.ipynb`)

- **TensorFlow / Keras** — treinamento da CNN
- **scikit-learn** — `LabelBinarizer` para one-hot encoding das classes
- **OpenCV** — pré-processamento das imagens

### Servidor Python (`server/server.py`)

- **Flask** + **flask-cors**
- **TensorFlow** (carrega `model.h5`)
- **OpenCV** + **imutils** + **PIL** — segmentação e processamento

### Frontend (`frontend/`)

- **React 18** + **TypeScript** + **Vite**
- **@tensorflow/tfjs** — inferência client-side (alternativa ao servidor)
- **@techstark/opencv-js** — OpenCV no navegador
- **react-sketch-canvas** — canvas de desenho

## Estrutura

```
.
├── example-code.ipynb       # notebook exploratório
├── final-code.ipynb         # notebook final (treinamento + avaliação)
├── server/
│   ├── server.py            # API Flask de inferência
│   ├── model.h5             # modelo Keras treinado
│   └── lb.pkl               # LabelBinarizer serializado
├── frontend/                # SPA Vite + React
├── imgs/                    # imagens usadas/exemplos
└── testes/                  # cases de teste
```

## Rodando localmente

### Servidor Python

```bash
cd server
pip install flask flask-cors tensorflow opencv-python imutils scikit-learn joblib pillow matplotlib numpy
python server.py
```

A API Flask sobe na porta padrão (5000) com endpoint para receber imagens e retornar texto.

### Frontend

```bash
cd frontend
npm install
npm run dev
```

Frontend disponível em `http://localhost:5173`.

### Notebooks

Para reproduzir o treinamento, abra `final-code.ipynb` em Jupyter ou Google Colab. O dataset e os hiperparâmetros estão documentados nas células.

## Observações

- A inferência pode ser feita 100% no browser via TFJS (sem servidor), ou via Flask.
- `model.h5` espera input 32×32×1 (grayscale).
