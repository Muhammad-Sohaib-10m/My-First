PYTHON = python3
export PYTHONPATH := $(CURDIR)/src:$(PYTHONPATH)

.PHONY: help setup build serve test lint init-db clean clean-pyc clean-build clean-db

help:
	@echo "Makefile for cli-arena-ml-fastapi"
	@echo ""
	@echo "Usage:"
	@echo "  make setup          Install dependencies and initialize the database."
	@echo "  make build          (No-op for this Python project, or container build if specified)."
	@echo "  make serve          Run the FastAPI application using Uvicorn."
	@echo "  make test           Run Pytest tests with coverage."
	@echo "  make lint           Run linters (e.g., Ruff)."
	@echo "  make init-db        Initialize the database (create tables)."
	@echo "  make clean          Remove Python cache, build artifacts, and test cache."
	@echo "  make clean-pyc      Remove Python __pycache__ directories and .pyc files."
	@echo "  make clean-build    Remove build artifacts."
	@echo "  make clean-db       Remove local SQLite database file (if configured, e.g., ./app.db or ./db/main.db)."
	@echo ""

setup:
	@echo "Installing dependencies from requirements.txt..."
	$(PYTHON) -m pip install --upgrade pip
	$(PYTHON) -m pip install -r requirements.txt
	$(PYTHON) -m pip install ruff
	@echo "Dependencies installed."
	@echo "Initializing database..."
	$(MAKE) init-db
	@echo "Setup complete."

init-db:
	@echo "Initializing database schema..."
	$(PYTHON) scripts/init_db.py
	@echo "Database initialization script finished."

build:
	@echo "Build step (no-op for this project)."

serve:
	@echo "Starting FastAPI application..."
	$(PYTHON) -m uvicorn src.app.main:app --reload --host 0.0.0.0 --port 8000

test:
	@echo "Running Pytest tests with coverage..."
	$(PYTHON) -m pytest -v --cov=src --cov-report=term-missing --cov-report=html tests/
        @echo "Tests finished. Coverage report in htmlcov/index.html"

lint:
	@echo "Running linters (Ruff)..."
	ruff check .
	ruff format --check .
	@echo "Linting finished."

clean: clean-pyc clean-build
	@echo "Cleaning pytest cache and coverage reports..."
	rm -rf .pytest_cache/
	rm -rf htmlcov/
	rm -f .coverage
	@echo "All main cleaning tasks complete."

clean-pyc:
	@echo "Removing Python __pycache__ directories and .pyc/.pyo files..."
	find . -type d -name "__pycache__" -exec rm -rf {} +
	find . -type f -name "*.py[co]" -delete
	find . -type f -name "*$$py.class" -delete

clean-build:
	@echo "Removing build artifacts..."
	rm -rf build/
	rm -rf dist/
	rm -rf .eggs/
	find . -name '*.egg-info' -exec rm -rf {} +
	find . -name '*.egg' -delete

clean-db:
	@echo "Removing local SQLite database files..."
	rm -f ./app.db
	rm -f ./db/main.db
	rm -f ./main.db
	@echo "Done."
