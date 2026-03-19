# PROIECT-Analizare-SIP-Honeypot-Asterisk

#1.REZUMAT

## 1.1 Descrierea proiectului
   Proiectul consta in implementarea unui server de telefonie IP bazat pe platforma Asterisk, expus intentionat in internet pentru a functiona ca un Honeypot. Sistemul a fost creat pentru a atrage, inregistra si analiza tentativele de atac cibernetic Brute Force si Toll Fraud. Intr-un interval de mai putin de 24 de ore, serverul a captat peste 3.1 milioane de evenimente, oferind date despre comportamentul netbots-ilor SIP.

## 1.2 Descrierea implementarii
- Am folosit un VPS in Oracle cu sistem de operare Ubuntu 24.04
- Am instalat si configurat Asterisk 20 LTS ca serviciu de fundal sub un utilizator securizat
- Am definit endpoint-uri PJSIP cu autentificare complexa pentru a preveni logarile reale, mentinand serverul ca momeala
- Am activat modulul de securitate in logger.conf pentru a capta detaliile fiecarei tentative de acces: IP, User Agent, Extensie
- Am procesat log-urile utilizand utilitare Linux CLI ex.grep, awk, sed, sort; pentru a genera rapoarte de tip Threat Intelligence.

# 2.INTRODUCERE
## 2.1 Prezentarea temei
   Acest proiect reprezinta o solutie de monitorizare activa a amenitarilor cibernetice in relete VoIP. Sistemul foloseste PJSIP pentru a gestiona cererile bazate pe protocolul SIP. Implementarea respinge orice tentativa neautorizata, dar salveaza datele atacatorului pentru analiza.

## 2.2 Motivarea practica pentru alegera temei
   Motivul principal pentru alegerea acestei teme este combaterea fraudei in telecomunicatii. Companiile care utilizeaza centrale VoIP slab securizate pot suferi pierderi financiare masive intr-un timp foarte scurt daca atacatorii reusesc sa foloseasca resursele serverului pentru a apela numere internationale cu suprataxa. Prin acest proiect, am realizat un sistem capabil sa identifice aceste moduri de atac si sa colecteze date despre atacatori ex.IP.

## 2.3 Sisteme actuale
   In prezent, protectia sistemelor VoIP se bazeaza pe Session Border Controllers si solutii automate de tip Intrusion Prevention precum Fail2Ban. Totusi, majoritatea acestor sisteme doar blocheaza atacul fara a oferi o analiza detaliata. Proiectul meu completeaza aceste solutii, oferind o privire asupra metodelor de scanare si a dictionarelor de extensii folosite de atacatori.

# 3.PREZENTAREA PLATFORMEI HARDWARE SI SOFTWARE

## 3.1 Caracteristici tehnice - Instanta Cloud Oracle
   Server virtual configurat pentru disponibilitate inalta si expunere controlata in internet.
   - Procesor: AMD EPYC 7551 32-Core Processor
   - Arhitectura: x86_64
   - Capacitate de calcul: 2 nuclee virtuale detectate
   - Tehnologie de virtualizare: KVM
   - Memorie Cache L3: 16 MiB
   - BogoMIPS: 3992.49

## 3.2 Caracteristici tehnice - Asterisk PBX
   Platforma Open Source utilizata pentru rutarea apelurilor si gestionarea stivei de comunicare SIP.
   - Versiune: Asterisk 20.18.2 LTS
   - Driver SIP: PJSIP 
   - Status: Rulare sub utilizator dedicat "asterisk" pentru securitate
   - Log-uri: Modulul res_security_log activat

## 3.3 Caracteristici tehnice - CLI
   Utilitare folosite pentru filtrarea datelor brute.
   - grep: Folosit pentru izolarea evenimentelor de tip "SECURITY" si a metodelor SIP specifice
   - awk: Utilizat pentru extractia coloanelor ex.IP sursa
   - sed: Utilizat pentru cenzurarea parolelor
   - sort si uniq: Folosite pentru generarea topurilor atacurilor

# 4. REZULTATE SI CONCLUZII

## 4.1 Statistici 
   Analiza log-urilor a evidentiat trei tipuri principale de activitati malitioase, clasificate dupa metoda SIP utilizata:
   Metoda SIP        Numar Cereri        Tipul Amenintarii
   REGISTER          3.181.759           Brute Force
   INVITE            5.299               Toll Fraud
   OPTIONS           16                  Reconnaissance

## 4.2 Analiza atacatorilor si a tintelor
   - Top 3 atacatori: IP: -138.68.185.26 cu 2489220 cereri 
                         -117.103.80.90 cu 692820 cereri 
                         -5.135.12.120 cu 4839 cereri

   - Exstensii tinta: Hackerii au folosit dictionare numerice, cele mai atacate fiind, 888, 5, 999, 30000, 50005 si 666

## 4.3 Concluzii finale
   Sistemul a functionat conform asteptarilor, fiind o momeala eficienta si rezistand la peste 3 milioane de atacuri fara a permite o logare reusita datorita urmatoarelor masuri:
   - Lipsa identitatilor: Atacatorii au vizat extensii inexistente in configuratia locala, ducand la respingerea instantanee a cererilor
   - Validarea endpoint-urilor: Driverul PJSIP a filtrat toate cererile care nu proveneau de la clienti autorizati, generand erori de tip "No matching endpoint"
   - Respingerea pachetelor malformate: Cele 12 cereri de tip "Request Line" invalid demonstreaza capacitatea Asterisk de a ignora traficul care nu respecta sintaxa protocolului SIP.
