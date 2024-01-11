# Dokumenthandtering-iac
Dokumenthandtering-iac automatiserer arbeidet med å oppdatere eller opprette nye Kafka-topics til Aiven for Team Dokumentløsninger. 
Mer informasjon om vedlikehold og tilgjengelige operasjoner kan finnes på [Managing topics and access in Nais](https://doc.nais.io/persistence/kafka/manage_topics/).  

Dokumenthandtering-iac er basert på [dagpenger-iac](https://github.com/navikt/dagpenger-iac).

For å kunne gjøre forandringer på Aiven-topicene må du ha installert kubectl og satt opp [tilgang til GCP](https://doc.nais.io/basics/access/#authenticate-kubectl).

## Opprettelse av topics
1. Lag en katalog med ønsket topicnavn under [kafka-aiven](kafka-aiven)
2. Legg til filene ```dev-vars.yaml, prod-vars.yaml og topic.yaml``` for templatevariabler til henholdsvis dev og prod, og selve topic-definisjonen.
3. Merge inn en PR og sjekk at Github Action kjører OK.
4. Sjekk at topic-ressursen er opprettet og klar i dev/prod-gcp klustrene

```
kubectl get topic -n teamdokumenthandtering

NAME                 AGE   STATE             FULLY QUALIFIED NAME               CREDENTIALS EXPIRY TIME
privat-dok-notifikasjon   2m26s   RolloutComplete   teamdokumenthandtering.privat-dok-notifikasjon
```

## Endring av ACL-liste for et topic
Legg til en ny bolk i ```topic.yaml``` i mappen under [kafka-aiven](kafka-aiven) med topicnavnet du vil gjøre endringer på.
```
    - team: bidrag
      application: bidrag-dokument-arkiv-feature
      access: read
```

## Større forandringer på topic
### Oppdatering av topickonfigurasjon
For å oppdatere en topic sin konfigurasjon kan ```dev-vars.yaml``` og ```prod-vars.yaml``` justeres. 
Merk at endring av topicnavn i ```topic.yaml``` vil generere en ny topic.

### Slette topic
Følg disse kommandoene for å [slette topic i Aiven](https://doc.nais.io/persistence/kafka/manage_topics/#permanently-deleting-topic-and-data). 
For å også slette tilhørende data kan det hende at en annotasjon må bli commitet før sletting.
Husk at slettingen må gjøres i riktig context - altså at enten ```dev-gcp``` eller ```prod-gcp``` må være valgt:
```
kubectl config use-context <context>
kubectl delete topic <topic> -n teamdokumenthandtering
```

### Kubectl-kommandoer for å se bl.a. ACL-listen til et topic
I dev-gcp:
```shell script
kubectl config use-context dev-gcp
kubectl get topic -n teamdokumenthandtering
kubectl describe topic <topic> -n teamdokumenthandtering 
```

I prod-gcp:
```shell script
kubectl config use-context prod-gcp
kubectl  get topic -n teamdokumenthandtering 
kubectl describe topic <topic> -n teamdokumenthandtering 
```

### Henvendelser
Spørsmål om koden eller prosjektet kan rettes til [Slack-kanalen for \#Team Dokumentløsninger](https://nav-it.slack.com/archives/C6W9E5GPJ).