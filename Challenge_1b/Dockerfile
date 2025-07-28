FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .
ENV PIP_DEFAULT_TIMEOUT=300
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

WORKDIR /app

ENTRYPOINT ["python", "main_pipeline.py"]
