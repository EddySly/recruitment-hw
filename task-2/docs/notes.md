# Note di progetto

1) Specs del BSPP
    -) L'alimentazione e' instabile-> potrebbe essere okay per il relay se ne trovo uno che mi copre la banda
-> per i sensori del freno e corrente devo trovare una soluzione stabile (magari un regolatore 78xx)

    -) il sensore del freno e' una retta di equazione f1(x) = 0.5 + 0.04x (in volt), trovo allora il votaggio del
sensore per 30 bar: f1 = 0.5 + 0.04*30 = 1.7V (immagino che se ho 1.7V o piu' di tensione per 500ms ho il primo seganle per l'and con l'altro sensore??)

    -) il sensore della corrente e' pure una retta di eq f2(x) = 2.5 + 0.02x ma conviene prima trovare una soglia di ampere dove fare scattare il sesore...
    -> P = vi => i = p/v => i(400V) = 12.5 A che a 600volt porterebbe al senosre 7500watt ergo e' troppo insensibile.
    -> p = vi con V=600 e W = 5000 => 8.33A questo significa che se scelgo questa come soglia il BSPD puo' intervenire tra 3300 w e 5000w ed è molto meglio
    della prima perche' dato che il sensore non conosce che tensione arriva ora sa che a 8.33A si interviene.
    -> questo si traduce in una differenza di potenziale nell' output del sensore di 2.67V (la quale e' molto vicina allo zero...chissa' se portera' problemi)

2) Sto cercando come comparare i due segnali:
    -) LM393 texas instruments: supply voltage (range) =0v a Vin-2V che quindi posso tranquillamente alimentare con l'instabile 9-17V (anche nel caso peggiore 9v sono dentro)
3) Per l'alimentazione ai due sensori va bene un 5V regolato e pure per i riferimenti(partitori per dare soglia ai comparatori).

            SCHEMATICHE KICAD1:
            -)Creati 2 partitori per le soglie dei comparatori con resistenze nell ordine 10^3 per diminuire calore. (V_refBrake, V_refCurrent)
            -)Aggiunti 2 comparatori TI LM393 per sensori e soglie
            -)aggiunto alimentatore per i comparatori (va bene ta tensione instabile del supply.)
            SCHEMATICHE KICAD2:
            -)Aggiunte le resistenze e le tensioni di pull up, resistenza scelta 10k ohm, sono dentro i limiti di corrente del comparatore(0.001ma- 4ma)
            -)Corrette le resistenze per la soglia della corrente (r3 ora 13k r4 ora 15k)

4) Per quanto riguarda la possibilità che i sensori vadano a quel paese:
ho pensato di mettere un pullup già su BREAKE_SENS e CURRENT_SENS, poi comparare L'out del comparatore e la tensione sul break sens o altro e agire di conseguenza ma
se il cavo del sensore al posto di staccarsi e basta va a massa allora la tensione va a 0 e non riesco a fare nulla... (idea scartata)

Mettere 4 altri comparatori e' piu' veloce e copre ogni caso di errore: sia sotto i 0.5v che sopra i 4.5v, utilizzo resistenze da 100k ohm in ogni caso per non interferire con 
i sensori e per avere un potenziale fisso di errore (5v). (nel caso che non si è attaccato il sensore.)

Per quanto riguarda le soglie ho scelto 0.4 per la bassa e 4.6 per la alta, non esattamente a zero perchè ad esempio 0.5 per il freno significa semplicemente che 
non si sta frenando.
Per quanto riguarda i comparatori dopo molte ricerche ho deciso di mettere un piccolo condensatore all alimentazione per "addolcire" i cambiamenti di stato.
            SCHEMATICHE KICAD3:
            -) Aggiunto 2 nuovi comparatori lm393 per testare la veridicità dei sensori
            -) differenza principe dall comparatore di pressione e corrente primo : pullup sul sensore per dare 5V al comparatore in caso di sensore staccato.
            -)aggiunti i condensatori all alimentazione.
            -) creati i partitori per le soglie di fail dei sensori alto e basso.

5) se i sensori failano e' un problema perch' tipo se va a massa il sensore della frenata, il mio comparatore u1a è come se non esistesse, ergo anche se frena non lo vede,
in duale pure se il sensore è staccato quindi mi parte la pullup che mi porta il sensore a 5V e fa segnare high a u1a anche se non sto frenando, ragionamento uguale per quelli
di corrennte... se i sensori failano sto pensando di aprire il rele' senza nemmeno 500ms di ritardo.

6) prima di capire che fare con i sensori meglio se penso a come fare l'and tra High_b e High_c, dopo un po di ricerche ho notato che potrei mettere in comune i rami dei due comparatori con un pullup che si collega pure al nodo, in questo modo se ad esempio ho corrente alta ma il freno e' okay (quindi and = 0) la tensione cade tutta ai capi di rpullup
se invece ho entrambi i comparatori alti (and =1), allora sto a 5v sul nodo!
            SCHEMATICHE KIKAD4:
            -)wired and tra i due comparatori (sensori funzionanti)
            -)eliminate le resistenze di pullup r5 e r6 perchè il nodo e' e deve essere in comune.
            -) cancellate le 2 label highb e highc e fuse in CRITIC_HIGH.