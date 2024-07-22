# NEXT LEVEL WEEK - Rocketseat
## PYTHON

Nesta ediçao do Next Level Week feita pela Rocketseat em julho de 2024 foi mostrada a criação de uma API utilizando Flask. O banco de dados utilizado para persistir os dados foi o SQLite.

Foi criado também um container Docker para executar a aplicação.

# Instalação

Será necessário instalar o flask e o pytest

```
pip install flask
pip install pytest
```

Para executar a aplicação:

```
python app.py
```

A API poderá ser acessada a partir do endereço:

**http://127.0.0.1:3000/**


Para executar os testes, na raiz do projeto, rodar o seguinte comando:

```
pytest
```

Os arquivos de teste terminar com _test.py


# Rotas da aplicação

Para ver as rotas da aplicação, execute o segiunte comando:

```
flask routes
```

Os endereços estão disponíves em 

**src - main - routes - trips_routes.py**


Endpoint                           | Methods  | Rule
---------------------------------- | -------- | ---------------------------------------- 
static                             | GET      | /static/<path:filename>
trip_routes.confirm_participant    | PATCH    | /participants/<participantId>/confirm
trip_routes.confirm_trip           | GET      | /trips/<tripId>/confirm
trip_routes.create_activity        | POST     | /trips/<tripId>/activities
trip_routes.create_trip            | POST     | /trips
trip_routes.create_trip_link       | POST     | /trips/<tripId>/links
trip_routes.find_trip              | GET      | /trips/<tripId>
trip_routes.find_trip_link         | GET      | /trips/<tripId>/links
trip_routes.get_trip_activities    | GET      | /trips/<tripId>/activities
trip_routes.get_trip_participants  | GET      | /trips/<tripId>/participants
trip_routes.invite_to_trip         | POST     | /trips/<tripId>/invites

********************************************************************

# DOCKER

Para criar a imagem da aplicação:

```
docker build -t flask-app .
```

Para executar a aplicação a partir do container Docker

```
docker run -p 3000:3000 flask-app
```

# TESTES DOS ENDPOINTS

**[POST]**

http://127.0.0.1:3000//trips 

Request:
```
{
"id": "1",
"destination": "Teste",
"start_date": "2024-07-10",
"end_date": "2024-07-12",
"owner_name": "Teste",
"owner_email": "teste@teste.com.br"
}
```

Response:
```
{
    "id": "8c9c1c5b-38eb-41a0-aeb2-a96e2dd3054c"
}
```

**[GET]**

http://127.0.0.1:3000//trips/8c9c1c5b-38eb-41a0-aeb2-a96e2dd3054c 

Response:
```
{
  "trip": {
    "destination": "Teste",
    "ends_at": "2024-07-12",
    "id": "8c9c1c5b-38eb-41a0-aeb2-a96e2dd3054c",
    "starts_at": "2024-07-10",
    "status": null
  }
}
```

**[GET]**

http://127.0.0.1:3000//trips/8c9c1c5b-38eb-41a0-aeb2-a96e2dd3054c/confirm

Response:
```
  "trip": {
    "destination": "Teste",
    "ends_at": "2024-07-12",
    "id": "8c9c1c5b-38eb-41a0-aeb2-a96e2dd3054c",
    "starts_at": "2024-07-10",
    "status": null
  }
}
```

**[POST]**

http://127.0.0.1:3000//trips/8c9c1c5b-38eb-41a0-aeb2-a96e2dd3054c/links

Request:
```
{
"id":"1",
"trip_id":"8c9c1c5b-38eb-41a0-aeb2-a96e2dd3054c",
"link":"https://www.google.com",
"title":"Google"
}
```

Responsa:
```
{
    "linkId": "21278fdc-5f0f-4078-986f-e89276a5312a"
}
```

**[GET]**

http://127.0.0.1:3000/trips/8c9c1c5b-38eb-41a0-aeb2-a96e2dd3054c/links

Response:
```
{
  "links": []
}
```

**[POST]**

http://127.0.0.1:3000/trips/8c9c1c5b-38eb-41a0-aeb2-a96e2dd3054c/invites

Request:
```
{
"id":"1",
"trip_id":"8c9c1c5b-38eb-41a0-aeb2-a96e2dd3054c",
"emails_to_invite_id":"1",
"name":"teste",
"email":"teste2@teste.com.br"
}
```

Response:
```
{
    "participant_id": "7d46e996-8765-436e-8f16-7607c7630515"
}
```

**[POST]**

http://127.0.0.1:3000/trips/8c9c1c5b-38eb-41a0-aeb2-a96e2dd3054c/activities"

Request:
```
{
"id":"1",
"trip_id":"8c9c1c5b-38eb-41a0-aeb2-a96e2dd3054c",
"title":"Atividade Teste",
"occurs_at":"2024-10-01"
}
```

Response:
```
{
    "activityId": "4125f4d0-f70f-4ac8-8352-ab4a2fe62128"
}
```

**[GET]**

http://127.0.0.1:3000/trips/8c9c1c5b-38eb-41a0-aeb2-a96e2dd3054c/participants

Response:
```
{
  "participants": [
    {
      "email": "teste2@teste.com.br",
      "id": "7d46e996-8765-436e-8f16-7607c7630515",
      "is_confirmed": null,
      "name": "teste"
    }
  ]
}
```

**[GET]**

http://127.0.0.1:3000/trips/8c9c1c5b-38eb-41a0-aeb2-a96e2dd3054c/activities

Response:
```
{
  "activities": [
    {
      "id": "4125f4d0-f70f-4ac8-8352-ab4a2fe62128",
      "occurs_at": "2024-10-01",
      "title": "Atividade Teste"
    }
  ]
}
```

**[PATCH]**

http://127.0.0.1:3000/participants/7d46e996-8765-436e-8f16-7607c7630515/confirm

(response: vazio)



# Estrutura do Flask

A estrutura da aplicação é composta das seguintes  pastas:


src
- controllers
- drivers
- main
  - routes
  - server
- models
  - repositories
  - settings


A seguir uma descrição e um exemplo do conteúdo de cada pasta.

### controllers
Um controller para cada ação (exemplo)

```
from typing import Dict

class TripFinder:
    def __init__(self, trips_repository) -> None:
        self.__trips_repository = trips_repository

    def find_trip_details(self, trip_id) -> Dict:
        try:
            trip = self.__trips_repository.find_trip_by_id(trip_id)
            if not trip: raise Exception("No Trip Found")

            return {
                "body": {
                    "trip": {
                        "id": trip[0],
                        "destination": trip[1],
                        "starts_at": trip[2],
                        "ends_at": trip[3],
                        "status": trip[6]
                    }
                },
                "status_code": 200
            }
        except Exception as exception:
            return {
                "body": { "error": "Bad Request", "message": str(exception) },
                "status_code": 400
            }

```

### drivers
--- email_sender.py
(email disparado pelo sistema)

### main
#### routes

Todas as rotas seguem este formato

```
from flask import jsonify, Blueprint, request

trips_routes_bp = Blueprint("trip_routes", __name__)

#Importação de Controllers

from src.controllers.trip_creator import TripCreator
(...)
```
Importação de Repositorios

```
from src.models.repositories.trips_repository import TripsRepository
from src.models.repositories.emails_to_invite_repository import EmailsToInviteRepository
(....)
```

Importando o gerente de conexões

```
from src.models.settings.db_connection_handler import db_connection_handler

@trips_routes_bp.route("/trips", methods=["POST"])
def create_trip():
    conn = db_connection_handler.get_connection()
    trips_repository = TripsRepository(conn)
    emails_repository = EmailsToInviteRepository(conn)
    controller = TripCreator(trips_repository, emails_repository)

    response = controller.create(request.json)

    return jsonify(response["body"]), response["status_code"]
```

### main
#### server

Registra as rotas no servidor

```
from flask import Flask
from src.main.routes.trips_routes import trips_routes_bp

app = Flask(__name__)

app.register_blueprint(trips_routes_bp)
```

### models
#### repositories

Para cada tabela do banco tem um repositorio e um arquivo de testes (exemplo)

```
from typing import Dict, Tuple
from sqlite3 import Connection

class TripsRepository:
    def __init__(self, conn: Connection) -> None:
        self.__conn = conn

    def create_trip(self, trips_infos: Dict) -> None:
        cursor = self.__conn.cursor()
        cursor.execute(
            '''
                INSERT INTO trips
                    (id, destination, start_date, end_date, owner_name, owner_email)
                VALUES
                    (?, ?, ?, ?, ?, ?)
            ''', (
                trips_infos["id"],
                trips_infos["destination"],
                trips_infos["start_date"],
                trips_infos["end_date"],
                trips_infos["owner_name"],
                trips_infos["owner_email"]
            )
        )
        self.__conn.commit()

    def find_trip_by_id(self, trip_id: str) -> Tuple:
        cursor = self.__conn.cursor()
        cursor.execute(
            '''SELECT * FROM trips WHERE id = ?''', (trip_id,)
        )
        trip = cursor.fetchone()
        return trip

    def update_trip_status(self, trip_id: str) -> None:
        cursor = self.__conn.cursor()
        cursor.execute(
            '''
                UPDATE trips
                    SET status = 1
                WHERE
                    id = ?
            ''', (trip_id,)
        )
        self.__conn.commit()
```

 Dentro da pasta repositores existem arquivos de teste. Os arquivos de teste tem o sufixo _test.py

```
import pytest # type: ignore
import uuid
from datetime import datetime, timedelta
from .trips_repository import TripsRepository
from src.models.settings.db_connection_handler import db_connection_handler

db_connection_handler.connect()
trip_id = str(uuid.uuid4())

@pytest.mark.skip(reason="interacao com o banco")
def test_create_trip():
    conn = db_connection_handler.get_connection()
    trips_repository = TripsRepository(conn)

    trips_infos = {
        "id": trip_id,
        "destination": "Osasco",
        "start_date": datetime.strptime("02-01-2024", "%d-%m-%Y"),
        "end_date": datetime.strptime("02-01-2024", "%d-%m-%Y") + timedelta(days=5),
        "owner_name": "Osvaldo",
        "owner_email": "osvaldo@email.com"
    }

    trips_repository.create_trip(trips_infos)

@pytest.mark.skip(reason="interacao com o banco")
def test_find_trip_by_id():
    conn = db_connection_handler.get_connection()
    trips_repository = TripsRepository(conn)

    trip = trips_repository.find_trip_by_id(trip_id)
    print(trip)

@pytest.mark.skip(reason="interacao com o banco")
def test_update_trip_status():
    conn = db_connection_handler.get_connection()
    trips_repository = TripsRepository(conn)

    trips_repository.update_trip_status(trip_id)
```

### models
#### settings

Conexão com o banco de dados relacional sqlite 

```
import sqlite3
from sqlite3 import Connection

class DbConnectionHandler:
    def __init__(self) -> None:
        self.__connection_string = "storage.db"
        self.__conn = None

    def connect(self) -> None:
        conn = sqlite3.connect(self.__connection_string, check_same_thread=False)
        self.__conn = conn

    def get_connection(self) -> Connection:
        return self.__conn

db_connection_handler = DbConnectionHandler()
```
