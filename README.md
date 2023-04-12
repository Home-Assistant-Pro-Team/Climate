# Homeassistant climate

Questo progetto è utilizzabile in tre modalità diverse.
- Card base per telecomando
  > Permette di simulare il telecomando per un'entità climate creata con smart ir (valido per tutte le lingue)
- Card avanzata con pkg per gestione automazioni e statistiche
  > Pacchetto completo per gestire il condizionatore integrato con smart ir in modo automatico con una card più completa rispetto alla precedente (testo in italiano)
- Blueprint per gestione automazione
  > Progetto di facile configurazione adatto per tutte le entità climate per la gestione automatica del condizionatore (traduzioni in Inglese ed Italiano)
 ## Indice
- [Card base per telecomando](#card-base-per-telecomando)
  - [Requisiti](#requisiti-card-base)
  - [Funzioni card](#funzioni-card)
  - [Caricamento](#caricamento-card-base)
- [Card avanzata con pkg per gestione automazioni e statistiche](#card-avanzata-con-pkg-per-gestione-automazioni-e-statistiche)
  - [Requisiti](#requisiti-pkg)
  - [Funzionamento pkg](#funzionamento-pkg)
  - [Funzioni card ](#funzioni-card-pkg)
  - [Caricamento](#caricamento-card-e-pkg)
- [Blueprint per gestione automazione](#blueperint-per-gestione-automazione)
  - [Requisiti](#requisiti-blueprint)
  - [Funzioni blueprint](#funzioni-blueprint)

## Card base per telecomando: 
 <img src="https://user-images.githubusercontent.com/62516592/230682774-197c62dc-205e-47e4-8195-d32e354635cc.jpg" width="200">
 
### Requisiti card base:
- [custom button card](https://github.com/custom-cards/button-card)
- [integrazione smart ir](https://github.com/smartHomeHub/SmartIR)
### Funzioni card:
Questa card permette di simulare il telecomando per entità climate e non è vincolato all'utilizzo di altri file.
La card è realizzata con immagine svg e custom button-card

  - **Display standard:** sul display è possibile visualizzare lo stato del condizionatore (spento o acceso), temperatura, modalità hvca, velocità di ventilazione impostata e la temperatura ed umidità interna.
 <img src="https://user-images.githubusercontent.com/62516592/230679944-f0c45d7c-95cd-42da-9c99-498a5de8c6fe.jpg" width="200" >
  
  - **1:** con un tap il condizionatore si accende o si spegne
  - **2:** con un tap è possibile cambiare la ventilazione
  - **3:** con un tap è possibile cambiare la modalità hvca (dry, cool, auto...)
  - **4:** con un tap è possibile aumentare la temperatura impostata
  - **5:** con un tap è possibile personalizzare la action. Di default apre more-info del condizionatore
  - **6:** con un tap è possibile personalizzare la action. Di default apre more-info del condizionatore
  - **7:** con un tap è possibile diminuire la temperatura impostata
 
### Caricamento card base:
Per eseguire la card basta copiare il file all'interno di una nuova card manuale e sostituire la variabile climate con la propria entità 

``` 
type: custom:button-card
variables:
  climate: climate.condizionatore_salone
```

# Card avanzata con pkg per gestione automazioni e statistiche
https://user-images.githubusercontent.com/62516592/230683695-5766ce0c-f760-4e4b-99d1-73056ebbfd53.mp4
### Requisiti pkg: 
- [custom button card](https://github.com/custom-cards/button-card) con template abilitato
- [integrazione smart ir](https://github.com/smartHomeHub/SmartIR)
- dashboard in modalità yaml, ben descritta da [MaxAlbani](https://www.maxalbani.it/2023/04/home-assistant-dashboards-lo-strumento-per-creare-interfacce-grafiche/#modalita)
- sensore finestra (non indispensabile)
- sensore allagamento (non indispensabile)
- sensore assorbimento in w (non indispensabile)
### Funzionamento pkg:
Questo utilizzo è sicuramente il più complesso ma anche il più completo, perchè prevede il funzionamento del condizionatore in modalità automatica tenendo in considerazione diversi fattori:
- **Modalità o periodo utilizzo:** È possibile scegliere 4 modalità di funzionamento:
  - [Estate Indice di thom:](https://indomus.it/progetti/definire-un-indicatore-di-benessere-estivo-sulla-domotica-home-assistant/) Il condizionatore si accenderà o spegnerà se l'indice di thom rilevato è maggiore a quello impostato
  - Estate Gradi Celsius: Il condizionatore si accenderà o spegnerà se la temperatura e l'umidità rilevata è maggiore a quella impostata
  - Inverno: Il condizionatore si accenderà o spegnerà se la temperatura rilevata è inferiore a quella impostata
  - Umidità: Il condizionatore si accenderà o spegnerà se l'umidità rilevata è maggiore a quella impostata
- **Temperatura interna rilevata:** in base alla modalità selezionata è possibile impostare una temperatura/umidità/thom rilevata per gestire l'accensione o lo spegnimento del condizionatore in modalita automatica
- **Velocità ventilazione:** è possibile impostare la velocità di ventilazione del condizionatore da utilizzare con l'accensione automatica
- **Modalità hvca:** è possibile impostare la modalità hvca (dry,cool,auto...) del condizionatore da utilizzare con l'accensione automatica
- **Temperatura condizionatore:** e possibile impostare la temperatura del condizionatore da utilizzare con l'accensione automatica
- **Fascia oraria:** è possibile scegliere una fascia oraria per l'accensioni o lo spegnimento automatico
- **Presenza in casa:** le automazioni funzioneranno solo se lo stato del gruppo o della singola entità person si trovano nello stato home. Se si passa allo stato  not_home il condizionatore verrà spento.
```
# Esempio di un gruppo famiglia
group:
  famiglia:
    name: Famiglia
    entities:
      - person.marco
      - person.serena
```
- **Notifiche:** si può decidere se abilitare o disabilitare le notifiche. Il pkg è impostato per riceverle su tutti i device con app companion installata. Nel caso si volessero utilizzare device diversi o media_player occorre modificarlo.
- **Stato Finestre:** viene eseguito un controllo sullo stato finestre:
    - Se il condizionatore è acceso e la finestra verrà aperta riceverai una notifica per chiudere la stessa, se questo non avverrà entro 30 secondi il condizionatore verrà spento.
    - Se il condiziontore è spento e viene acceso manualmente con la finestra aperta si riceverà una notifica di avviso
    - Se l'accensione automatica è abilitata e ci sono i requisiti per accendere il condizionatore ma la finestra è aperta si riceverà una notifica
- **Temperatura esterna:** Viene eseguita in due modalità
   - Rispettando una sua fascia oraria: è possibile impostare una differenza di temperatura rilevata tra interna ed esterna che consiglia di aprire o chiudere la finestra se il controllo finestra è attivo ne verifica anche lo stato (es. quando rileva la temperatura esterna maggiore di 5° rispetto a quella interna)
   - Legata allo stato del condizionatore:
      - Nel momento in cui il condizionatore si deve accendere in automatico ma la temperatura esterna è maggiore/minore (in base alla modalità impostata) di quella target, non avviene l'accensione del condizionatore ma si riceverà un notifica per aprire la finestra.
      - Se il condizionatore è acceso ma la temperatura esterna è maggiore/minore (in base alla modalità impostata) di quella target, si riceverà una notifica per aprire o chiudere la finestra e spegnere il condizionatore.
- **Livello acqua:** utilizzo un sensore allagamento aqara per controllare lo stato del serbatoio dove scarica l'acqua il condizionatore.
  - se il condizionatore è acceso ed il serbatoio è pieno ricevi una notifica per svuotarlo
  - se il condizionatore è acceso ed il serbatoio è pieno da 5 minuti il condizionatore si spegnerà con notifica
  - se il serbatoio è pieno e verrà acceso il condizionatore, riceverai una notifica per svuotarlo
  - se accendi il condizionatore ed il serbatoio è pieno ma non verrà svuotato entro 5 minuti si spegnerà con notifica.
- **Statistiche utilizzo:** Utilizzando un dispositivo per rilevare la potenza assorbita, nel mio caso shelly-em puoi vedere visualizzato, il tempo di accensione, il costo ed il consumo del condizionatore senza utilizzo del recorder ed avere la possibilità resettare i dati in qualsiasi momento.
### Funzioni card pkg:
 <img src="https://user-images.githubusercontent.com/62516592/230679628-4aa84cd4-3cbd-45fe-87e7-e33b6362e0dd.jpg" width="200" >
 
  - **Display standard:**  sul display è possibile visualizzare lo stato del condizionatore (spento o acceso), temperatura, modalità hvca, velocità di ventilazione impostata e la temperatura ed umidità interna e se attivo, l'accensione e lo spegnimento automatico.
 <img src="https://user-images.githubusercontent.com/62516592/230679944-f0c45d7c-95cd-42da-9c99-498a5de8c6fe.jpg" width="200">
 
  - **1:** con un tap il condizionatore si accende o si spegne
  - **2:** con un tap è possibile cambiare la ventilazione
  - **3:** con un tap è possibile cambiare la modalità hvca (dry, cool, auto...)
  - **4:** con un tap è possibile aumentare la temperatura impostata
  - **5:** con un tap è possibile visualizzare sul display le 4 pagine di impostazioni, con un hold tap è possibile forzare l'uscita dal menu
  - **6:** con un tap è possibile visualizzare sul display le statistiche
  - **7:** con un tap è possibile diminuire la temperatura impostata
 <img src="https://user-images.githubusercontent.com/62516592/230681033-1e8d73eb-9df6-4b3a-8e85-5f1b748f2185.jpg" width="200">
 
  - **Display statistiche**: questa schermata è solo informativa e non è possibile interagire
  
<img src="https://user-images.githubusercontent.com/62516592/230681298-782bfe27-80ac-401c-a02b-647845cdeadc.jpg" width="200" > <img src="https://user-images.githubusercontent.com/62516592/230681321-41e9f25a-0914-4d77-8749-6c3d2827ca2a.jpg" width="200" >
 <img src="https://user-images.githubusercontent.com/62516592/230681339-1a0ca9c5-5e31-4327-8e60-494acac2bfe9.jpg" width="200" >
 <img src="https://user-images.githubusercontent.com/62516592/230681353-a568a529-7292-49a4-bc04-7b0ac99d903e.jpg" width="200" >
 
  - **Display impostazioni**: ogni pagina è composta da 5 sezioni, molte delle quali da due righe, per cambiare le impostazioni basta eseguire un tap per modificare i valori che si trovano sulla prima riga mentre un hold tap per selezionare la seconda riga dove presente
### Caricamento card e pkg
- **Caricamento pkg:**
  - Caricare il contenuto della cartella packages appena scaricata nella cartella packages presente nella propria istanza
  - Aprire ogni singolo file e sostituire le entità presenti negli anchors
```
# esempio
homeassistant:
  customize:
    package.node_anchors:
        Entità clima:                               &climate        climate.condizionatore_salone
```
  - Modificare gli array evidenziati con le proprie entità
```
# esempio
{% set climate = 'climate.condizionatore_salone' %}
```
  - Se si vuole utilizzare il pkg per un secondo condizionatore occorre sostistuire OVUNQUE la parola ac_salone con una nuova a piacimento
  - Di default le notifiche sono impostate per essere ricevute su tutti i device con app companion installata, se si voglioni riceve notifiche diverse es.media player occorre personalizzare i file  
```
# servizio utilizzato di default
- service: notify.notify
  data:
    title: title
    message: message
```            
- **Caricamento card:**
  - Per eseguire la card basta copiare il file all'interno della propria dashboard yaml
  - cambiare la variabile climate con la propria entità
  - cambiare (se precedentemente sostituito) la variabile name con quello personalizzato
``` 
type: custom:button-card
variables:
  climate: climate.condizionatore_salone
  name: ac_salone
```
## Blueperint per gestione automazione:
### Requisiti Blueprint

A differenza di quanto trattato sopra questo progetto è compatibile con tutte le entità climate 
- Entità climate configurata
Facoltativo:
- Sensore finestra
- Sensore allagamento
### Funzioni blueprint:
https://github.com/marco-hacs/Blueprint-Automatic-air-conditioner

Questo progetto prevede l'utilizzo automatico del climatizzatore sia in inverno che in estate, in base a una temperatura iniziale e finale. In modo facoltativo, si può abilitare:

- Controllo dello stato della finestra
- Controllo del livello dell'acqua nel serbatoio
- Controllo della presenza domestica
- Notifiche (inglese e italiano)
- Decidere la fascia oraria per il funzionamento

Di seguito le impostazioni per il funzionamento sono:

1) **Seleziona lingua**: Scegli la lingua per le notifiche (default: italiano).
2) **Entità clima**: Scegli l'entità clima da utilizzare.
3) **Seleziona stagione**: Scegli la stagione di utilizzo (predefinito: Estate)
4) **Imposta temperatura clima**: Seleziona la temperatura da impostare al clima
5) **Modalità Hvac**: selezionare la modalità di utilizzo Hvac (riscaldamento, raffreddamento, deumidificazione, solo ventilazione)
6) **Modalità ventola**: selezionare la modalità di utilizzo della ventola (automatica, alta, media, bassa). Nel caso in cui le tue impostazioni climatiche siano diverse dalle mie, queste possono essere personalizzate dal file sorgente.
7) **Presenza Home**: FACOLTATIVO Selezionare dall'elenco il gruppo creato con entità persona per:
- Accendere il climatizzatore se sono soddisfatte le condizioni impostate per l'accensione
- Spegnere il condizionatore d'aria nel momento in cui si passa allo stato fuori casa
```
group:
  famiglia:
    entities:
      - person.marco
      - person.serena
```
8) **Livello dell'acqua**. FACOLTATIVO: selezionare il binario\_sensore utilizzato per indicare il serbatoio dell'acqua pieno. Per funzionare, deve essere impostato con *dispositivo\_classe: umidità*.
9) **Finestra**: FACOLTATIVO selezionare il sensore binario utilizzato per il contatto finestra. Per funzionare deve essere impostato con *device\_class:window*
10) **Temperatura target di avvio**: Impostare la temperatura di avvio:
- Se impostato su inverno, il clima verrà attivato se la temperatura interna è inferiore alla temperatura impostata
- Se impostato su estate, il clima verrà attivato se la temperatura interna è superiore alla temperatura impostata
11) **Arresto temperatura target**: impostare la temperatura di spegnimento:
- Se impostato su inverno, il clima verrà disattivato se la temperatura interna è superiore alla temperatura impostata
- Se impostato su estate, il clima verrà disattivato se la temperatura interna è inferiore alla temperatura impostata
12) **Ritardo arresto temperatura**: Imposta un ritardo espresso in minuti per lo spegnimento del climatizzatore una volta raggiunto “*Arresto temperatura target*”
13) **Ora di inizio**: Impostare l'ora di inizio del funzionamento automatico del climatizzatore. NB: Se si desidera che il clima sia automatico h24, impostare Orario di inizio e Orario di fine con l'orario 00:00:00.
14) **Stop time**: imposta l'ora di fine e di spegnimento del funzionamento automatico della climatizzazione. NB: Se si desidera che il clima sia automatico h24, impostare Orario di inizio e Orario di fine con l'orario 00:00:00.
15) **Dispositivo per notifica push**: FACOLTATIVO selezionare il dispositivo su cui si desidera ricevere la notifica push. Sul dispositivo deve essere installata l'app ufficiale HomeAssistant.

Questo progetto è stato realizzato rispettando le mie esigenze personali e le entità climatiche utilizzate con Broadlink.

Rimango aperto al feedback e a qualsiasi idea per rendere questo progetto più utilizzabile per tutti.

[![Open your Home Assistant instance and show the blueprint import dialog with a specific blueprint pre-filled.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fgithub.com%2Fmarco-hacs%2FAutomatic-air-conditioner%2Fblob%2Fmain%2Fautomatic_air_conditioner.yaml)
https://community.home-assistant.io/t/automatic-air-conditioner/511251
