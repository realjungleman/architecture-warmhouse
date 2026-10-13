FROM python:3.8-alpine

WORKDIR /app

COPY ./sensorrandom.py /app/sensorrandom.py
COPY ./requirements.txt /app/requirements.txt
COPY ./init.sql /app/init.sql

RUN pip install --no-cache-dir -r requirements.txt

EXPOSE 8081
CMD ["python", "sensorrandom.py"]