# 01-dia

# aplicacion en python 

## opcional
```bash
apt install python3.12-venv -y 
```

## pasos

```bash
mkdir python_prom
cd python_prom/
vim app.py
python -m venv .venv
source .venv/bin/activate
pip install flask prometheus_client
```

## app en python 

`app.py`
```python
from flask import Flask
from prometheus_client import Counter, generate_latest

app = Flask(__name__)
# Definición de la métrica Counter
REQUEST_COUNT = Counter('http_requests_total', 'Total HTTP Requests', ['method', 'endpoint'])

@app.route('/')
def home():
    REQUEST_COUNT.labels(method='GET', endpoint='/').inc()
    return "Player 1 Ready!"

@app.route('/metrics')
def metrics():
    return generate_latest(), 200, {'Content-Type': 'text/plain'}

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

## lanzar la app instrumentada

```bash
python app.py
```


# prometheus
```bash
docker run \
    -p 9090:9090 \
    -v ./prom:/etc/prometheus \
    --add-host=host.docker.internal:host-gateway \
    prom/prometheus
```


## promql
redirecionar el puerto 9090 

### validar los paths
/targets
/query

### ejecutar los queries
```prompql
up
{app="prometheus"}
{app="python"}
```