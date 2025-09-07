# Stage 1: generate requirements
FROM python:3.12-slim AS generate-requirements

# Install curl and pip
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

# Install Poetry 2.x
ENV POETRY_VERSION=2.1.3
RUN pip install "poetry==$POETRY_VERSION"

# Add export plugin
RUN poetry self add "poetry-plugin-export[requirements]"

# Copy project metadata and export requirements.txt
COPY pyproject.toml poetry.lock ./
RUN poetry export --without-hashes --output requirements.txt

# Stage 2: production
FROM python:3.12-slim AS production

# Copy generated requirements and install dependencies
COPY --from=generate-requirements /requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the full project
COPY . . 

# Start the app
CMD ["python", "main.py"]
