############################
# Stage 1 - Builder
############################
FROM python:3.11-slim AS builder

WORKDIR /app

# Copy dependency file
COPY requirements.txt .

# Install dependencies into a separate directory
RUN pip install --no-cache-dir \
    --prefix=/install \
    -r requirements.txt

############################
# Stage 2 - Runtime
############################
FROM python:3.11-slim

WORKDIR /app

# Copy only installed packages
COPY --from=builder /install /usr/local

# Copy application source
COPY . .

# Create a non-root user
RUN useradd -m appuser

# Change ownership
RUN chown -R appuser:appuser /app

# Switch to non-root user
USER appuser

EXPOSE 5000

CMD ["gunicorn","--bind","0.0.0.0:5000","app:app"]
