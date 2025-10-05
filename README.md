# FASE3
script simulador_smart_office.py e o arquivo smart_office_data.csv. 

import pandas as pd
import numpy as np
from datetime import datetime, timedelta

# Período de simulação
inicio = datetime(2024, 10, 1)
fim = inicio + timedelta(days=7)
intervalo = timedelta(minutes=15)

timestamps = []
temperatura = []
luminosidade = []
ocupacao = []
sensor_ids = []

t = inicio
while t < fim:
    hora = t.hour
    # Temperatura: mais fria à noite
    temp = np.random.normal(24 if 8 <= hora <= 18 else 20, 1.5)
    # Luminosidade: zero à noite
    lux = np.random.normal(300 if 7 <= hora <= 18 else 0, 50)
    # Ocupação: mais alta no horário comercial
    occ = 1 if 8 <= hora <= 18 and np.random.rand() > 0.2 else 0

    timestamps.append(t)
    temperatura.append(round(temp, 1))
    luminosidade.append(max(0, round(lux, 0)))
    ocupacao.append(occ)
    sensor_ids.append(f"SENSOR_{np.random.randint(1,4)}")
    t += intervalo

df = pd.DataFrame({
    "timestamp": timestamps,
    "sensor_id": sensor_ids,
    "temperatura": temperatura,
    "luminosidade": luminosidade,
    "ocupacao": ocupacao
})

df.to_csv("smart_office_data.csv", index=False)
print("Arquivo smart_office_data.csv gerado com sucesso!")

