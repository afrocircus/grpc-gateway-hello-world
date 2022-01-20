FROM python:3.9
# RUN apk add g++

COPY requirements.txt /app/
WORKDIR /app
RUN pip install -r requirements.txt
COPY . .
EXPOSE 50051

ENTRYPOINT ["python"]
CMD ["-m", "grpc-backend.async_server"]
