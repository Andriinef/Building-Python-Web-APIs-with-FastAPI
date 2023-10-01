# Используем базовый образ Python 3.10
FROM python:3.10.0

# Устанавливаем переменные окружения
ENV PIP_DISABLE_PIP_VERSION_CHECK 1
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# Устанавливаем зависимости для сборки Python-пакетов и очищаем неиспользуемые файлы
RUN apt-get update \
    && apt-get install -y build-essential \
    libcurl4-openssl-dev libssl-dev \
    && rm -rf /var/lib/apt/lists/*

# Установка pipenv, pip и setuptools
RUN pip install --upgrade pip pipenv setuptools

# Устанавливаем рабочую директорию внутри контейнера
WORKDIR /app

# Копируем requirements.txt в контейнер
COPY requirements.txt /app/

# Установка зависимостей из requirements.txt в Pipfile
RUN pipenv install --requirements requirements.txt

# Запуск pipenv lock для создания Pipfile.lock
RUN pipenv lock

# Запуск pipenv sync для установки зависимостей
RUN pipenv sync --system ${PIPENV_EXTRA_ARGS}

# Копируем все файлы проекта в контейнер
COPY . .

# Открываем порт, на котором будет доступно приложение
EXPOSE 8000

# Запускаем FastAPI приложение при старте контейнера
CMD ["pipenv", "run", "uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
