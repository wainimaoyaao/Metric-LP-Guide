FROM python:3.12-alpine

WORKDIR /app
COPY metric_server.py index.html style.css app.js math.js ./
RUN mkdir -p data && chown -R 65534:65534 /app

ENV PYTHONDONTWRITEBYTECODE=1 PYTHONUNBUFFERED=1
USER 65534:65534
EXPOSE 8767
CMD ["python", "metric_server.py"]
