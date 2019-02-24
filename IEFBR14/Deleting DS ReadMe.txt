### PL ###

JCL Job Statement:
JOB3  JOB  CLASS=C,MSGLEVEL=(1,1)

JOB3 - nazwa joba,
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
DELDD DD  DSN=LOGIN.OLD.SEQ,DISP=(OLD,DELETE)

DELDD - nazwa DD statementu,
DD - określa, że definiowany jest DD Statement,
DSN - określa jaki dataset wykorzystamy (w tym przypadku LOGIN.OLD.SEQ),
DISP - określa dyspozycję datasetu:
	(OLD,DELETE) 	- OLD - oznacza, że STEP1 będzie wykorzystywał już wcześniej utworzony dataset w systemie. Ponadto dostęp do datsetu (LOGIN.OLD.SEQ) będzie eksluzywny dla tego joba (JOB3) w trakcie wykonywania tego stepu (STEP1), innymi słowy dopóki ten step (STEP1) się nie ukończy to żaden inny job nie będzie miał dostęu do wykorzystywanego tutaj datasetu (LOGIN.OLD.SEQ),
					- DELETE - dataset zostanie usunięty, jeżeli ten step (STEP1) joba wykona się poprawnie