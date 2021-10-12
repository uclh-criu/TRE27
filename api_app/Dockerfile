FROM python:3.8-slim-buster as base
COPY requirements.txt .
RUN pip3 install -r requirements.txt
COPY . /api


FROM base as test
COPY requirements-dev.txt .
RUN pip3 install -r requirements-dev.txt
WORKDIR /api
RUN pytest --junit-xml /test-results/api-pytest.xml --ignore e2e_tests


FROM scratch as test-results
COPY --from=test /test-results/*.xml .


FROM base as runtime
WORKDIR /api
RUN groupadd -r api && useradd -r -s /bin/false -g api api_user
USER api_user
CMD ["sh", "-c", "uvicorn main:app --host 0.0.0.0 --port 8000"]