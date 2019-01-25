FROM python:2-slim
RUN pip install --upgrade pip
RUN pip install flask
COPY hello.py /root
WORKDIR /root
EXPOSE 3000
ENV NAME Hello
CMD ["python", "hello.py"]
