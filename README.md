# **nOS (Next Operating System)**

*nOS e l'ecosistema di versioni Next nascono all'interno del progetto **nOS for you**. Sfruttando modelli di visione artificiale e calibrazione oculare basati su WebGazer.js e WebGL, Next abbatte le barriere digitali, rendendo l'interazione con le interfacce software accessibile a chiunque tramite hardware consumer.*

---

## **1. Inclusione e Accessibilità**

La ragione d'essere del progetto **nOS** è il supporto a utenti con disabilità motorie gravi (es. paraplegia, tetraplegia, patologie neuromuscolari). I sistemi operativi tradizionali dipendono da input fisici (touchscreen, mouse, tastiere) o da puntatori oculari hardware dedicati ad alto costo.

nOS supera queste barriere integrando il tracciamento oculare direttamente nel browser via webcam:
* **Nessun hardware dedicato:** Utilizza la fotocamera frontale standard di smartphone, tablet o PC.
* **Controllo a sguardo (Gaze Tracking):** Algoritmi di rilevamento facciale e tracciamento pupillare convertono la direzione dello sguardo in coordinate cartesiane ($X, Y$) a schermo.
* **Integrazione nativa:** Interfaccia utente progettata con target di selezione ampliati per facilitare il puntamento oculare e ridurre l'affaticamento visivo.

---

## **2. Architettura Tecnica e Specifiche**

Niente strati software superflui: l'architettura è interamente **Web-First**, sviluppata per garantire la massima efficienza hardware sui client.

### **Stack Tecnologico Core**
* **Frontend Runtime:** HTML5, CSS3 (Modern Flexbox/Grid Layout), JavaScript Vanilla (ES6+).
* **Eye-Tracking Engine:** `WebGazer.js` (Libreria di Gaze Tracking client-side basata su regressione ridge).
* **Graphics & Rendering:** WebGL2 / CSS3 GPU Acceleration per il rendering dell'interfaccia a 60 FPS.
* **Distribution Model:** Progressive Web App (PWA) con Service Worker per il caching e la fruibilità offline.

---

## **3. Dettagli Tecnologici dell'Eye-Tracking & Rendering**

### **Pipeline di Tracciamento Oculare (WebGazer.js)**
1. **Video Capture:** Acquisizione del flusso video locale tramite API `navigator.mediaDevices.getUserMedia`.
2. **Face & Feature Detection:** Riconoscimento del volto e isolamento della ROI (Region of Interest) relativa agli occhi tramite la matrice di pixel dell'elemento `<canvas>`.
3. **Pupil Tracking:** Individuazione del centro della pupilla mediante rilevamento dei contorni a contrasto di luminanza.
4. **Regression Model (Ridge Regression):** Mappatura dinamica tra le coordinate del centro pupillare ($x_{eye}, y_{eye}$) e le coordinate corrispondenti sullo schermo ($X_{screen}, Y_{screen}$).
5. **Prediction & Prediction Point:** Generazione di un punto di sovrapposizione DOM in tempo reale con interpolazione delle coordinate per smussare il jitter del tracciamento.

### **Loop di Rendering e Misurazione Prestazioni**
* **FPS Counter & Loop:** Utilizzo di `requestAnimationFrame()` per sincronizzare i calcoli e le metriche di rendering con la frequenza di aggiornamento dello schermo.
* **Metriche Simulated Hardware:** In assenza di API dirette del browser per accedere alla temperatura o alla frequenza della GPU, il calcolo del carico simulato (CPU/GPU) viene derivato analiticamente dal delta temporale tra i frame (`performance.now()`) e dallo stato di attivazione dei thread di tracciamento visivo.

---

## **4. Struttura dell'Interfaccia e UX**

L'interfaccia di **nOS Nvidia** è strutturata su layout fisso in viewport (`100vh`/`100vw`) privo di scroll del body per prevenire l'offset delle coordinate di tracciamento oculare:

1. **Status Bar:** Monitoraggio in tempo reale del clock di sistema e del framerate (FPS).
2. **Home Screen Grid:** Grid flessibile contenente i punti d'accesso alle finestre principali:
   * **Next Apps:** Launcher per la suite di applicazioni Web (Stay, tUday, Notify, hmCanvas, Pagafà, pignIA).
   * **Accessibilità:** Pannello per l'inizializzazione del tracciamento, reset della calibrazione e pulizia dei dati di regressione.
   * **Impostazioni:** Dettagli su versione software, architettura e stato del rendering.
3. **Super Dock:** Dock inferiore ad accesso rapido organizzato per scorciatoie alle funzioni di sistema del dispositivo host (Protocolli `tel:`, `sms:`, `mailto:` e collegamenti Web).
4. **Window System:** Sistema di modali gestite a livello CSS con transizioni trasformate sull'asse Y (`translateY`) tramite curve di Bezier cubiche per minimizzare i costi di *reflow* e *repaint* nel browser.

---

## **5. Installazione ed Esecuzione**

Trattandosi di una Progressive Web App (PWA), nOS non richiede pacchetti binari o compilazione:

1. Aprire un browser moderno con supporto per WebRTC e WebGL (Chrome, Safari, Edge, Firefox).
2. Navigare alla directory del progetto o all'URL di hosting.
3. Concedere i permessi per l'uso della fotocamera quando viene attivato il tracciamento oculare.

---

© 2026 **NEXT** by Tobia Roncoroni. Tutti i diritti riservati.