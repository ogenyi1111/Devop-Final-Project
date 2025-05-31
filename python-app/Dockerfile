# Use Python slim as base image
FROM python:3.9-slim

# Set working directory
WORKDIR /app

# Copy requirements file
COPY requirements.txt .

# Install dependencies
RUN pip install --no-cache-dir -r requirements.txt

# Copy application code
COPY . .

# Expose the application port
EXPOSE 5000

# Set environment variables
ENV PYTHONUNBUFFERED=1

# Start the application
CMD ["python", "app.py"] 