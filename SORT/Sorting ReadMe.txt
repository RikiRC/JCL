### PL ###

JCL Job Statement:
JOB5  JOB  CLASS=C,MSGLEVEL=(1,1)

JOB5 - nazwa joba,
JOB - określa, że ta linijka kodu będzie określać nasz JCL Job Statement,
CLASS=C - przypisuje joba do klasy inicjatora C,
MSGLEVEL=(1,1) - określa jakie dane znajdziemy w pliku wyjściowym joba

----

Exec Statement:
STEP1 EXEC PGM=SORT

STEP1 - nazwa stepu,
EXEC - określa, że ta linijka kodu określa nasz exec statement,
PGM=SORT - określa jaki program będzie uruchomiony w tym stepie. W tym przypadku będzie to program SORT

----

DD Statement:
SYSOUT DD SYSOUT=*
SORTIN DD DSN=LOGIN.DATA.SEQ,DISP=SHR
SORTOUT DD DSN=LOGIN.SORT1.SEQ,
           DISP=(NEW,CATLG,DELETE),
           SPACE=(CYL,(3,1,0)),
           DCB=(LRECL=80,BLKSIZE=800,RECFM=FB,DSORG=PS)
SYSIN DD *
 SORT FIELDS=(1,80,CH,A)
/*

SYSOUT - DD statement, który jest niezbędny do poprawnego działania programu sortującego. SYSOUT definiuje dataset w jakim pojawią się wiadomości związane z działaniem programu. Jeżeli wykorzystamy SYSOUT=* wtedy cały log SORTa pojawi się w tym samym outpucie co reszta joba.

SORTIN - DD statement, który jest niezbędny do poprawnego działania programu sortującego. Odpowiada za zdefiniowanie ścieżki datasetu wejściowego do sortowania. Parametr DISP=SHR mówi o tym, że dataset (LOGIN.DATA.SEQ) będzie mógł być wykorzystany także przez inne joby w trakcie wykonywania tego stepu (STEP1),

SORTOUT - DD statement, który jest niezbędny do poprawnego działania programu sortującego. Odpowiada za zdefiniowanie datasetu wyjściowego z posortowanymi danymi.
		
DISP - określa dyspozycję datasetu:
	(NEW,CATLG,DELETE) -NEW - oznacza to tyle, że zostanie utworzony nowy dataset w tym stepie joba,
					- CATLG - dataset zostanie zapisany z wpisem w katalogu systemowym, jeżeli ten step (STEP1) joba wykona się poprawnie,
					- DELETE - dataset zostanie usunięty jeżeli ten step (STEP1) joba wykona się niepoprawnie

SPACE - określa	ile przestrzeni zostanie wykorzystane przez dany dataset
(CYL,(3,1,0)) - CYL - oznacza wykorzystanie pamięci liczonej w cylindrach
			  -	3 - oznacza podstawową ilość cylindrów wykorzystanych do zapisu data setu,
			  - 1 - oznacza dodatkową ilość cylindrów - jest to maksimum pamięci jakie zostanie wykorzystane jeżeli zabraknie podstawowej ilość pamięci (w tym przypadku 3)
			  - 0 - oznacza ilość directory blocks
			  
DCB - Data Control Block - określa parametry fizyczne datasetu.
	LRECL - określa długość każdego rekordu w datasecie w bajtach,
	BLKSIZE - określa wielkość fizycznego bloku w bajtach w któym przechowywane będą rekordy. Zwykle BLKSIZE definiuje się jako dziesięciokrotność LRECL (w tym przypadku 80 * 10 = 800),
	RECFM - określa format datasetu. FB (Fixed Block) - oznacza taką organizację bloku, że jeden lub więcej rekordów logicznych będzie zgrupowanych w jednym bloku.
	DSORG - określa typ organizacji datasetu. Tworząc Sequential Dataset wykorzystujemy opcję PS.

SYSIN - DD statement, który jest niezbędny do poprawnego działania programu sortującego. Określa w jaki sposób i co ma być posortowane z datasetu wejściowego.
	1 - określa w której kolumnie zaczyna się klucz wg, którego dane będą sortowane,
	80 - określa długość klucza,
	CH - Character - oznacza, że kluczem będą wszystkie znaki mieszczące się w kolumnach 1-80,
	A - oznacza kolejność rosnącą posortowanych kluczy (D oznacza kolejność malejącą)