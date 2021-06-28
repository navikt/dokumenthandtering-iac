# Doknotifikasjon

# Funksjonelle Krav
Denne applikasjonen automatisere arbeidet for å oppdatere eller opprette nye kafka topics til Aiven. For å slette topics så må det gjøres manuelt med å følge denne linken: [nais-aiven](https://doc.nais.io/addons/kafka/#delete-schema) 

Denne applikasjonen er basert på [dagpenger-iac](https://github.com/navikt/dagpenger-iac)

# Begrensninger
tjenesten har ikke noe forhold på innholdt til feltet epostTekst og smsTekst.

# Utviklingsmiljø
## Forutsetninger
* Tilgang for å deploye til teamdokumenthandtering på github

# Opprettelse av topics

1. Lag en katalog med ønsket topic navn under [kafka-aiven](kafka-aiven)
2. Legg til filene ```dev-vars.yaml, prod-vars.yaml og topic.yaml``` for henholdsvis template variabler til dev, prod og selve topic definisjonen.
3. Sjekk GHA action kjører OK.
4. Sjekk at topics er ressursen er opprettet og klar i dev/prod-gcp klustrene

```
kubectl  get topic

NAME                 AGE   STATE             FULLY QUALIFIED NAME               CREDENTIALS EXPIRY TIME
arena.oppgave.v1     18m   RolloutComplete   teamdagpenger.arena.oppgave.v1
arena.vedtak.v1      9d    RolloutComplete   teamdagpenger.arena.vedtak.v1
inntektbrukt.v1      45d   RolloutComplete   teamdagpenger.inntektbrukt.v1      2021-05-14T02:29:18Z
journalforing.v1     51d   RolloutComplete   teamdagpenger.journalforing.v1
mottak.v1            39d   RolloutComplete   teamdagpenger.mottak.v1            2021-05-14T03:31:38Z
rapid.v1             56d   RolloutComplete   teamdagpenger.rapid.v1
regel.v1             49d   RolloutComplete   teamdagpenger.regel.v1             2021-05-17T03:03:23Z
soknadsdata.v1       51d   RolloutComplete   teamdagpenger.soknadsdata.v1       2021-05-16T05:57:28Z
subsumsjonbrukt.v1   46d   RolloutComplete   teamdagpenger.subsumsjonbrukt.v1   2021-05-13T04:25:50Z
```

### Kubectl
For dev-fss: --endre denne til gcp
```shell script
kubectl config use-context dev-fss --endre denne til gcp
kubectl get topic
kubectl describe <topic>
```

For prod-fss: --endre denne til gcp
```shell script
kubectl config use-context prod-fss --endre denne til gcp
kubectl  get topic | grep doknotifikasjon
kubectl describe <topic>
```

## Henvendelser
Spørsmål koden eller prosjekttet kan rettes til Team Dokumentløsninger på:
* [\#Team Dokumentløsninger](https://nav-it.slack.com/client/T5LNAMWNA/C6W9E5GPJ)
