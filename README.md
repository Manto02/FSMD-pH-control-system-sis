# Progetto SIS - Sistema di Controllo pH (Architettura degli Elaboratori)

Questo repository contiene l'elaborato sviluppato per il corso di Architettura degli Elaboratori (A.A. 2021/2022) presso l'Universita di Verona. Il progetto consiste nella progettazione e implementazione in linguaggio BLIF di un circuito sequenziale (FSMD) per il controllo di un macchinario chimico automatizzato.

## Autori
* Matteo Mantovani (VR474454)
* Matteo Gasparato (VR478799)

---

## Descrizione del Progetto
Il sistema ha lo scopo di ricevere in ingresso una soluzione a pH noto (espresso in formato fixed-point a 8 bit: 4 bit per la parte intera e 4 per quella decimale, compreso tra 0 e 14) e di riportarla a una soglia di neutralita (compresa tra 7 e 8) azionando due valvole di erogazione:
* Soluzione Acida: Abbassa il pH di 0.5 ogni ciclo di clock.
* Soluzione Basica: Alza il pH di 0.25 ogni ciclo di clock.

Il circuito valuta inoltre la validita del sensore (segnalando un errore se il pH inserito e superiore a 14) e traccia il numero di cicli di clock necessari al completamento dell'operazione.

---

## Architettura del Circuito (FSMD)
Il sistema e suddiviso in due componenti principali che comunicano tramite appositi segnali di controllo:

1. FSM (Macchina a Stati Finiti): Controllore di tipo Mealy responsabile della gestione degli stati (RESET, ACD, BAS, END, ERRORE), della validazione del pH in ingresso e del pilotaggio delle valvole.
2. Datapath: Gestisce le elaborazioni numeriche a 8 bit, inclusi i registri, i sommatori per l'incremento/decremento del pH, i comparatori e il contatore dei cicli di clock (NCLK).

---

## Struttura del Repository
* FSMD.blif: File principale (ottimizzato e pre-mapping) che unisce la FSM e il Datapath.
* fsm.blif / datapath.blif: Componenti logici del circuito.
* libreria_comp.blif / libreria_8bit.blif: Librerie di supporto contenenti porte logiche elementari e componenti a 8 bit (sommatori, multiplexer, registri, demux).
* non_ottimizzato/: Sottocartella contenente i codici sorgente originali prima delle fasi di ottimizzazione.
* Relazione.pdf: Relazione tecnica dettagliata del progetto.

---

## Istruzioni per l'uso e la simulazione
Per eseguire la simulazione del circuito tramite lo strumento SIS (Sequential Interactive Synthesis):

1. Avviare SIS da terminale.
2. Caricare il circuito principale digitando:
   ```text
   read_blif FSMD.blif
   ```
3. Effettuare la computazione e verificare la correttezza delle transizioni e dei segnali di output (FINE_OPERAZIONE, ERRORE_SENSORE, VALVOLA_ACIDO, VALVOLA_BASICO, PH_FINALE, NCLK).
