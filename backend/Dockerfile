# Use a lightweight Python image
FROM python:3.10-slim

# Install system dependencies like curl and other required libraries
RUN apt-get update && \
    apt-get install -y \
    curl \
    build-essential \
    libpq-dev && \
    rm -rf /var/lib/apt/lists/*

# Set the working directory
WORKDIR /app

# Copy only necessary files for dependency installation first (to cache layers)
COPY pyproject.toml poetry.lock ./

# Install Poetry and add it to PATH in a single layer
RUN curl -sSL https://install.python-poetry.org | python3 - && \
    ln -s /root/.local/bin/poetry /usr/local/bin/poetry && \
    poetry config virtualenvs.create false



# Copy the rest of the application code
COPY . .

# Regenerate the lock file and install dependencies
RUN poetry lock && poetry install --no-root --no-interaction

# Add the application directory to PYTHONPATH
ENV PYTHONPATH=/app:$PYTHONPATH

# Ensure prestart.sh is executable
RUN chmod +x /app/prestart.sh

# Expose the port for FastAPI
EXPOSE 8000

# Default command runs the prestart.sh and then the server
CMD ["/bin/bash", "-c", "./prestart.sh && poetry run uvicorn app.main:app --host 0.0.0.0 --port 8000"]