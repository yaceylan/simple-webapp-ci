# Simple WebApp CI mit Jenkins

Dieses Projekt zeigt die Einrichtung einer einfachen Jenkins CI-Pipeline für eine React-Webanwendung, die mit Vite erstellt wurde.  
Die Pipeline führt automatisch den Checkout und den Build-Prozess der Anwendung aus, sobald Änderungen im GitHub-Repository auftreten.

---

## Projektstruktur

- `simple-webapp/` – React-App (Vite-basiert)
- `Jenkinsfile` – Pipeline-Definition für Jenkins
- `screenshots/` – Dokumentation der Aufgabe (siehe unten)

---

## Webanwendung lokal starten

### Voraussetzungen:
- Node.js (empfohlen: v22+ oder v23)
- npm

### Schritte:
```bash
cd simple-webapp
npm install
npm run dev
```

### Build-Prozess:
```bash
npm run build
```

---

## Jenkins lokal starten (Docker)

```bash
docker run \
  --name my-simple-jenkins \
  -p 8080:8080 \
  -v jenkins_home:/var/jenkins_home \
  jenkins/jenkins:lts
```

Dann im Browser: `http://localhost:8080`

### Setup-Schritte:
- Admin-Passwort aus Docker-Logs kopieren
- „Suggested Plugins“ installieren
- Admin-Benutzer anlegen
- Job erstellen (Pipeline → SCM: Git → Branch: main → Script: Jenkinsfile)

---

## Jenkinsfile (CI-Definition)

```groovy
pipeline {
  agent any

  tools {
    nodejs 'node-22'
  }

  stages {
    stage('Checkout') {
      steps {
        checkout scm
      }
    }
    stage('Build') {
      steps {
        sh 'npm ci'
        sh 'npm run build'
      }
    }
  }
}
```

---

## Screenshots

| Beschreibung              | Datei                                      |
|---------------------------|--------------------------------------------|
| Jenkins läuft             | screenshots/01_jenkins_dashboard.png       |
| Pipeline-Konfiguration    | screenshots/02_jenkins_pipeline_konfiguration.png |
| Jenkinsfile-Inhalt        | screenshots/03_jenkinsfile_inhalt.png      |
| Erfolgreicher Build       | screenshots/04_build_console_output.png    |

---

## Reflexion 

### Welche Schritte waren notwendig, um Jenkins lokal mit Docker zum Laufen zu bringen?

Zuerst musste Docker installiert und gestartet sein. Dann wurde der Jenkins-Container mit einem Docker-Befehl (`docker run ...`) ausgeführt. Im Browser wurde die Jenkins-Weboberfläche unter `localhost:8080` geöffnet, das Initial-Passwort eingegeben, empfohlene Plugins installiert und ein Admin-Benutzer erstellt.

---

### Was ist der Zweck der Datei Jenkinsfile, und wo muss sie im Verhältnis zu deinem Anwendungscode liegen?

Die Datei `Jenkinsfile` beschreibt die Schritte der Pipeline (z. B. Checkout und Build). Sie muss im Wurzelverzeichnis des Projekts liegen – also dort, wo auch die `package.json` deiner Webanwendung liegt.

---

### Beschreibe die zwei Hauptstages, die du in deiner Pipeline definiert hast, und was der Hauptzweck der Steps in jeder Stage ist.

- **Stage "Checkout"**: Lädt den aktuellen Code aus dem GitHub-Repository in Jenkins.
- **Stage "Build"**: Installiert die Abhängigkeiten (`npm ci`) und baut die Anwendung (`npm run build`).

---

### Wie hast du in Jenkins konfiguriert, von welchem Git-Repository und welchem Branch der Code für die Pipeline geholt werden soll?

Im Pipeline-Job wurde „Pipeline script from SCM“ ausgewählt, als SCM „Git“ angegeben, die URL des GitHub-Repos eingefügt und der Branch `*/main` gesetzt.

---

### Was ist der Unterschied zwischen dem checkout scm Step in deiner Pipeline und dem git clone Befehl, den du manuell im Terminal ausführen würdest?

`checkout scm` ist ein Jenkins-spezifischer Schritt, der automatisch mit den im Job definierten Repository-Einstellungen arbeitet. `git clone` ist ein manueller Befehl ohne Jenkins-Kontext und ohne Integration mit z. B. Jenkins-Credentials oder Job-Konfiguration.

