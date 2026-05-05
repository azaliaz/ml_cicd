# Лабораторная №1

## 1) Подготовка окружения

```bash
cd /Users/azaliagm/ml_cicd
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```
## 2) Подготовка данных

```bash
python -m src.preprocess
```

Результат:
- `data/raw/california_housing.csv`;
- `data/processed/X_train.csv`;
- `data/processed/X_test.csv`;
- `data/processed/y_train.csv`;
- `data/processed/y_test.csv`.

## 3) Обучение модели

Гиперпараметры читаются из `config.ini`.

```bash
python -m src.train
```

Результат:
- файл модели `models/model.joblib`;
- метрики в консоли (`r2`, `mae`).

## 4) API сервис

Запуск:
```bash
uvicorn api.main:app --reload --host 0.0.0.0 --port 8000
```

Проверка health:
```bash
curl http://127.0.0.1:8000/health
```

Проверка predict:
```bash
curl -X POST "http://127.0.0.1:8000/predict" \
  -H "Content-Type: application/json" \
  -d '{
    "MedInc": 8.3252,
    "HouseAge": 41.0,
    "AveRooms": 6.984127,
    "AveBedrms": 1.02381,
    "Population": 322.0,
    "AveOccup": 2.555556,
    "Latitude": 37.88,
    "Longitude": -122.23
  }'
```

## 5) Тесты и покрытие

Обычные тесты:
```bash
pytest -v
```

Тесты с покрытием:
```bash
pytest --cov=src --cov=api --cov-report=term-missing --cov-report=json:coverage.json
```

## 6) DVC

Запуск пайплайна:
```bash
dvc repro
```

Проверка состояния:
```bash
dvc status
dvc dag
```

## 7) Docker image и контейнер

Сборка:
```bash
docker build -t <dockerhub_user>/ml-course-assignment:latest .
```

Запуск:
```bash
docker run --rm -p 8000:8000 <dockerhub_user>/ml-course-assignment:latest
```

Либо через compose:
```bash
docker compose up --build
```

## 8) Заполнение `dev_sec_ops.yml`

Обнови коммиты и покрытие:
```bash
python scripts/update_dev_sec_ops.py
```

Собери и отправь образ:
```bash
docker push <dockerhub_user>/ml-course-assignment:latest
```

Получи digest:
```bash
docker inspect --format='{{index .RepoDigests 0}}' <dockerhub_user>/ml-course-assignment:latest
```

## 9) Сценарии функционального тестирования контейнера

Если контейнер уже запущен на 8000:
```bash
python scripts/run_scenarios.py --base-url http://127.0.0.1:8000
```
