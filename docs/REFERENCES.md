# Especificacions de referència — IAB / DCP / Transport

Aquest document recull els estàndards oficials (SMPTE i ISDCF) rellevants per a
`CineIA_CLI`: des del format del bitstream IAB fins al seu transport en xarxa
cap a un processador de so de cinema (p. ex. Dolby CP950/CP950A).

Els documents originals són propietat de la SMPTE / ISDCF. Els PDFs complets
es guarden a `docs/specs/` a petició explícita del mantenidor del repositori;
aquest fitxer n'enllaça també les fonts oficials per referència i verificació.

## 1. Format del bitstream IAB

- **SMPTE ST 2098-1:2018** — *Immersive Audio Metadata*
  [docs/specs/ST2098-1_2018_Immersive_Audio_Metadata.pdf](specs/ST2098-1_2018_Immersive_Audio_Metadata.pdf) ·
  https://pub.smpte.org/pub/st2098-1/st2098-1-2018.pdf

- **SMPTE ST 2098-2:2022** — *Immersive Audio Bitstream Specification*
  (revisió de l'ST 2098-2:2019; defineix el bitstream IAB en si —
  el que llegeix/genera `cineia.cpp`)
  [docs/specs/ST2098-2_2022_Immersive_Audio_Bitstream.pdf](specs/ST2098-2_2022_Immersive_Audio_Bitstream.pdf) ·
  https://pub.smpte.org/pub/st2098-2/st2098-2-2022.pdf
  Revisió pública en curs (Public Committee Draft): https://github.com/SMPTE/st2098-2

- **SMPTE ST 2098-5:2018** — *D-Cinema Immersive Audio Channels and Soundfield Groups*
  [docs/specs/ST2098-5_2018_Immersive_Audio_Channels.pdf](specs/ST2098-5_2018_Immersive_Audio_Channels.pdf) ·
  https://pub.smpte.org/pub/st2098-5/st2098-5-2018.pdf

- **SMPTE EG 2098-3:2020** — *Immersive Audio Renderer Expectations and Testing
  Recommendations* (guia d'enginyeria, no normativa)
  [docs/specs/EG2098-3_2020_Renderer_Testing.pdf](specs/EG2098-3_2020_Renderer_Testing.pdf) ·
  https://pub.smpte.org/latest/eg2098-3/eg2098-3-2020.pdf

- **SMPTE RDD 57:2021** — *IAB Application Profile 1* (la constricció concreta
  que implementa `CineIA_CLI` — límits de canals/objectes, bit depth, frame rate...)
  [docs/specs/RDD57_2021_IAB_Application_Profile_1.pdf](specs/RDD57_2021_IAB_Application_Profile_1.pdf) ·
  https://doi.org/10.5594/SMPTE.RDD57.2021

- **ISDCF Document 15** — nota tècnica sobre l'ST 2098-2 / IAB Profile 1
  [docs/specs/ISDCF-Doc15_IAB_Profile_1.pdf](specs/ISDCF-Doc15_IAB_Profile_1.pdf) ·
  https://files.isdcf.com/papers/ISDCF-Doc15-IAB-Profile-1-202006012.pdf

## 2. Empaquetat del bitstream en un fitxer MXF (DCP)

- **SMPTE ST 429-18:2019** — *D-Cinema Packaging — Immersive Audio Track File*
  Defineix com el bitstream IAB (ST 2098-2) es mapeja dins d'un fitxer MXF
  (Frame Wrapping, KLV, Essence Descriptor amb `MaxChannelCount`,
  `MaxObjectCount`, `FirstFrame`, `IAB Sample Rate`...). Aquesta és
  l'especificació que `main.cpp` implementa via `ASDCP::ATMOS::MXFWriter` /
  `AtmosDescriptor`.
  [docs/specs/ST429-18_2019_Immersive_Audio_Track_File.pdf](specs/ST429-18_2019_Immersive_Audio_Track_File.pdf) ·
  https://pub.smpte.org/doc/st429-18/20190603-pub/st429-18-2019.pdf

## 3. Transport en xarxa: servidor → processador de so

- **SMPTE ST 430-14:2015** — *D-Cinema Operations — Digital Sync Signal and
  Aux Data Transfer Protocol*
  Defineix (a) el protocol de transferència HTTP sobre LAN pel qual el
  processador (explícitament anomenat "Immersive Sound Processor" al
  document — categoria on encaixa el CP950/CP950A) demana els frames
  d'àudio immersiu al servidor, i (b) el senyal de sincronització AES/EBU
  que permet reproduir-los alineats amb la imatge.
  [docs/specs/ST430-14_2015_Sync_Aux_Data_Transfer.pdf](specs/ST430-14_2015_Sync_Aux_Data_Transfer.pdf) ·
  https://pub.smpte.org/doc/st430-14/20150801-pub/st0430-14-2015.pdf

- **SMPTE ST 430-10** — *D-Cinema Operations — Auxiliary Content
  Synchronization (ACS) Protocol* — defineix el "Digital Cinema Server" (DCS)
  i el protocol pel qual el processador descobreix l'adreça de xarxa del
  servidor abans de començar a demanar dades (referenciat per l'ST 430-14,
  no s'ha pogut recuperar un enllaç PDF d'accés lliure).

- **SMPTE ST 429-14:2014** — *D-Cinema Packaging — Aux Data Track File*
  Format genèric de "fitxer de dades auxiliars" del qual l'Immersive Audio
  Track File (ST 429-18) n'és una especialització (referenciat per
  l'ST 430-14, no s'ha pogut recuperar un enllaç PDF d'accés lliure).

### 3.1 Resta de la sèrie ST 430 (Operacions) — protocols de xarxa/TCP intra-teatre

Aquesta família cobreix tota la comunicació en xarxa entre els sistemes d'un
cinema (servidor, gestor de pantalla SMS, dispositius de seguretat...). No és
específica d'àudio immersiu, però és la família on realment apareix **TCP**
de forma explícita.

- **SMPTE ST 430-10:2010** — *Auxiliary Content Synchronization (ACS) Protocol*
  Confirma explícitament l'ús de TCP: *"The Control Protocol is a TCP-based
  protocol... The DCS shall listen for a connection on TCP port 4170."*
  [docs/specs/ST430-10_2010_ACS_Protocol.pdf](specs/ST430-10_2010_ACS_Protocol.pdf) ·
  https://pub.smpte.org/latest/st430-10/st0430-10-2010.pdf

- **SMPTE ST 430-17:2022** — *SMS-OMB Communications Protocol Specification*
  Protocol de comunicació SMS↔OMB; especifica connexió TCP amb adreça IP i
  port configurables per TLS.
  [docs/specs/ST430-17_2022_SMS-OMB_Communications_Protocol.pdf](specs/ST430-17_2022_SMS-OMB_Communications_Protocol.pdf) ·
  https://pub.smpte.org/latest/st430-17/st0430-17-2022.pdf

- **SMPTE ST 430-6:2010** — *Auditorium Security Messages for Intra-Theater Communications*
  Missatges de seguretat entre dispositius del teatre, sobre socket TLS.
  [docs/specs/ST430-6_2010_Auditorium_Security_Messages.pdf](specs/ST430-6_2010_Auditorium_Security_Messages.pdf) ·
  https://pub.smpte.org/latest/st430-6/st0430-6-2010.pdf

- **SMPTE ST 430-2:2017** — *Digital Certificate*
  [docs/specs/ST430-2_2017_Digital_Certificate.pdf](specs/ST430-2_2017_Digital_Certificate.pdf) ·
  https://pub.smpte.org/latest/st430-2/st0430-2-2017.pdf

- **SMPTE ST 430-3:2012** — *Generic Extra-Theater Message Format*
  [docs/specs/ST430-3_2012_Generic_ExtraTheater_Message.pdf](specs/ST430-3_2012_Generic_ExtraTheater_Message.pdf) ·
  https://pub.smpte.org/latest/st430-3/st0430-3-2012.pdf

- **SMPTE ST 430-4:2008** — *Log Record Specification*
  [docs/specs/ST430-4_2008_Log_Record_Specification.pdf](specs/ST430-4_2008_Log_Record_Specification.pdf) ·
  https://pub.smpte.org/latest/st430-4/st0430-4-2008.pdf

- **SMPTE ST 430-9:2008** — *Key Delivery Bundle*
  [docs/specs/ST430-9_2008_Key_Delivery_Bundle.pdf](specs/ST430-9_2008_Key_Delivery_Bundle.pdf) ·
  https://pub.smpte.org/latest/st430-9/st0430-9-2008.pdf

- **SMPTE ST 430-15:2017** — *Facility List Message Exchange Protocol*
  [docs/specs/ST430-15_2017_Facility_List_Message_Exchange.pdf](specs/ST430-15_2017_Facility_List_Message_Exchange.pdf) ·
  https://pub.smpte.org/latest/st430-15/st0430-15-2017.pdf

- **SMPTE ST 430-16:2017** — *Extended Facility List Message*
  [docs/specs/ST430-16_2017_Extended_Facility_List_Message.pdf](specs/ST430-16_2017_Extended_Facility_List_Message.pdf) ·
  https://pub.smpte.org/latest/st430-16/st0430-16-2017.pdf

- **No recuperats** (URL d'accés lliure no localitzada): ST 430-1:2017 (Key
  Delivery Message), ST 430-5:2011 (Security Log Event Class), ST 430-11:2011
  (Auxiliary Resource Presentation List), ST 430-12:2014 (FSK Synchronization
  Protocol).

## 4. Predecessor propietari de l'IAB i perfil global de compatibilitat DCP

- **SMPTE RDD 29:2019** — *Dolby Atmos® Bitstream Specification*
  El bitstream propietari original de Dolby Atmos, anterior a l'estandardització
  IAB (ST 2098-2). Estructura pràcticament idèntica (`ATMOSFrame` en lloc
  d'`IAFrame`, `BedDefinition`, `ObjectDefinition`...) — és l'avantpassat
  directe del format que genera `CineIA_CLI`. No conté cap informació de
  transport de xarxa.
  [docs/specs/RDD29_2019_Dolby_Atmos_Bitstream.pdf](specs/RDD29_2019_Dolby_Atmos_Bitstream.pdf) ·
  https://pub.smpte.org/pub/rdd29/rdd29-2019.pdf

- **SMPTE RDD 28:2014** — *Dolby Atmos® Print Master File Specification*
  Defineix com s'emmagatzema l'àudio i les metadades Atmos abans d'empaquetar-les
  (el DCDM, el master previ al DCP). No conté informació de transport de xarxa.
  [docs/specs/RDD28_2014_Dolby_Atmos_Print_Master_File.pdf](specs/RDD28_2014_Dolby_Atmos_Print_Master_File.pdf) ·
  https://pub.smpte.org/pub/rdd28/rdd28-2014.pdf

- **SMPTE RDD 52:2020** — *SMPTE DCP Bv2.1 Application Profile*
  Requisits i constriccions perquè un DCP es pugui reproduir en el màxim de
  sistemes possibles arreu del món (el perfil "Bv2.1" habitual en distribució
  general). No conté informació de transport de xarxa.
  [docs/specs/RDD52_2020_DCP_Bv21_Application_Profile.pdf](specs/RDD52_2020_DCP_Bv21_Application_Profile.pdf) ·
  https://pub.smpte.org/pub/rdd52/rdd52-2020.pdf

- **SMPTE ER 1008:2022** — *Digital Cinema — Overview for the SMPTE 428, 429,
  430, 431, 432, and 433 Document Suites*
  Índex oficial de **tots** els estàndards de cinema digital SMPTE, organitzats
  per família (427 Seguretat, 428 Master de Distribució, 429 Empaquetat,
  430 Operacions, 431 Qualitat, 432 Processament de Font Digital,
  433 Tipus de Dades XML), més un annex de Registered Disclosure Documents
  (RDD) rellevants. Útil com a mapa per localitzar qualsevol altre document
  relacionat no llistat aquí.
  [docs/specs/ER1008_2022_Digital_Cinema_Document_Suites_Overview.pdf](specs/ER1008_2022_Digital_Cinema_Document_Suites_Overview.pdf) ·
  https://5253154.fs1.hubspotusercontent-na1.net/hubfs/5253154/SMPTE-ER-DigitalCinemaOverviewDocumentSuties.pdf

## Nota sobre la interconnexió física real (RJ45)

El port físic "Dolby Atmos Connect" del CP950/CP950A i el seu suport per a
AES67/Blu-Link és documentació **de producte, pròpia de Dolby** — no un
estàndard SMPTE públic. Vegeu el manual oficial:
https://professional.dolby.com/siteassets/products/cp950a/dolby_cp950-cp950a_manual_issue_13.pdf
