
FROM python:3.11-slim

WORKDIR /app

# Copy only dependency file first (better Docker layer caching)
COPY requirements.txt .

# Install Python dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application source code
COPY . .


EXPOSE 5000


CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]
