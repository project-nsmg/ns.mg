---
title: Speech Recognition for Lojban
layout: page
description: A Lojban speech recognition tool for our community.
links:
  - title: Progress
    url: /projects/speech-recognition-for-lojban.html
---

[sorpa'as](//sorpaas.com "//sorpaas.com") is working on a speech recognition application for Lojban. Stay tuned for more information.

* * * * *

Introduction
------------

We need a Lojban speech recognition tool for our community. In general, in order to make a speech recognition tool, we need to do at least as follows:

1.  Acoustic model
2.  Dictionary
3.  Grammar/Language model

After they are built, they can be passed together with source of speech, and a live speech recognizer or stream speech recognizer can be created. For example:

    Configuration configuration = new Configuration();

    // Set path to acoustic model.
    configuration.setAcousticModelPath("resource:/WSJ_8gau_13dCep_16k_40mel_130Hz_6800Hz");
    // Set path to dictionary.
    configuration.setDictionaryPath("resource:/WSJ_8gau_13dCep_16k_40mel_130Hz_6800Hz/dict/cmudict.0.6d");
    // Set language model.
    configuration.setLanguageModelPath("resource:/edu/cmu/sphinx/models/language/en-us.lm.dmp");

    LiveSpeechRecognizer recognizer = new LiveSpeechRecognizer(configuration);
    // Start recognition process pruning previously cached data.
    recognizer.startRecognition(true);
    SpeechResult result = recognizer.getResult();
    // Pause recognition process. It can be resumed then with startRecognition(false).
    recognizer.stopRecognition();

Engine
------

We will use sphinx4 as the engine.

Acoustic Model
--------------

We won't build a acoustic model from scratch because we don't have enough data. Instead, what we will do is to adapt an existing acoustic model. A detailed tutorial can be found at [the CMUSphix Wiki](http://cmusphinx.sourceforge.net/wiki/tutorialadapt "http://cmusphinx.sourceforge.net/wiki/tutorialadapt").

### Example Files

#### arctic20.txt

    ni'o la teris. po'u le tirxu ge'u .e lei pendo no'u la .elis. po'u le xanto ge'u joi la zo,is. po'u le xirmrxipotigre cu xabju le cicricfoi
    .i la teris. ta'e djica le nu vitke zi'o le barda tcadu noi lei vinji ga'u voikla ke'a
    .isemu'ibo ca pa donri la teris. co'a dzukla le barda tcadu
    .i la teris. za klama lo rirxe gi'e reisku fi lo finpe be di'o ry. fu le du'u dakau cu pluta le tcadu
    .i le finpe cu spuda ty. ko'a goi lu ko cadzu mo'i ne'a le rirxe fi'o seldei li ci
    .ibabo do viska ru li'u
    .i la teris. se gidva ko'a
    .ijebo mo'u le cimoi donri la teris. viska le tcadu .uicai tergu'i
    .i cnidu'e .isemu'ibo la teris. jdice le du'u cadzu ca'o le piro nicte
    .i co'a le cerni la teris. klamu'o le zarci di'o le tcadu korbi
    .i nanla
    .i lu .iicai tirxu li'u se cusku le nanla
    .i lu .iicai nanla li'u se cusku la teris.
    to le nanla ku fa'u la teris. pu noroi zgana lo tirxu ku fa'u lo nanla toi
    .i le nanla no'u la mulis. cu ganse le ka le tirxu cu pendo ku gi'e te preti le nu la teris. djica le nu te jarco tu'a le tcadu my.
    .i lu .iesai ku'i mi ba'e ca djica le nu mi sipna
    .i mi mutce tatpi li'u se cusku la teris.
    .i lu je'e
    .i mi'o zifre le nu klama le mi zdani li'u se cusku la mulis.
    .iseki'ubo le remei cu cadzu seka'a le zdani be la mulis.

#### arctic20.fileids

    arctic_0001
    arctic_0002
    arctic_0003
    arctic_0004
    arctic_0005
    arctic_0006
    arctic_0007
    arctic_0008
    arctic_0009
    arctic_0010
    arctic_0011
    arctic_0012
    arctic_0013
    arctic_0014
    arctic_0015
    arctic_0016
    arctic_0017
    arctic_0018
    arctic_0019
    arctic_0020

#### arctic20.transcription

    <s> NIHO LA TERIS POHU LE TIRXU GEHU E LEI PENDO NOHU LA ELIS POHU LE XANTO GEHU JOI LA ZO IS POHU LE XIRMRXIPOTIGRE CU XABJU LE CICRICFOI </s> (arctic_0001)
    <s> I LA TERIS TAHE DJICA LE NU VITKE ZIHO LE BARDA TCADU NOI LEI VINJI GAHU VOIKLA KEHA </s> (arctic_0002)
    <s> ISEMUHIBO CA PA DONRI LA TERIS COHA DZUKLA LE BARDA TCADU </s> (arctic_0003)
    <s> I LA TERIS ZA KLAMA LO RIRXE GIHE REISKU FI LO FINPE BE DIHO RY FU LE DUHU DAKAU CU PLUTA LE TCADU </s> (arctic_0004)
    <s> I LE FINPE CU SPUDA TY KOHA GOI LU KO CADZU MOHI NEHA LE RIRXE FIHO SELDEI LI CI </s> (arctic_0005)
    <s> IBABO DO VISKA RU LIHU </s> (arctic_0006)
    <s> I LA TERIS SE GIDVA KOHA </s> (arctic_0007)
    <s> IJEBO MOHU LE CIMOI DONRI LA TERIS VISKA LE TCADU UICAI TERGUHI </s> (arctic_0008)
    <s> I CNIDUHE .ISEMUHIBO LA TERIS JDICE LE DUHU CADZU CAHO LE PIRO NICTE </s> (arctic_0009)
    <s> I COHA LE CERNI LA TERIS KLAMUHO LE ZARCI DIHO LE TCADU KORBI </s> (arctic_0010)
    <s> I NANLA </s> (arctic_0011)
    <s> I LU IICAI TIRXU LIHU SE CUSKU LE NANLA </s> (arctic_0012)
    <s> I LU IICAI NANLA LIHU SE CUSKU LA TERIS </s> (arctic_0013)
    <s> TO LE NANLA KU FAHU LA TERIS. PU NOROI ZGANA LO TIRXU KU FAHU LO NANLA TOI </s> (arctic_0014)
    <s> I LE NANLA NOHU LA MULIS. CU GANSE LE KA LE TIRXU CU PENDO KU GIHE TE PRETI LE NU LA TERIS. DJICA LE NU TE JARCO TUHA LE TCADU MY. </s> (arctic_0015)
    <s> I LU .IESAI KUHI MI BAHE CA DJICA LE NU MI SIPNA </s> (arctic_0016)
    <s> I MI MUTCE TATPI LIHU SE CUSKU LA TERIS </s> (arctic_0017)
    <s> I LU JEHE </s> (arctic_0018)
    <s> I MIHO ZIFRE LE NU KLAMA LE MI ZDANI LIHU SE CUSKU LA MULIS </s> (arctic_0019)
    <s> ISEKIHUBO LE REMEI CU CADZU SEKAHA LE ZDANI BE LA MULIS </s> (arctic_0020)

#### arctic20.dic

This file is a little bit difficult to create. Here is an example for the related English version.

    'EM	AH M
    A	AH
    A(2)	EY
    ACROSS	AH K R AO S
    AGAIN	AH G EH N
    AGAIN(2)	AH G EY N
    ALMOST	AO L M OW S T
    ALWAYS	AO L W EY Z
    ALWAYS(2)	AO L W IY Z
    AND	AE N D
    AND(2)	AH N D
    APOLOGIZED	AH P AA L AH JH AY Z D
    ASLEEP	AH S L IY P
    AT	AE T
    AURORA	ER AO R AH
    AUTHOR	AO TH ER
    BACK	B AE K
    BALLS	B AO L Z
    BE	B IY
    BELIZE	B EH L IY Z
    BEYOND	B IH AA N D
    BEYOND(2)	B IH AO N D
    BEYOND(3)	B IY AO N D
    BLESS	B L EH S
    BOREALIS	B AO R IY AE L AH S
    BUSINESS	B IH Z N AH S
    BUSINESS(2)	B IH Z N IH S
    BUT	B AH T
    CAME	K EY M
    CASE	K EY S
    CHAIR	CH EH R
    CHANCES	CH AE N S AH Z
    CHANCES(2)	CH AE N S IH Z
    CHANGE	CH EY N JH
    CHURCHILL	CH ER CH HH IH L
    CHURCHILL(2)	CH ER CH IH L
    CITIES	S IH T IY Z
    CLUBS	K L AH B Z
    COMING	K AH M IH NG
    COMPANION	K AH M P AE N Y AH N
    DANGER	D EY N JH ER
    DEGREE	D IH G R IY
    DELICATE	D EH L AH K AH T
    DOWN	D AW N
    ETC	EH T S EH T ER AH
    EVENING	IY V N IH NG
    EVER	EH V ER
    EXCLAIMED	IH K S K L EY M D
    FACED	F EY S T
    FEET	F IY T
    FIGHTER	F AY T ER
    FOLLOWED	F AA L OW D
    FOR	F AO R
    FOR(2)	F ER
    FOR(3)	F R ER
    FOREVER	F ER EH V ER
    FORGET	F AO R G EH T
    FORGET(2)	F ER G EH T
    FORT	F AO R T
    FRIENDSHIP	F R EH N D SH IH P
    FRIENDSHIP(2)	F R EH N SH IH P
    FROM	F ER M
    FROM(2)	F R AH M
    GAD	G AE D
    GAME	G EY M
    GLAD	G L AE D
    GO	G OW
    GOD	G AA D
    GREGSON	G R EH G S AH N
    GREW	G R UW
    HAND	HH AE N D
    HANDS	HH AE N D Z
    HANDS(2)	HH AE N Z
    HATRED	HH EY T R AH D
    HE	HH IY
    HEAD	HH EH D
    HIS	HH IH Z
    HOPE	HH OW P
    I	AY
    I'LL	AY L
    I'M	AY M
    IF	IH F
    IN	IH N
    IT	IH T
    IT'S	IH T S
    JEALOUSY	JH EH L AH S IY
    JUST	JH AH S T
    JUST(2)	JH IH S T
    LETTER	L EH T ER
    LIFE	L AY F
    LIKE	L AY K
    LINE	L AY N
    LOOKING	L UH K IH NG
    LOOKS	L UH K S
    LORD	L AO R D
    LOSING	L UW Z IH NG
    MEMORIES	M EH M ER IY Z
    MEN	M EH N
    MOMENT	M OW M AH N T
    MY	M AY
    NEED	N IY D
    NEEDED	N IY D AH D
    NEEDED(2)	N IY D IH D
    NOT	N AA T
    NOW	N AW
    OF	AH V
    ON	AA N
    ON(2)	AO N
    ONE	HH W AH N
    ONE(2)	W AH N
    ONLY	OW N L IY
    PARTICULAR	P AA T IH K Y AH L ER
    PARTICULAR(2)	P ER T IH K Y AH L ER
    PHIL	F IH L
    PHILIP	F IH L AH P
    PHILIP(2)	F IH L IH P
    PHYSIQUE	F AH Z IY K
    PLAYING	P L EY IH NG
    PROPOSED	P R AH P OW Z D
    RAILROAD	R EY L R OW D
    RIDGE	R IH JH
    RIFLESHOT	R AY F AH L SH AA T
    ROSE	R OW Z
    SEE	S IY
    SEEING	S IY IH NG
    SHARPLY	SH AA R P L IY
    SHOOK	SH UH K
    SHORTER	SH AO R T ER
    SHOVED	SH AH V D
    SINGLE	S IH NG G AH L
    STEELS	S T IY L Z
    SUPERLATIVE	S UH P ER L AH T IH V
    TABLE	T EY B AH L
    THAN	DH AE N
    THAN(2)	DH AH N
    THAT	DH AE T
    THAT(2)	DH AH T
    THE	DH AH
    THE(2)	DH IY
    THEM	DH AH M
    THEM(2)	DH EH M
    THERE	DH EH R
    THERE'S	DH EH R Z
    THIS	DH IH S
    TIME	T AY M
    TO	T AH
    TO(2)	T IH
    TO(3)	T UW
    TOM	T AA M
    TRAIL	T R EY L
    TURNED	T ER N D
    TURNS	T ER N Z
    TWENTIETH	T W EH N IY AH TH
    TWENTIETH(2)	T W EH N IY IH TH
    TWENTIETH(3)	T W EH N T IY AH TH
    TWENTIETH(4)	T W EH N T IY IH TH
    TWO	T UW
    WANT	W AA N T
    WANT(2)	W AO N T
    WAS	W AA Z
    WAS(2)	W AH Z
    WAS(3)	W AO Z
    WE	W IY
    WHAT	HH W AH T
    WHAT(2)	W AH T
    WHITTEMORE	HH W IH T M AO R
    WHITTEMORE(2)	W IH T M AO R
    WILL	W AH L
    WILL(2)	W IH L
    YOU	Y UW
    YOU'RE	Y UH R
    YOU'RE(2)	Y UW R
    YOUR	Y AO R
    YOUR(2)	Y UH R

#### Audio Files

This will use the audio files from [la teris. po'u lo tirxu cu vitke zi'o le barda tcadu](http://www.lojban.org/tiki/la%20teris.%20po%27u%20lo%20tirxu%20cu%20vitke%20zi%27o%20le%20barda%20tcadu "http://www.lojban.org/tiki/la%20teris.%20po%27u%20lo%20tirxu%20cu%20vitke%20zi%27o%20le%20barda%20tcadu").

### Precedure

From [Adapting the default acoustic model](http://cmusphinx.sourceforge.net/wiki/tutorialadapt "http://cmusphinx.sourceforge.net/wiki/tutorialadapt").

#### Generating acoustic feature files

    sphinx_fe -argfile hub4wsj_sc_8k/feat.params \
            -samprate 16000 -c arctic20.fileids \
           -di . -do . -ei wav -eo mfc -mswav yes

#### Converting the sendump and mdef files

    pocketsphinx_mdef_convert -text hub4wsj_sc_8k/mdef hub4wsj_sc_8k/mdef.txt

#### Accumulating observation counts

    ./bw \
     -hmmdir hub4wsj_sc_8k \
     -moddeffn hub4wsj_sc_8k/mdef.txt \
     -ts2cbfn .semi. \
     -feat 1s_c_d_dd \
     -svspec 0-12/13-25/26-38 \
     -cmn current \
     -agc none \
     -dictfn arctic20.dic \
     -ctlfn arctic20.fileids \
     -lsnfn arctic20.transcription \
     -accumdir .

#### Creating transformation with MLLR

    ./mllr_solve \
        -meanfn hub4wsj_sc_8k/means \
        -varfn hub4wsj_sc_8k/variances \
        -outmllrfn mllr_matrix -accumdir .

#### Updating the acoustic model files with MAP

    cp -a hub4wsj_sc_8k hub4wsj_sc_8kadapt

and

    ./map_adapt \
        -meanfn hub4wsj_sc_8k/means \
        -varfn hub4wsj_sc_8k/variances \
        -mixwfn hub4wsj_sc_8k/mixture_weights \
        -tmatfn hub4wsj_sc_8k/transition_matrices \
        -accumdir . \
        -mapmeanfn hub4wsj_sc_8kadapt/means \
        -mapvarfn hub4wsj_sc_8kadapt/variances \
        -mapmixwfn hub4wsj_sc_8kadapt/mixture_weights \
        -maptmatfn hub4wsj_sc_8kadapt/transition_matrices

#### Recreating the adapted sendump file

    ./mk_s2sendump \
        -pocketsphinx yes \
        -moddeffn hub4wsj_sc_8kadapt/mdef.txt \
        -mixwfn hub4wsj_sc_8kadapt/mixture_weights \
        -sendumpfn hub4wsj_sc_8kadapt/sendump

Dictionary
----------

There's no need to use any grapheme-to-phoneme (g2p) tool for Lojban. However, there are still many works to be done for dictionary. Here's an example for an English dictionary.

    BACKWARD	B AE K W ER D
    BROWSER	B R AW Z ER
    E-MAIL	IY M EY L
    FORWARD	F AO R W ER D
    LAST	L AE S T
    LAST(2)	L AO S T
    LAST(3)	L AE S
    MUSIC	M Y UW Z IH K
    NEW	N UW
    NEW(2)	N Y UW
    NEXT	N EH K S T
    NEXT(2)	N EH K S
    OPEN	OW P AH N
    PLAYER	P L EY ER
    WINDOW	W IH N D OW

Grammar/Language model
----------------------

### Grammar

Grammar model for CMUSphinx needs to be written in JSGF format. For example:

    #JSGF V1.0;
    /**
     * JSGF Grammar for Hello World example
     */
    grammar hello;
    public <greet> = (Good morning | Hello) ( Bhiksha | Evandro | Paul | Philip | Rita | Will );

### Language model

Language model seems really complex to generate. I'm still trying to understand it. Here's an example:

    Language model created by QuickLM on Sat Nov 15 03:11:02 EST 2014
    Copyright (c) 1996-2010 Carnegie Mellon University and Alexander I. Rudnicky

    The model is in standard ARPA format, designed by Doug Paul while he was at MITRE.

    The code that was used to produce this language model is available in Open Source.
    Please visit http://www.speech.cs.cmu.edu/tools/ for more information

    The (fixed) discount mass is 0.5. The backoffs are computed using the ratio method.
    This model based on a corpus of 7 sentences and 13 words

    \data\
    ngram 1=13
    ngram 2=18
    ngram 3=13

    \1-grams:
    -0.8873 </s> -0.3010
    -0.8873 <s> -0.2407
    -1.7324 BACKWARD -0.2407
    -1.7324 BROWSER -0.2407
    -1.7324 E-MAIL -0.2407
    -1.7324 FORWARD -0.2407
    -1.7324 LAST -0.2846
    -1.7324 MUSIC -0.2929
    -1.7324 NEW -0.2929
    -1.7324 NEXT -0.2846
    -1.4314 OPEN -0.2846
    -1.7324 PLAYER -0.2407
    -1.4314 WINDOW -0.2407

    \2-grams:
    -1.1461 <s> BACKWARD 0.0000
    -1.1461 <s> FORWARD 0.0000
    -1.1461 <s> LAST 0.0000
    -1.1461 <s> NEW 0.0000
    -1.1461 <s> NEXT 0.0000
    -0.8451 <s> OPEN 0.0000
    -0.3010 BACKWARD </s> -0.3010
    -0.3010 BROWSER </s> -0.3010
    -0.3010 E-MAIL </s> -0.3010
    -0.3010 FORWARD </s> -0.3010
    -0.3010 LAST WINDOW 0.0000
    -0.3010 MUSIC PLAYER 0.0000
    -0.3010 NEW E-MAIL 0.0000
    -0.3010 NEXT WINDOW 0.0000
    -0.6021 OPEN BROWSER 0.0000
    -0.6021 OPEN MUSIC 0.0000
    -0.3010 PLAYER </s> -0.3010
    -0.3010 WINDOW </s> -0.3010

    \3-grams:
    -0.3010 <s> BACKWARD </s>
    -0.3010 <s> FORWARD </s>
    -0.3010 <s> LAST WINDOW
    -0.3010 <s> NEW E-MAIL
    -0.3010 <s> NEXT WINDOW
    -0.6021 <s> OPEN BROWSER
    -0.6021 <s> OPEN MUSIC
    -0.3010 LAST WINDOW </s>
    -0.3010 MUSIC PLAYER </s>
    -0.3010 NEW E-MAIL </s>
    -0.3010 NEXT WINDOW </s>
    -0.3010 OPEN BROWSER </s>
    -0.3010 OPEN MUSIC PLAYER

    \end\

Get Involved
------------

If you are interested in a speech recognition tool for Lojban. Please just contact [sorpa'as](mailto:me@sorpaas.com "mailto:me@sorpaas.com").
