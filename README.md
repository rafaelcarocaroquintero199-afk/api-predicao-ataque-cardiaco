# api-predicao-ataque-cardiaco

> Descrição:hola mundo : adan ema andrade gregorio rodrigez gonzalez ulices rodrigez gonzalez erick ema andrade amelia ema andrade adan ema andrade gregorio rodrigez gonzalez import numpy as np
import matplotlib.pyplot as plt
from matplotlib.animation import FuncAnimation
import time
import random

class SimuladorCardiaco:
def init(self):
self.frecuencia = 75 # latidos por minuto
self.presion_sistolica = 120
self.presion_diastolica = 80
self.ritmo_normal = True
self.tiempo = 0
self.datos_ecg = []
self.datos_presion = []
self.tiempos = []

def generar_latido_normal(self, tiempo):
    """Genera una forma de onda de ECG normal"""
    # Onda P
    if tiempo % 1.0 < 0.1:
        return 0.5 * np.sin(10 * np.pi * tiempo)
    # Complejo QRS
    elif tiempo % 1.0 < 0.2:
        return 1.0 * np.sin(20 * np.pi * tiempo)
    # Onda T
    elif tiempo % 1.0 < 0.3:
        return 0.3 * np.sin(8 * np.pi * tiempo)
    else:
        return 0.0
        
def generar_arritmia(self, tiempo):
    """Genera diferentes tipos de arritmias"""
    tipos_arritmia = ['taquicardia', 'bradicardia', 'fibrilacion']
    tipo = random.choice(tipos_arritmia)
    
    if tipo == 'taquicardia':
        return 1.2 * np.sin(25 * np.pi * tiempo)
    elif tipo == 'bradicardia':
        return 0.3 * np.sin(6 * np.pi * tiempo)
    else:  # fibrilacion
        return random.uniform(-0.5, 0.5)
        
def simular_latido(self):
    """Simula un latido cardíaco"""
    self.tiempo += 0.01
    
    if self.ritmo_normal:
        ecg = self.generar_latido_normal(self.tiempo)
    else:
        ecg = self.generar_arritmia(self.tiempo)
        
    # Variación natural de la presión arterial
    presion = self.presion_diastolica + (
        (self.presion_sistolica - self.presion_diastolica) * 
        abs(np.sin(2 * np.pi * self.tiempo))
    )
    
    self.datos_ecg.append(ecg)
    self.datos_presion.append(presion)
    self.tiempos.append(self.tiempo)
    
    return ecg, presion
    
def cambiar_ritmo(self):
    """Cambia entre ritmo normal y arritmia"""
    self.ritmo_normal = not self.ritmo_normal
    return "Normal" if self.ritmo_normal else "Arritmia"
    
def cambiar_frecuencia(self, nueva_frecuencia):
    """Cambia la frecuencia cardíaca"""
    self.frecuencia = max(40, min(200, nueva_frecuencia))
    return self.frecuencia
def main():
# Crear el simulador
corazon = SimuladorCardiaco()

# Configurar la gráfica
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(12, 8))
fig.suptitle('SIMULADOR CARDIACO - MONITOR EN TIEMPO REAL')

# Configurar ECG
ax1.set_title('Electrocardiograma (ECG)')
ax1.set_xlabel('Tiempo (segundos)')
ax1.set_ylabel('Amplitud')
ax1.set_ylim(-1.5, 1.5)
ax1.grid(True)

# Configurar presión arterial
ax2.set_title('Presión Arterial')
ax2.set_xlabel('Tiempo (segundos)')
ax2.set_ylabel('mmHg')
ax2.set_ylim(60, 180)
ax2.grid(True)

# Líneas iniciales
line_ecg, = ax1.plot([], [], 'b-', linewidth=2)
line_presion, = ax2.plot([], [], 'r-', linewidth=2)

def init():
    line_ecg.set_data([], [])
    line_presion.set_data([], [])
    return line_ecg, line_presion
    
def update(frame):
    # Generar nuevo latido
    ecg, presion = corazon.simular_latido()
    
    # Mantener solo los últimos 200 puntos para visualización
    if len(corazon.tiempos) > 200:
        tiempos_show = corazon.tiempos[-200:]
        ecg_show = corazon.datos_ecg[-200:]
        presion_show = corazon.datos_presion[-200:]
    else:
        tiempos_show = corazon.tiempos
        ecg_show = corazon.datos_ecg
        presion_show = corazon.datos_presion
    
    # Actualizar gráficos
    line_ecg.set_data(tiempos_show, ecg_show)
    line_presion.set_data(tiempos_show, presion_show)
    
    # Ajustar límites del eje x
    for ax in [ax1, ax2]:
        ax.set_xlim(max(0, corazon.tiempo - 20), corazon.tiempo + 1)
    
    # Actualizar título con información en tiempo real
    estado = "Normal" if corazon.ritmo_normal else "Arritmia"
    ax1.set_title(f'ECG - Frecuencia: {corazon.frecuencia} lpm - Estado: {estado}')
    
    return line_ecg, line_presion

# Crear animación
ani = FuncAnimation(fig, update, frames=1000, 
                   init_func=init, blit=True, interval=50)

plt.tight_layout()
plt.show()

# Demostración de funcionalidades
print("=== SIMULADOR CARDIACO ===")
print("Funcionalidades disponibles:")
print("1. Cambiar ritmo (normal/arritmia)")
print("2. Ajustar frecuencia cardíaca")
print("3. Monitoreo en tiempo real")

# Ejemplo de uso
print(f"\nFrecuencia inicial: {corazon.frecuencia} lpm")
print("Cambiando a arritmia...")
time.sleep(2)
print(f"Estado: {corazon.cambiar_ritmo()}")

print("Aumentando frecuencia...")
time.sleep(1)
print(f"Nueva frecuencia: {corazon.cambiar_frecuencia(120)} lpm")
if name == "main":
main()
### Esta API contém views utilizando ModelViewSet que é capaz de realizer o CRUD (Create, Read, Update, Delete) completo em poucas linhas, com código limpo e eficiente, além de usar SimpleRouter nas rotas, tornando as urls mais padronizadas. A rede neural original em códigos não está presente neste repositório, ela apenas foi carregada a partir de um json contendo a sua estrutura e outro arquivo contendo os pesos (no arquivo "ataque_cardiaco_carregar.py" é possível saber de mais detalhes). A rede neural utiliza recursos do Deep Learning (Aprendizado profundo) e foi a primeira que fiz sozinho. A rede neural foi treinada com dataset obtido no Kaggle e por não possuir muitas linhas de dados (tem 303 linhas e 14 colunas) não foi tão eficiente para o treinamento com a precisão chegando em torno dos 88%, porém, sendo apenas um projeto teste, foi bem produtivo. A base de dados para registros da API é apenas um SQLite simples e será guardado localmente, se quiser utilizar outro banco de dados pode conectar nas settings da API. Caso obtenha interesse no código da rede neural, basta entrar em contato comigo, aqui está meu linkedin: https://www.linkedin.com/in/higor-ribeiro-araujo-892008211/.

> Execução:

### - Você pode executar de dois modos: por máquina virtual ou por docker.
### - No ambiente Windows, basta seguir os seguintes passos no terminal:
1. criar uma venv:
~~~python
python -m venv venv
~~~
2. executar a venv:
~~~python
./venv/scripts/activate
~~~
3. Por conta do docker o "tensorflow" não foi incluído no requirements.py, portanto rode o seguinte comando: 
~~~
pip install tensorflow
~~~
4. Instale todas as outras bibliotecas necessárias:
~~~
pip install -r requirements.txt
~~~
5. Executar o projeto:
~~~python
python manage.py runserver
~~~
#### obs: Não irei fazer passos para outros OS. Caso tenha problemas, pesquise na internet ou consulte alguma IA.
### - Para executar por meio do docker, basta digitar o seguinte comando no terminal:
~~~
docker compose up
~~~
#### Se quiser executar em segundo plano: 
~~~
docker compose up -d
~~~
### - Você pode abrir o swagger do projeto no navegador, pela rota: api/schema/swagger-ui/
### - Utilizando o swagger, faça o registro do paciente em "users" e posteriormente pode realizar a consulta do mesmo em "possibility_attack"
### - Para registrar o usuário basta:
~~~
{
  "name": "Higor",
  "cpf": "888.888.888-99"
}
~~~
#### Obs: O cpf será utilizado apenas para identificar o paciente e diferenciá-lo dos demais, portanto não necessita ser o dado real.
### - Já para a consulta, é necessário mais dados e detalhes, por exemplo:
~~~
{
  "age": 22,
  "sex": 1,
  "cp": 1,
  "trtbps": 110,
  "chol": 130,
  "fbs": 0,
  "restecg": 0,
  "thalachh": 150,
  "exng": 0,
  "oldpeak": 2.1,
  "slp": 1,
  "caa": 2,
  "thall": 3,
  "user_id": 1
}
~~~
> Mais Detalhes:
### - Para entender melhor o que significa cada uma das chaves citadas acima, deixarei abaixo os detalhes:
#### Idade : Idade do paciente (Inteiro, anos)
#### Sexo : Gênero (categórico; 1 = masculino, 0 = feminino)
#### CP : Tipo de dor no peito (categórica)
##### 1: angina típica
##### 2: angina atípica
##### 3: dor não anginosa
##### 4: assintomático
#### Trestbps : Pressão arterial em repouso (Inteiro, mm Hg)
#### Col : Colesterol sérico (Inteiro, mg/dl)
#### FBS : Glicemia de jejum > 120 mg/dl (categórico; 1 = verdadeiro, 0 = falso)
#### Restecg : Resultados eletrocardiográficos em repouso (categórico)
##### 0: normal
##### 1: Anormalidade da onda ST-T
##### 2: hipertrofia ventricular esquerda provável ou definitiva
#### Thalach : Frequência cardíaca máxima alcançada (Inteiro)
#### Exng : Angina induzida por exercício (categórico; 1 = sim, 0 = não)
#### Oldpeak : Depressão do segmento ST induzida pelo exercício em relação ao repouso (Inteiro)
#### Slp : Declive do segmento ST de pico do exercício (categórico)
##### 1: ascendente
##### 2: plano
##### 3: declive
#### Caa : Número de vasos principais coloridos pela fluoroscopia (Inteiro; 0-3)
#### Thal : Talassemia (Categórica)
##### 3: normal
##### 6: defeito corrigido
##### 7: defeito reversível
#### User_id : id do usuário, pode ser encontrado na lista de usuários registrados.
