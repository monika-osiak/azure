FROM python:3
#ADD . /code
COPY requirements.txt /code/requirements.txt
WORKDIR /code
RUN pip install -r requirements.txt
#CMD python3 main.py