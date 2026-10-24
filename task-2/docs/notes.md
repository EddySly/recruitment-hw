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

            SCHEMATICHE KICAD:
            -)Creati 2 partitori per le soglie dei comparatori con resistenze nell ordine 10^3 per diminuire calore. (V_refBrake, V_refCurrent)
            -)Aggiunti 2 comparatori TI LM393 per sensori e soglie
            -)aggiunto alimentatore per i comparatori (va bene ta tensione instabile del supply.)

