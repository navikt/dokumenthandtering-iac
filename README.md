# dokumenthandtering-iac

# Funksjonelle Krav
Denne applikasjonen automatisere arbeidet for å oppdatere eller opprette nye kafka topics til Aiven. For å slette topics så må det gjøres manuelt med å følge denne linken: [nais-aiven](https://doc.nais.io/addons/kafka/#delete-schema) 

Denne applikasjonen er basert på [dagpenger-iac](https://github.com/navikt/dagpenger-iac)

# Forutsetninger
* Tilgang for å deploye til teamdokumenthandtering på github
* Sette opp lokalt tilgang for gcp for [kubectl](https://doc.nais.io/basics/access/#authenticate-kubectl)

# Opprettelse av topics

1. Lag en katalog med ønsket topic navn under [kafka-aiven](kafka-aiven)
2. Legg til filene ```dev-vars.yaml, prod-vars.yaml og topic.yaml``` for henholdsvis template variabler til dev, prod og selve topic definisjonen.
3. Sjekk GHA action kjører OK.
4. Sjekk at topics er ressursen er opprettet og klar i dev/prod-gcp klustrene

```
kubectl -n teamdokumenthandtering get topic

NAME                 AGE   STATE             FULLY QUALIFIED NAME               CREDENTIALS EXPIRY TIME
privat-dok-notifikasjon   2m26s   RolloutComplete   teamdokumenthandtering.privat-dok-notifikasjon
```

# Oppdatering av topics
For å oppdatere er det å gjøre ønsket endringene til filene ```dev-vars.yaml, prod-vars.yaml og topic.yaml```. 
Endring av navn av topic vil genere en ny topic, dette må gjøres forsiktig og passe på data ikke blir borte.

# Slette en topic
Følg disse komandoene for å slette topic ifra Aiven, må gjøres i  context ```dev-gcp og prod-gcp```:

```
kubectl config use-context <context>
kubectl delete topic <topic> -n teamdokumenthandtering
```

### Kubectl
For dev-gcp:
```shell script
kubectl config use-context dev-gcp
kubectl get -n teamdokumenthandtering topic
kubectl -n teamdokumenthandtering describe topic <topic>
```

For prod-gcp:
```shell script
kubectl config use-context prod-gcp
kubectl  get -n teamdokumenthandtering topic 
kubectl -n teamdokumenthandtering describe topic <topic>
```

## Henvendelser
Spørsmål koden eller prosjekttet kan rettes til Team Dokumentløsninger på:
* [\#Team Dokumentløsninger](https://nav-it.slack.com/client/T5LNAMWNA/C6W9E5GPJ)
