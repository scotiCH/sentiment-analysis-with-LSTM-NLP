# 🎬 Sentiment Analysis with LSTM (NLP)

![Python](https://img.shields.io/badge/Python-3.11-blue)
![PyTorch](https://img.shields.io/badge/PyTorch-2.2.0-red)
![scikit-learn](https://img.shields.io/badge/scikit--learn-1.3.0-green)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

## О проекте

Проект реализует **анализ тональности (sentiment analysis)** отзывов о фильмах (IMDb) с использованием двунаправленной LSTM-модели на PyTorch. Репозиторий демонстрирует полный практический цикл работы с текстовыми данными: загрузка, очистка, токенизация/лэмматизация, построение словаря, обучение модели, оценка качества и сохранение артефактов (модель, словарь).

Особенность проекта — аккуратно структурированный Jupyter Notebook (`NLP.ipynb`), который можно запустить как локально, так и в среде Google Colab для воспроизводимости результатов.

---

## 🔎 Основные возможности

- Предобработка текста: очистка, нормализация, лемматизация/токенизация.
- Построение словаря (vocab) и преобразование текстов в последовательности индексов.
- Обучение BiLSTM с эмбеддингами на PyTorch.
- Использование `EarlyStopping` и сохранение лучшей модели по валидационной метрике.
- Подробная отчётность: accuracy, precision/recall/f1, confusion matrix.
- Визуализация кривых обучения и матрицы ошибок.
- Сохранение модели и словаря для дальнейшего использования в приложениях (инференс/REST API).

---

## 📁 Структура репозитория (рекомендуемая)

```
.
├── data/
│   ├── raw/IMDB Dataset.csv        # исходные данные
│   └── processed/                  # подготовленные данные и словарь
├── models/
│   └── best_model.pth              # обученная модель
├── results/                        # графики, метрики (png)
├── NLP.ipynb                       # основной Jupyter Notebook
├── README.md                       # этот файл
└── requirements.txt                # зависимости с версиями
```

---

## ⚙️ Установка и запуск (локально)

**1. Клонируйте репозиторий и перейдите в папку проекта**

```bash
git clone https://github.com/USERNAME/PROJECT_NAME.git
cd PROJECT_NAME
```

**2. Создайте и активируйте виртуальное окружение (рекомендуется)**

Linux / macOS:
```bash
python -m venv venv
source venv/bin/activate
```

Windows (PowerShell):
```powershell
python -m venv venv
venv\Scripts\Activate.ps1
```

**3. Установите зависимости**

```bash
pip install -r requirements.txt
```

**4. Запустите Jupyter Notebook**

```bash
jupyter notebook NLP.ipynb
```

---

## 🧪 Как использовать: пример предсказания (инференс)

Ниже — минимальный пример загрузки сохранённой модели и словаря и получения предсказания для одного текста.

```python
import torch
import pickle
from pathlib import Path

# Путь к артефактам
models_dir = Path("models")
vocab_path = models_dir / "vocab.pkl"
model_path = models_dir / "best_model.pth"

# Загрузка словаря
with open(vocab_path, "rb") as f:
    vocab = pickle.load(f)

# Пример функции преобразования текста -> индексы (в ноутбуке есть полная версия)
def text_to_sequence(text, vocab, max_len=200):
    tokens = text.lower().split()  # простая токенизация, замените на ту, что в ноутбуке
    seq = [vocab.get(t, vocab.get("<UNK>", 1)) for t in tokens]
    if len(seq) < max_len:
        seq = seq + [vocab.get("<PAD>", 0)] * (max_len - len(seq))
    else:
        seq = seq[:max_len]
    return torch.tensor([seq], dtype=torch.long)

# Загрузка модели (пример, адаптируйте класс модели под вашу реализацию)
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")
model = torch.load(model_path, map_location=device)
model.eval()

sample_text = "This movie was absolutely fantastic — great acting and story."
seq = text_to_sequence(sample_text, vocab)
with torch.no_grad():
    logits = model(seq.to(device))
    probs = torch.softmax(logits, dim=1)
    pred = torch.argmax(probs, dim=1).item()

print("Prediction:", pred, "Probabilities:", probs.cpu().numpy())
```

> В ноутбуке присутствует более аккуратная версия препроцессинга и соответствующая реализация модели — используйте её для точного воспроизведения результатов.

---

## 📊 Пример ожидаемых результатов

```
              precision    recall  f1-score   support

    negative       0.88      0.90      0.89      5000
    positive       0.90      0.88      0.89      5000

    accuracy                           0.89     10000
```

Иллюстрации: разместите в `results/` (например, `confusion_matrix.png`, `training_plot.png`).

---

## 🛠 Технологии и библиотеки

Проект использует следующие библиотеки (версии указаны в `requirements.txt`):
- Python 3.11
- PyTorch, pandas, scikit-learn, numpy, NLTK, matplotlib, seaborn, tqdm

---

## 📈 Планы по улучшению

- Добавить расширенную токенизацию/субсловную сегментацию (SentencePiece / BytePair).
- Реализовать предобученные эмбеддинги (GloVe / fastText) или дообучение BERT-like модели.
- Сделать REST API для инференса (FastAPI + Uvicorn) и Dockerfile для контейнеризации.
- Добавить CI: тесты и проверку стиля (pre-commit, flake8/isort).

---

## ⚖️ Лицензия

Этот проект распространяется под лицензией MIT — подробности в файле `LICENSE`.
