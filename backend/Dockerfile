FROM mcr.microsoft.com/devcontainers/python:1-3.11-bookworm

# Install basic development tools
RUN apt update && apt install -y less man-db sudo

RUN pip install fastapi
RUN pip install "uvicorn[standard]"

# Set `DEVCONTAINER` environment variable to help with orientation
ENV DEVCONTAINER=true

WORKDIR /app
COPY ./app/ .
CMD ["uvicorn", "main:app", "--host", "0.0.0.0", "--port", "8000"]
EXPOSE 8000
