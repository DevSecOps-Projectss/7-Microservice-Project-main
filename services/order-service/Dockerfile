# ---------- Dependency stage ----------
FROM python:3.12-slim AS build
WORKDIR /build

COPY requirements.txt .
RUN python -m pip install --no-cache-dir --prefix=/install -r requirements.txt

# ---------- Runtime stage ----------
FROM python:3.12-slim
WORKDIR /app

ENV PYTHONUNBUFFERED=1 \
    PYTHONDONTWRITEBYTECODE=1

RUN groupadd --system appgroup && useradd --system --gid appgroup --create-home appuser

COPY --from=build /install /usr/local
COPY --chown=appuser:appgroup app ./app

USER appuser
EXPOSE 8084
CMD ["uvicorn", "app.main:app", "--host", "0.0.0.0", "--port", "8084"]
