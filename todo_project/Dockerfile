FROM python:3.10.12

WORKDIR /app

# Copy requirements first (for caching)
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Copy the rest of the application
COPY . .

# Run migrations and collect static files (optional)
RUN python manage.py migrate --noinput
# Uncomment the next line if you use static files
RUN python manage.py collectstatic --noinput

# Expose port
EXPOSE 8000

# Command to run the server
CMD ["python", "manage.py", "runserver", "0.0.0.0:8000"]
