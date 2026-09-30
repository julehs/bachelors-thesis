Questa tesi documenta la modernizzazione della pipeline di Continuous Integration e
Continuous Delivery di optovera-be, il backend della piattaforma Optovera di SOBEREYE,
strutturato come monorepo Maven che raccoglie nove microservizi. Nella configurazione
di partenza la pipeline trattava ogni modifica come impatto sull’intero sistema: qualunque
cambiamento ricostruiva e ripubblicava le immagini Docker di tutti i servizi, mentre il
rilascio in produzione avveniva manualmente, aggiornando a mano i tag delle immagini in
un repository Terraform e lanciando un apply. Di conseguenza non era possibile rilasciare
in produzione un singolo microservizio in modo indipendente.
Il lavoro introduce un’analisi dell’impatto delle modifiche che, a partire dai file toccati,
individua i soli microservizi coinvolti e rende selettivi la build, la containerizzazione e la
pubblicazione delle immagini su Amazon ECR, oltre all’analisi statica della qualità con
SonarQube. Un refactoring architetturale mirato rimuove l’accoppiamento nel codice
condiviso tra microservizi, condizione necessaria affinché la selettività sia reale e concettualmente corretta, non soltanto apparente. L’autenticazione verso AWS è migrata da
credenziali statiche a un meccanismo OIDC con credenziali temporanee, e un nuovo job
automatizza il deploy in produzione su Amazon ECS, disaccoppiando il ciclo di rilascio
applicativo dal provisioning infrastrutturale. Un ulteriore controllo in pipeline presidia la
disciplina di un microservizio impattato per singola modifica.
La soluzione è stata integrata e dimostrata con un primo rilascio reale e selettivo nella
pipeline di produzione. Sulla pipeline in questione, la selettività riduce significamente la
tempistica della pubblicazione delle immagini di circa del -34% quando un solo microservizio è impattato, e del -92% nello scenario privo di impatto. Vengono infine discussi i limiti
residui, tra cui la granularità del rilascio sui push che accorpano più richieste di merge e
la persistenza di risorse dati condivise fra i servizi.


Parole chiave: CI/CD, microservizi, monorepo, analisi d’impatto, build selettiva, deploy
indipendente, GitLab CI/CD, Docker, Amazon ECS, Terraform, OIDC, DevOps.
