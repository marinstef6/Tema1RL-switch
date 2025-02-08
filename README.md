1 2 3
Marin Stefania, 334CC

Am folosit variabile globale pentru a le folosi in mai multe functii.
own_bid = bid-ul nostru
root_path_cost = costul pana la root
root_port = ne arata drumul cel mai scurt pana la root
root_bid = bid pentru root

Interfaces este initializat cu 0, iar pe parcurs va reprezenta interfetele
disponibile.
Vlans este o lista goala unde vom stoca informatii despre VLAN-uri pentru 
fiecare interfata.
States este o lista goala care va stoca starea fiecarui port BLOCKING/LISTENING
MAC_Table este un dictionar gol care va fi folosit pentru a mapa adresele MAC
la interfetele corespunzatoare.

    Functia send_bdpu_every_sec trimite periodic pachete BPDU pe interfetele care
sunt configurate ca trunk. Comparam daca own_bid este root_bid, iar daca nu este
asteptam o secunda si continua bucla. Parcurgem interfetele in caz afirmativ si
daca pachetul este de tip trunk, cream si trimitem pachete BDPU.

    Functia bpdu_create va defini adresa MAC, headerul LLC, headerul pentru BPDU
un byte de steaguri si opt octeti pentru padding-ul BPDU. Construieste si
returneaza pachetul BPDU complet.

    Functia bpdu_received verifica daca ID-ul root este mai mic decat cel 
actual si actualizeaza ID-ul si costul acolo unde e cazul. Setam portul ca 
LISTENING si trimitem un pachet.

    Functia parse_file citeste fisierul din config si adaugam in lista VLAN
informatii. Vom returna lista de vlan-uri si proprietatile.

    Functia destination_unfound_yet este folosita atunci cand nu stim destinatia
pachetului. Vom verifica tipul interfetei de receptie acces/trunk si aplicam 
logica corespunzatoare pentru a trimite pachetul.

    Functia main initializeaza switch-ul si gestioneaza trimiterea si receptionarea
pachetelor. Trimiterea lor va fi facuta pe mai multe cazuri:
- daca adresa MAC de destinatie este in tabela MAC atunci verificam starea
- daca sursa este trunk trimitem pe trunk sau acces
- daca sursa este acces trimitem catre destinatia trunk
- daca adresa MAC de destinatie nu este in tabela MAC si nu e broadcast 
vom trimite pachetul pe toate porturile relevante. 
