# Instrukcja obs艂ugi aplikacji API


## Uruchamianie serwera API 馃殌

0. Je艣li u偶ywasz venva, aktywuj venva wpisuj膮c w terminalu komend臋:
`[艣cie偶ka do folderu venv]\Scripts\activate`

1. Zainstaluj potrzebne biblioteki zawarte w pliku ***python_server_backend\app\requirements.txt*** za pomoc膮 polecenia:
`pip install -r [艣cie偶ka do pliku requirements.txt]`

2. W terminalu przejd藕 do folderu g艂贸wnego aplikacji ***Rasberek***

3. Uruchom serwer API, za pomoc膮 polecenia:
`uvicorn python_server_backend.app.api:app`.
Je艣li chcesz, 偶eby serwer automatycznie si臋 od艣wie偶a艂 po wprowadzeniu zmian w kodzie, dodaj flag臋 `--reload`:
`uvicorn python_server_backend.app.api:app --reload`

4. 呕eby przej艣膰 do webowego GUI serwisu API, wpisz w przegl膮darce:
http://localhost:8000/docs


## Struktura folder贸w 馃搧

### API: `python_server_backend\app`:

- `routers\` - folder z "ruterami" endpoint贸w dla poszczeg贸lnych czujnik贸w (ruter, czyli grupa endpoint贸w wykorzystuj膮cych np. ten sam url, dziel膮ca ustawienia itp). Tutaj s膮 zdefiniowane funkcje wykonywane po wpisaniu konkretnego URLa w przegl膮darce np.
- `sql_app\schemas\` - zbi贸r model贸w biblioteki pydantic wykorzystywane w    fastapi do wysy艂ania i odbierania danych w konkretnym formacie (np. sposr贸d danych u偶ytkownika, chcemy odczyta膰 tylko imi臋 i email, ale nie has艂o)
- `sql_app\database.py` - skrypt tworz膮cy lub otwieraj膮cy baz臋 danych (je艣li ju偶 istnieje + przechowuje zmienn膮 silnika SQLAlchemy, instancj臋 sesji i bazow膮 instancj臋 obiektu
- `sql_app\models.py` - modele mapuj膮ce tablice bazy danych na "obiekty", kt贸re wykorzystuje sqlalchemy
- `api.py` - g艂贸wny skrypt uruchamiaj膮cy aplikacj臋
- `constants.py` - sta艂e globalne (scie偶ka __ROOT__ - " \python_server_backend\")
- `dependencies.py` - powtarzaj膮ce si臋 funkcje zale偶no艣ci wykorzystywane przez fastapi
- `hashing.py` - funkcje zwi膮zane z haszowaniem w fastapi
- `login.py` - funkcje i endpointy zwi膮zane z logowaniem? (ale nie wiem czy wgl je teraz wykorzystuj臋... chyba jeszcze nie)
- `requirements.txt` - lista bilbiotek wymaganych do uruchomienia aplikacji
- `authentication.py` - wszelkie funkcje na potrzeby logowania/autoryzacji/haszowania i dekodowania hase艂

### Baza danych: `python_server_backend\sqlite_db`:
- `database.db` - plik bazy danych sqlite3
- `database_setup.py` - skrypt tworz膮cy baz臋 i tabele
- `sqlite_operations.py` - listy z poleceniami sql wykorzystywanymi do tworzenia bazy (np. **CREATE TABLE**)

### Skrypty do obs艂ugi czujnik贸w: `python_server_backend\???`:
- TODO :)

## A dzia艂a to tak:

Uruchamiamy serwer poleceniem `uvicorn`, kt贸re wywo艂uje skrypt z pliku `api.py`, kt贸ry robi kilka rzeczy:
   - importuje, a wi臋c wykonuje skrypty z folderu `routers`, kt贸re definiuj膮 funkcje przypisane do konkretnych adres贸w 
   URL - takie zbiory metod nazywa si臋 tu *ruterami*
   - importuje plik `models.py` z `sql_app`, kt贸ry tworzy instancje klas mapuj膮cych tabele bazy danych SQL do bardziej 
   pythonowego formatu
   - wykonuje plik `sql_app.database` i importuje tworzon膮 tam instancj臋 `engine` - tj. silnika SQLAlchemy wykorzystywanego
    do operowania na bazie
   - wywo艂ywany jest skrypt z pliku `sqlite_db\database_setup.py`, kt贸ry tworzy baz臋 je艣li baza nie istnieje
   - zaimportowane wcze艣niej modele s膮 *bindowane* do instancji silnika
   - tworzona jest instancja aplikacji FastAPI: `app = FastAPI()`
   - do `app` dowi膮zywane s膮 rutery z zdefiniowanymi funkcjami dla konkretnych adres贸w URL
