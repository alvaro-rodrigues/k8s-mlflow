FROM python:3.8.2-slim
WORKDIR /serving

RUN apt update && apt install -y libgomp1

RUN pip install mlflow[extras]==1.9.1 && \
    pip install psycopg2-binary==2.8.5 && \
    pip install boto3==1.15.16 && \
    pip install pandas && \
    pip install scikit-learn && \
    pip install lightgbm && \
    pip install matplotlib \
    pip install uvicorn && \
    pip install numpy && \
    pip install fastapi && \
    pip install pydantic

COPY . .

EXPOSE 8000

ENTRYPOINT ["python", "serve.py"]