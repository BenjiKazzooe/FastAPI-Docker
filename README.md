# 🚀 FastAPI Dockerized Machine Learning API

## 📌 Projektbeschreibung

Diese API wurde mit **FastAPI** erstellt und dient zur **automatisierten Klassifikation von Tumoren** basierend auf medizinischen Daten. Sie analysiert eine Liste von numerischen Features und gibt eine Vorhersage, ob ein Tumor **bösartig (Malignant) oder gutartig (Benign)** ist. 

Das Machine-Learning-Modell basiert auf einem **Random Forest Classifier**, der mit **scikit-learn** trainiert wurde. Der Fokus liegt auf hoher Vorhersagegenauigkeit und einfacher Skalierbarkeit durch den Einsatz von **Docker**. 

Durch die Bereitstellung als **Docker-Container** kann die API plattformunabhängig genutzt und problemlos auf verschiedenen Systemen oder in der Cloud bereitgestellt werden.

---

## Voraussetzungen

Bevor du startest, stelle sicher, dass folgende Tools installiert sind:

- **Docker** 🐳 → [Download Docker](https://www.docker.com/products/docker-desktop/)  
  Docker wird benötigt, um den Container mit der API zu erstellen und auszuführen.
- **Python 3.9+** → Falls du das Projekt lokal ohne Docker testen möchtest.

### 🔍 Überprüfen der Docker-Installation

Öffne das Terminal und führe folgenden Befehl aus:

```bash
docker --version
```

Wenn Docker richtig installiert ist, bekommst du eine Ausgabe wie:

```
Docker version 20.10.14, build e0f593
```

Falls Docker nicht installiert ist, folge den Anweisungen auf der [offiziellen Docker-Website](https://www.docker.com/products/docker-desktop/).

---

## 📌 Installation & Setup

### 1. `requirements.txt` erstellen (falls nicht vorhanden)

Die Datei `requirements.txt` enthält alle benötigten Abhängigkeiten für das Projekt. Falls sie fehlt, erstelle sie mit:

```bash
echo -e "fastapi\nuvicorn\nscikit-learn\npandas\nnumpy\nkagglehub" > requirements.txt
```

### 2. `Dockerfile` erstellen (falls nicht vorhanden)

Falls das `Dockerfile` nicht existiert, erstelle es mit:

```bash
touch Dockerfile
nano Dockerfile
```

Füge folgenden Inhalt ein:

```dockerfile
# Basis-Image mit Python 3.9
FROM python:3.9

# Arbeitsverzeichnis im Container setzen
WORKDIR /app

# Anforderungen kopieren und installieren
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

# Projektdateien kopieren
COPY . .

# Port für FastAPI freigeben
EXPOSE 8000

# Startbefehl für die API
CMD ["uvicorn", "api:app", "--host", "0.0.0.0", "--port", "8000", "--reload"]
```

Speichere die Datei (`CTRL + X`, dann `Y` und `Enter` in Nano).

---

## 🐳 Docker-Container bauen & starten

### 1. Docker-Image bauen

Erstelle das Docker-Image mit folgendem Befehl:

```bash
docker build -t fastapi-app .
```

Falls es Probleme gibt, versuche es ohne Cache:

```bash
docker builder prune -a
docker build --no-cache -t fastapi-app .
```

### 2. Docker-Container starten

Starte den Container mit folgendem Befehl:

```bash
docker run -d -p 8000:8000 --name fastapi-container fastapi-app
```

📌 **Erklärung:**

- `-d` → Container läuft im Hintergrund
- `-p 8000:8000` → Verbindet Port 8000 des Containers mit Port 8000 des Hosts
- `--name fastapi-container` → Gibt dem Container einen Namen
- `fastapi-app` → Name des zuvor gebauten Docker-Images

### 🔎 3. Überprüfen, ob der Container läuft

```bash
docker ps
```

Falls dein Container läuft, siehst du eine Ausgabe wie:

```
CONTAINER ID   IMAGE         COMMAND                  CREATED        STATUS        PORTS                    NAMES
3de1c64523d7   fastapi-app   "uvicorn api:app --h…"   10 seconds ago   Up 9 seconds   0.0.0.0:8000->8000/tcp   fastapi-container
```

Falls dein Container **nicht läuft**, Logs anzeigen:

```bash
docker logs fastapi-container
```

---

## 🚀 API testen

### 1. Mit Swagger UI (`http://127.0.0.1:8000/docs`)

1. Öffne [Swagger UI](http://127.0.0.1:8000/docs) im Browser.
2. Klicke auf `POST /predict`.
3. Wähle **"Try it out"**, gib die Testdaten ein und drücke **"Execute"**.

### 2. Mit `curl` (Terminal)

```bash
curl -X 'POST' 'http://127.0.0.1:8000/predict' \
     -H 'Content-Type: application/json' \
     -d '{"features": [10.38, 17.77, 122.8, 1001.0, 0.1184, 0.2776, 0.3001, 0.1471, 0.2419, 0.07871, 
                      0.2199, 0.8663, 2.011, 17.95, 0.0141, 0.0461, 0.05999, 0.01738, 0.0233, 0.00743, 
                      21.37, 25.49, 137.9, 1421.0, 0.1959, 0.428, 0.55, 0.2663, 0.4601, 0.1189, 0.1]}'
```

Erwartete Antwort:

```json
{
  "prediction": "Malignant",
  "probability_malignant": 0.72
}
```

---

## 🛠 Nützliche Docker-Befehle

| Befehl                          | Beschreibung                             |
| ------------------------------- | ---------------------------------------- |
| `docker ps`                     | Laufende Container anzeigen              |
| `docker ps -a`                  | Alle Container anzeigen (auch gestoppte) |
| `docker stop fastapi-container` | Container stoppen                        |
| `docker rm fastapi-container`   | Container löschen                        |
| `docker logs fastapi-container` | Logs eines Containers anzeigen           |

---





