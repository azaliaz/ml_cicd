# Лабораторная №1

1. Установка и запуска
```
python3 -m venv .venv
source .venv/bin/activate  
pip install -r requirements.txt
python -m src.preprocess
python -m src.train
uvicorn api.main:app --reload --host 0.0.0.0 --port 8000
```