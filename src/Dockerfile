# temp stage
FROM python:3.8.12-slim-buster as builder

EXPOSE 5000

ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

RUN apt update && \
    apt install -y --no-install-recommends gcc

RUN python -m venv /opt/venv
ENV PATH="/opt/venv/bin:$PATH"

RUN python -m pip install --upgrade pip

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt --use-pep517


# final stage
FROM python:3.8.12-slim-buster

COPY --from=builder /opt/venv /opt/venv
COPY --from=builder /app/requirements.txt .

WORKDIR /app

ENV PATH="/opt/venv/bin:$PATH"

RUN addgroup --system app && adduser --system --group app && chown -R 755 /app

USER app

COPY . /app

CMD ["gunicorn", "--bind", "0.0.0.0:5000", "-c", "config.py", "app:app"]
