# Especificacions de referència — IAB / DCP / Transport

Aquest document recull els estàndards oficials (SMPTE i ISDCF) rellevants per a
`CineIA_CLI`: des del format del bitstream IAB fins al seu transport en xarxa
cap a un processador de so de cinema (p. ex. Dolby CP950/CP950A).

Els documents originals són propietat de la SMPTE / ISDCF. Aquest fitxer
només enllaça a les fonts oficials i en resumeix l'abast — no en redistribueix
el contingut.

## 1. Format del bitstream IAB

- **SMPTE ST 2098-1:2018** — *Immersive Audio Metadata*
  https://pub.smpte.org/pub/st2098-1/st2098-1-2018.pdf

- **SMPTE ST 2098-2:2022** — *Immersive Audio Bitstream Specification*
  (revisió de l'ST 2098-2:2019; defineix el bitstream IAB en si —
  el que llegeix/genera `cineia.cpp`)
  https://pub.smpte.org/pub/st2098-2/st2098-2-2022.pdf
  Revisió pública en curs (Public Committee Draft): https://github.com/SMPTE/st2098-2

- **SMPTE ST 2098-5:2018** — *D-Cinema Immersive Audio Channels and Soundfield Groups*
  https://pub.smpte.org/pub/st2098-5/st2098-5-2018.pdf

- **SMPTE EG 2098-3:2020** — *Immersive Audio Renderer Expectations and Testing
  Recommendations* (guia d'enginyeria, no normativa)
  https://pub.smpte.org/latest/eg2098-3/eg2098-3-2020.pdf

- **SMPTE RDD 57:2021** — *IAB Application Profile 1* (la constricció concreta
  que implementa `CineIA_CLI` — límits de canals/objectes, bit depth, frame rate...)
  https://doi.org/10.5594/SMPTE.RDD57.2021

- **ISDCF Document 15** — nota tècnica sobre l'ST 2098-2 / IAB Profile 1
  https://files.isdcf.com/papers/ISDCF-Doc15-IAB-Profile-1-202006012.pdf

## 2. Empaquetat del bitstream en un fitxer MXF (DCP)

- **SMPTE ST 429-18:2019** — *D-Cinema Packaging — Immersive Audio Track File*
  Defineix com el bitstream IAB (ST 2098-2) es mapeja dins d'un fitxer MXF
  (Frame Wrapping, KLV, Essence Descriptor amb `MaxChannelCount`,
  `MaxObjectCount`, `FirstFrame`, `IAB Sample Rate`...). Aquesta és
  l'especificació que `main.cpp` implementa via `ASDCP::ATMOS::MXFWriter` /
  `AtmosDescriptor`.
  https://pub.smpte.org/doc/st429-18/20190603-pub/st429-18-2019.pdf

## 3. Transport en xarxa: servidor → processador de so

- **SMPTE ST 430-14:2015** — *D-Cinema Operations — Digital Sync Signal and
  Aux Data Transfer Protocol*
  Defineix (a) el protocol de transferència HTTP sobre LAN pel qual el
  processador (explícitament anomenat "Immersive Sound Processor" al
  document — categoria on encaixa el CP950/CP950A) demana els frames
  d'àudio immersiu al servidor, i (b) el senyal de sincronització AES/EBU
  que permet reproduir-los alineats amb la imatge.
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

## Nota sobre la interconnexió física real (RJ45)

El port físic "Dolby Atmos Connect" del CP950/CP950A i el seu suport per a
AES67/Blu-Link és documentació **de producte, pròpia de Dolby** — no un
estàndard SMPTE públic. Vegeu el manual oficial:
https://professional.dolby.com/siteassets/products/cp950a/dolby_cp950-cp950a_manual_issue_13.pdf
