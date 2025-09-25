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


## 📊 Пример ожидаемых результатов

```
              precision    recall  f1-score   support

    negative       0.88      0.90      0.89      5000
    positive       0.90      0.88      0.89      5000

    accuracy                           0.89     10000
```

Иллюстрации:
![training_history](/results/plots/training_history.png)

![confusion_matrix](/results/plots/confusion_matrix.png)

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
