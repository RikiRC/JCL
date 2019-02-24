### PL ###

JCL Job Statement:
JOB4  JOB  CLASS=C,MSGLEVEL=(1,1)

JOB4 - nazwa joba,
JOB - określa, że ta linijka kodu będzie określać nasz JCL Job Statement,
CLASS=C - przypisuje joba do klasy inicjatora C,
MSGLEVEL=(1,1) - określa jakie dane znajdziemy w pliku wyjściowym joba

----

Exec Statement:
STEP1 EXEC PGM=IEBGENER

STEP1 - nazwa stepu,
EXEC - określa, że ta linijka kodu określa nasz exec statement,
PGM=IEBGENER - określa jaki program będzie uruchomiony w tym stepie. W tym przypadku będzie to IEBGENER

----

DD Statement:
SYSUT1 DD DSN=LOGIN.OLD.SEQ,DISP=SHR
SYSUT2 DD DSN=LOGIN.COPY.SEQ,DISP=(,CATLG,DELETE),
          SPACE=(CYL,(4,1,0))

SYSUT1/2 - nazwa DD statementu - SYSUT1 odpowiada za dataset wejściowy natomiast SYSUT2 odpowiada za dataset wyjściowy,
DD - określa, że definiowany jest DD Statement,
DSN - określa to jaki dataset wykorzystamy (w tym przypadku LOGIN.OLD.SEQ oraz LOGIN.COPY.SEQ),
DISP - określa dyspozycję datasetu:
	SHR - oznacza, że dostęp do tego datasetu (LOGIN.OLD.SEQ) będzie miał każdy inny zasób w trakcie trwania tego stepu (STEP1).
	(,CATLG,DELETE) - brak pierwszego parametru jest równoznaczne z wykorzystaniem parametru NEW - oznacza to tyle, że zostanie utworzony nowy dataset w tym stepie joba,
					- CATLG - dataset zostanie zapisany z wpisem w katalogu systemowym, jeżeli ten step (STEP1) joba wykona się poprawnie,
					- DELETE - dataset zostanie usunięty, jeżeli ten step (STEP1) joba wykona się niepoprawnie

SPACE - określa	ile przestrzeni zostanie wykorzystane przez dany dataset
(CYL,(4,1,0)) - CYL - oznacza wykorzystanie pamięci liczonej w cylindrach
			  -	4 - oznacza podstawową ilość cylindrów wykorzystanych do zapisu data setu,
			  - 1 - oznacza dodatkową ilość cylindrów - jest to maksimum pamięci jakie zostanie wykorzystane jeżeli zabraknie podstawowej ilość pamięci (w tym przypadku 4)
			  - 0 - oznacza ilość directory blocks - jeżeli kopiujemy SEQ wystarczy nam 0, w przypadku kopiowania PDS liczbę tę musimy zwiększyć.
			  

!!!Jeżeli job zaliczy abend z SB37 wystarczy spróbować zwiększyć ilość pamięci (w sekcji SPACE) i wykonać restart!!!