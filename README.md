# M324 DevOps
## Aufgabe 1
1. Ich habe ein GitHub-Repository erstellt
2. Danach habe ich das Repository geklont
3. Ich habe das Repository m346-ref-card-02 heruntergeladen.
4. In diesem Repository habe ich ein Dockerfile erstellt.
```Dockerfile
FROM node:latest
WORKDIR /app
RUN git clone https://github.com/evanlueber/324_devops.git
WORKDIR /app/324_devops/aufgabe1
EXPOSE 3000
ENTRYPOINT ["npm", "run", "start"]
```
5. Danach habe ich ein Docker-Hub Repository vorbereitet.
6. Im Anschluss habe ich eine GitHub Action erstellet, die ausgeführt wir, wenn etwas auf das Repository gepusht wird.
```yml	
# Workflow wird bei Push-Events auf den Hauptzweig ausgelöst
name: "Build and Push to Server"
on:
  push:
    branches: [main]

# Umgebungsvariablen definieren
env:
  working-directory: ./  # Arbeitsverzeichnis für den Job

# Jobs definieren
jobs:
  # Job wird bei jedem Push-Event ausgeführt
  if_pushed:
    runs-on: ubuntu-latest
    steps:
      # Schritt 1: Repository auschecken
      - name: Repository auschecken
        uses: actions/checkout@v4

      # Schritt 2: Bei Docker Hub mit den bereitgestellten Anmeldedaten anmelden
      - name: Bei Docker Hub anmelden
        uses: docker/login-action@f4ef78c080cd8ba55a85445d5b36e214a81df20a
        with:
          username: ${{ secrets.DOCKER_USERNAME }}
          password: ${{ secrets.DOCKER_PASSWORD }}

      # Schritt 3: Metadaten (Tags, Labels) für das Docker-Image extrahieren
      - name: Metadaten (Tags, Labels) für Docker extrahieren
        id: meta
        uses: docker/metadata-action@9ec57ed1fcdbf14dcef7dfbe97b2010124a938b7
        with:
          images: evanlueber/m324-devops

      # Schritt 4: Docker-Image bauen und pushen
      - name: Docker-Image bauen und pushen
        uses: docker/build-push-action@3b5e8027fcad23fda98b2e3ac259d8d67585f671
        with:
          context: .
          file: ./Dockerfile
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          labels: ${{ steps.meta.outputs.labels }}
```
7. Danach habe ich auf Github in meinem Repository ein Secret erstellt, in dem ich meine Docker-Hub Anmeldedaten hinterlegt habe.