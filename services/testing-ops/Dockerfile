# Stage 1: Build
FROM python:3.12-slim AS build

WORKDIR /app

# Copy dependencies and install
COPY app/requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy app code
COPY app/ ./

# Stage 2: Production image
FROM python:3.12-slim

WORKDIR /app

# Copy installed packages
COPY --from=build /usr/local/lib/python3.12/site-packages /usr/local/lib/python3.12/site-packages
COPY --from=build /app /app

# Expose FastAPI port
EXPOSE 8000

# Run FastAPI
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
