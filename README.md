# Entrega-Edge
Primeira Imagem:

import requests
import json
import matplotlib.pyplot as plt

url = "http://46.17.108.113:8666/STH/v1/contextEntities/type/Lamp/id/urn:ngsi-ld:Lamp:001/attributes/luminosity?lastN=20"

payload = {}
headers = {
    'fiware-service': 'smart',
    'fiware-servicepath': '/'
}

response = requests.request("GET", url, headers=headers, data=payload)

# Verifica se a resposta foi bem-sucedida (código de status 200)
if response.status_code == 200:
    data = json.loads(response.text)
    luminosity_values = [entry['attrValue'] for entry in data['contextResponses'][0]['contextElement']['attributes'][0]['values']]

    # Calcula os minutos a partir do índice das leituras, limitando a 15 minutos
    minutos = list(range(len(luminosity_values)))[:15]

    # Cria um gráfico de barras com os valores de luminosidade
    plt.bar(minutos, luminosity_values[:15])  # Limita os valores de luminosidade também a 15 minutos
    plt.xlabel('Minutos')
    plt.ylabel('Luminosidade')
    plt.title('Histórico de Luminosidade (últimos 15 minutos)')
    plt.show()
else:
    print("Erro na solicitação: Código de status", response.status_code)







Segunda imagem:



import requests

import matplotlib.pyplot as plt

from datetime import datetime

 

url = "http://46.17.108.113:8666/STH/v1/contextEntities/type/Lamp/id/urn:ngsi-ld:Lamp:001/attributes/luminosity?hLimit=100&hOffset=1&dateFrom=2023-09-22T10:00:00.000&dateTo=2023-09-22T10:15:00.000"

 

payload = {}

headers = {

  'fiware-service': 'smart',

  'fiware-servicepath': '/'

}

 

response = requests.get(url, headers=headers)

 

if response.status_code == 200:

    data = response.json()

    values = [entry['attrValue'] for entry in data['contextResponses'][0]['contextElement']['attributes'][0]['values']]

    timestamps = [datetime.strptime(entry['recvTime'], '%Y-%m-%dT%H:%M:%S.%fZ') for entry in data['contextResponses'][0]['contextElement']['attributes'][0]['values']]

 

    # Agora, vamos criar o gráfico.

    plt.figure(figsize=(10, 6))

    plt.plot(timestamps, values, marker='o', linestyle='-')

    plt.xlabel('Timestamp')

    plt.ylabel('Luminosity')

    plt.title('Luminosity Over Time')

    plt.grid(True)

    plt.xticks(rotation=45)

    plt.tight_layout()

 

    # Exibir o gráfico

    plt.show()

else:

    print(f"Failed to fetch data. Status code: {response.status_code}")







