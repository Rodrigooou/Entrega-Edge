# Entrega-Edge

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

    ![image](https://github.com/Rodrigooou/Entrega-Edge/assets/126582881/3452e23f-c787-4059-87e9-c642811b6db1)
