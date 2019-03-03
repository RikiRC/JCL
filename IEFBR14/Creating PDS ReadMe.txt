### PL ###

JCL Job Statement:
JOB1  JOB  CLASS=C,MSGLEVEL=(1,1)

JOB1 - nazwa joba,
JOB - określa, że ta linijka kodu będzie określać nasz JCL Job Statement,
CLASS=C - przypisuje joba do klasy inicjatora C,
MSGLEVEL=(1,1) - określa jakie dane znajdziemy w pliku wyjściowym joba

----

Exec Statement:
STEP1 EXEC PGM=IEFBR14

STEP1 - nazwa stepu,
EXEC - określa, że ta linijka kodu określa nasz exec statement,
PGM=IEFBR14 - określa jaki program będzie uruchomiony w tym stepie. W tym przypadku będzie to IEFBR14

----

DD Statement:
ALLDD DD DSN=LOGIN.NEW.PDS,
           DISP=(NEW,CATLG,DELETE),
           SPACE=(CYL,(3,1,2)),
           DCB=(LRECL=80,BLKSIZE=800,RECFM=FB,DSORG=PO)

ALLDD - nazwa DD statementu,
DD - określa, że definiowany jest DD Statement,
DSN - określa to jaki dataset wykorzystamy (w tym przypadku LOGIN.NEW.PDS),
DISP - określa dyspozycję datasetu:
	(NEW,CATLG,DELETE) -NEW - oznacza to tyle, że zostanie utworzony nowy dataset w tym stepie joba,
					- CATLG - dataset zostanie zapisany z wpisem w katalogu systemowym, jeżeli ten step (STEP1) joba wykona się poprawnie,
					- DELETE - dataset zostanie usunięty jeżeli ten step (STEP1) joba wykona się niepoprawnie

SPACE - określa	ile przestrzeni zostanie wykorzystane przez dany dataset
(CYL,(3,1,2)) - CYL - oznacza wykorzystanie pamięci liczonej w cylindrach
			  -	4 - oznacza podstawową ilość cylindrów wykorzystanych do zapisu data setu,
			  - 1 - oznacza dodatkową ilość cylindrów - jest to maksimum pamięci jakie zostanie wykorzystane jeżeli zabraknie podstawowej ilość pamięci (w tym przypadku 3)
			  - 2 - oznacza ilość directory blocks
			  
DCB - Data Control Block - określa parametry fizyczne datasetu.
	LRECL - określa długość każdego rekordu w datasecie w bajtach,
	BLKSIZE - określa wielkość fizycznego bloku w bajtach w któym przechowywane będą rekordy. Zwykle BLKSIZE definiuje się jako dziesięciokrotność LRECL (w tym przypadku 80 * 10 = 800),
	RECFM - określa format datasetu. FB (Fixed Block) - oznacza taką organizację bloku, że jeden lub więcej rekordów logicznych będzie zgrupowanych w jednym bloku.
	DSORG - określa typ organizacji datasetu. Tworząc Partitioned Dataset wykorzystujemy opcję PO.