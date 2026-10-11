# Loga TV stanic pro kabelovou televizi Vodafone

Loga televizních stanic pro [Tvheadend](https://tvheadend.org/), pojmenovaná tak, aby je Tvheadend sám přiřadil ke kanálům digitální kabelové televize (DVB-C) Vodafone v České republice. Z Tvheadendu je pak přebírají i [Kodi](https://kodi.tv/) a další klienti. Repozitář vznikl v roce 2016 pro kabelovou televizi UPC Česká republika. Tuto síť dnes provozuje Vodafone Czech Republic a.s., právní nástupce společnosti UPC Česká republika, s.r.o.

[English version](#tv-channel-logos-for-vodafone-cable-tv)

## Podle jaké nabídky

Repozitář se řídí frekvenčním přehledem digitální kabelové televize Vodafone („Programová nabídka Vodafone TV pro TV přijímače s CI+ modulem a kartou“) platným od 8. 9. 2026:
<https://static.vodafone.cz/cp/docs/tv/frekvencni_prehled_digitalni_kabelove_televize.pdf>

Vodafone zveřejňuje nové verze přehledu na stejné adrese, odkaz proto může vést na novější verzi. Soubory se k této nabídce teprve přizpůsobují, podrobnosti jsou v části [Stav](#stav).

## Použití s Tvheadendem

1. Stáhněte repozitář do adresáře, který může číst uživatel, pod kterým Tvheadend běží, například:

   ```sh
   git clone https://github.com/BKPepe/czech-channels-icons.git /opt/czech-channels-icons
   ```

   Aktualizace je pak `git -C /opt/czech-channels-icons pull`.
2. Ve webovém rozhraní Tvheadendu přepněte „View level“ na „Advanced“ nebo „Expert“ a otevřete Configuration → General → Base, skupinu „Channel icon/Picon Settings“.
3. Nastavte a uložte:

   | Položka | Hodnota |
   |---|---|
   | Channel icon path | `file:///opt/czech-channels-icons/%C.png` |
   | Channel icon name scheme | No scheme |

   Za `%C` Tvheadend dosadí upravený název kanálu (viz [Jak vznikají názvy souborů](#jak-vznikají-názvy-souborů)). Cesta k adresáři nesmí obsahovat `+` ani `%`.
4. V Configuration → Channel / EPG → Channels označte kanály a klikněte na „Reset Icon“. Kanál, který už má uloženou ikonu, by si ji jinak ponechal. Totéž je potřeba po každé změně cesty, schématu nebo názvů souborů.
5. Sloupec „User icon“ v seznamu kanálů ukazuje, který soubor Tvheadend pro kanál hledá, například `file:///opt/czech-channels-icons/CT%201.png` (`%20` je mezera).

Poznámky:

- Kodi (doplněk Tvheadend HTSP Client) nic nastavovat nemusí, loga si stahuje z Tvheadendu. Po výměně souboru pod stejným názvem může být potřeba vymazat mezipaměť náhledů v Kodi.
- Pokud název kanálu obsahuje diakritiku, musí Tvheadend běžet s knihovnou glibc v locale UTF-8, jinak diakritiku převede jinak a soubor nenajde. V balíčku pro Debian stačí přidat `LANG=C.UTF-8` do `/etc/default/tvheadend`, kontejneru s glibc předejte `-e LANG=C.UTF-8`. Oficiální image na Alpine dává jiné soubory pro část názvů s diakritikou (například „Óčko“ nebo „Markíza“), buildy s knihovnou musl (například pro OpenWrt) pro všechny. V seznamu kanálů z Tvheadendu na síti Vodafone z 11. 10. 2026 jsou všechny názvy služeb bez diakritiky (například „CT 1“).
- V české lokalizaci Tvheadendu je přeložena jen část popisků. „No scheme“ se tam jmenuje „Žádné schéma“ a „Reset Icon“ „Resetuj ikonu“.

## Jak vznikají názvy souborů

Název souboru je název kanálu v Tvheadendu s příponou `.png`. Kanál, který nepřejmenujete, má název služby z vysílání. Ten se od názvu v přehledu Vodafonu může lišit, například ve vysílání je „Nova HD“ a v přehledu „NOVA HD“. S `%C` a „No scheme“ Tvheadend název upraví takto:

- diakritiku odstraní (Č → C, Ó → O),
- znaky `: < > | * ? ' " { } ~` nahradí podtržítkem `_`,
- `/` a `\` nahradí pomlčkou `-`,
- tečky na začátku názvu nahradí podtržítkem,
- mezery, velikost písmen a ostatní znaky (například `&`, `+` a `.`) ponechá.

| Název kanálu | Soubor |
|---|---|
| ČT 1 HD | `CT 1 HD.png` |
| Óčko HD | `Ocko HD.png` |
| CT :D HD / CT art HD | `CT _D HD - CT art HD.png` |
| Crime & Investigation | `Crime & Investigation.png` |
| Nick jr. | `Nick jr..png` |

Velikost písmen musí přesně sedět, protože souborové systémy v Linuxu ji rozlišují.

Tvheadend 4.2.2 a starší a vývojové verze 4.3 do června 2017 nahrazovaly podtržítkem i mezery. Pro ně je [vydání 0.1](https://github.com/BKPepe/czech-channels-icons/releases/tag/0.1) z roku 2017, které se už neaktualizuje. Pět souborů s „ _ “ v názvu (například `CT _D _ CT art.png`) bylo pojmenováno podle pravidla Tvheadendu z roku 2017, kdy se z `/` stávalo `_`. Dnešní Tvheadend místo toho píše „ - “, proto byly tyto soubory přejmenovány nebo smazány.

## Stav

Stav k 11. 10. 2026:

- Názvy kanálů jsou ověřené podle seznamu kanálů z Tvheadendu na síti Vodafone z 11. 10. 2026. Seznam pokrývá 28 z 32 muxů přehledu a má 164 televizních kanálů. Zbylé 4 muxy (562, 746, 770 a 778 MHz) zatím naskenované nejsou. Jejich 19 kanálů má předběžný název odvozený z přehledu.
- Ze 183 kanálů má 115 logo pod názvem, který Tvheadend hledá, a 68 logo chybí.
- Repozitář má 115 souborů s logy, pro každý kanál s logem jeden. Podle seznamu z Tvheadendu bylo 25 souborů přejmenováno a 7 zkopírováno pro kanály se stejným logem. Další 2 soubory dostaly předběžný název podle přehledu. Smazáno bylo 94 souborů, které žádný kanál nepoužívá.

Úplný seznam kanálů je v [CHANNELS.md](CHANNELS.md).

### Známé problémy

- `Leo TV HD.png` má příponu `.png`, ale je ve skutečnosti GIF.
- `CT _D HD - CT art HD.png` ukazuje loga ČT :D HD a ČT art HD pod sebou v jednom obrázku.

## Právní upozornění

Loga televizních stanic v tomto repozitáři nejsou mým dílem. Autorská práva a práva z ochranných známek k nim patří jejich vlastníkům, zpravidla provozovatelům příslušných stanic. Loga pocházejí z veřejně dostupných zdrojů. Soubory jsem přejmenoval, aby je Tvheadend přiřadil ke správným kanálům, a nanejvýš technicky upravil (poměrné zmenšení, formát souboru, bezeztrátová komprese). Na podobě log nic neměním.

Tento projekt není spojen se společností Vodafone Czech Republic a.s. (právní nástupce společnosti UPC Česká republika, s.r.o.) ani s žádnou televizní stanicí a nemá jejich podporu ani schválení. Názvy a loga slouží jen k označení stanic, které uživatel přijímá. Seznam kanálů vychází z programové nabídky Vodafone TV podle [frekvenčního přehledu](https://static.vodafone.cz/cp/docs/tv/frekvencni_prehled_digitalni_kabelove_televize.pdf), názvy souborů se k ní postupně přizpůsobují.

K logům neposkytuji žádnou licenci a ani ji poskytnout nemohu. Repozitář vznikl pro nekomerční zobrazení ikon kanálů v programech jako Tvheadend nebo Kodi. Kdo chce loga použít jinak, například komerčně, potřebuje souhlas jejich vlastníků.

Pokud jste vlastníkem práv k některému logu a nesouhlasíte s jeho zveřejněním, napište mi na <github@bloodkings.eu> nebo založte issue. Logo odstraním bez diskuse a bez zbytečného odkladu, nejpozději do 30 dnů od obdržení žádosti.

## Jak přispět

Pull requesty i issues jsou vítané.

- Nejvíc pomůžou chybějící loga (viz [CHANNELS.md](CHANNELS.md)) a skutečné názvy kanálů z muxů 562, 746, 770 a 778 MHz, které zatím naskenované nejsou. Ty najdete v Tvheadendu na síti Vodafone v Configuration → Channel / EPG → Channels a můžete je poslat třeba v issue. Podle nich se ověří předběžné názvy v [CHANNELS.md](CHANNELS.md).
- Název nového souboru odvoďte z názvu kanálu v Tvheadendu podle pravidel výše. Sloupec „User icon“ ho ukazuje přímo.
- Logo dodejte jako PNG s průhledným pozadím, ideálně z oficiálních materiálů stanice nebo z Wikimedia Commons se štítkem PD-textlogo nebo PD-shape. V pull requestu uveďte, odkud logo pochází.
- Loga neupravujte. Nepřebarvujte je, neořezávejte do nových tvarů, neskládejte více log do jednoho obrázku a nepřekreslujte je. Přípustné jsou jen technické úpravy (poměrné zmenšení, formát souboru, bezeztrátová komprese).
- Nepřidávejte loga ze zdrojů, které šíření zakazují nebo omezují (placené sady piconů, soubory „Non-free logo“ z Wikipedie, sady jen pro osobní použití).
- Do repozitáře nepatří playlisty IPTV, adresy streamů ani klíče.

Dotazy a připomínky posílejte na <github@bloodkings.eu>.

# TV channel logos for Vodafone cable TV

TV channel logos for [Tvheadend](https://tvheadend.org/), named so that Tvheadend assigns them to the channels of Vodafone's digital cable TV (DVB-C) in the Czech Republic on its own. [Kodi](https://kodi.tv/) and other clients then take them from Tvheadend. The repository started in 2016 for the cable TV of UPC Česká republika. Today the network is run by Vodafone Czech Republic a.s., the legal successor of UPC Česká republika, s.r.o.

## Which lineup

The repository follows Vodafone's digital cable TV channel overview ("Programová nabídka Vodafone TV pro TV přijímače s CI+ modulem a kartou") valid from 8 September 2026:
<https://static.vodafone.cz/cp/docs/tv/frekvencni_prehled_digitalni_kabelove_televize.pdf>

Vodafone publishes new versions of the overview at the same address, so the link may lead to a newer version. The files are still being aligned with this lineup, see [State](#state).

## Using it with Tvheadend

1. Download the repository into a directory that the user Tvheadend runs as can read, for example:

   ```sh
   git clone https://github.com/BKPepe/czech-channels-icons.git /opt/czech-channels-icons
   ```

   To update it later, run `git -C /opt/czech-channels-icons pull`.
2. In the Tvheadend web interface, set "View level" to "Advanced" or "Expert" and open Configuration → General → Base, group "Channel icon/Picon Settings".
3. Set and save:

   | Setting | Value |
   |---|---|
   | Channel icon path | `file:///opt/czech-channels-icons/%C.png` |
   | Channel icon name scheme | No scheme |

   Tvheadend replaces `%C` with the processed channel name (see [How file names are formed](#how-file-names-are-formed)). The directory path must not contain `+` or `%`.
4. In Configuration → Channel / EPG → Channels, select the channels and click "Reset Icon". Otherwise a channel that already has an icon stored keeps it. Do the same after every change of the path, the scheme or the file names.
5. The "User icon" column of the channel list shows which file Tvheadend looks for, for example `file:///opt/czech-channels-icons/CT%201.png` (`%20` is a space).

Notes:

- Kodi (the Tvheadend HTSP Client add-on) needs no setup, it downloads the logos from Tvheadend. After a file is replaced under the same name, Kodi's thumbnail cache may need to be cleared.
- If a channel name contains diacritics, Tvheadend has to run with glibc in a UTF-8 locale, otherwise it transliterates them differently and does not find the file. With the Debian package, add `LANG=C.UTF-8` to `/etc/default/tvheadend`. Pass `-e LANG=C.UTF-8` to a glibc-based container. The official Alpine image produces different file names for some names with diacritics (for example "Óčko" or "Markíza"), and musl builds (for OpenWrt, for example) do so for all of them. In the Tvheadend channel list on the Vodafone network from 11 October 2026, no service name has diacritics (for example "CT 1").

## How file names are formed

The file name is the channel name in Tvheadend plus `.png`. A channel you do not rename has the service name from the broadcast. It can differ from the name in Vodafone's overview, for example "Nova HD" on air and "NOVA HD" in the overview. With `%C` and "No scheme" Tvheadend

- removes diacritics (Č becomes C, Ó becomes O),
- replaces the characters `: < > | * ? ' " { } ~` with an underscore `_`,
- replaces `/` and `\` with a hyphen `-`,
- replaces leading dots with an underscore,
- keeps spaces, letter case and other characters (such as `&`, `+` and `.`).

| Channel name | File |
|---|---|
| ČT 1 HD | `CT 1 HD.png` |
| Óčko HD | `Ocko HD.png` |
| CT :D HD / CT art HD | `CT _D HD - CT art HD.png` |
| Crime & Investigation | `Crime & Investigation.png` |
| Nick jr. | `Nick jr..png` |

Letter case has to match exactly because Linux file systems are case-sensitive.

Tvheadend 4.2.2 and older, and 4.3 development builds before June 2017, also replaced spaces with underscores. [Release 0.1](https://github.com/BKPepe/czech-channels-icons/releases/tag/0.1) from 2017 is for them and is no longer updated. Five files with " _ " in the name (for example `CT _D _ CT art.png`) followed the Tvheadend rule of 2017, when `/` became `_`. Current Tvheadend writes " - " instead, so these files have been renamed or removed.

## State

As of 11 October 2026:

- The channel names are checked against a Tvheadend channel list on the Vodafone network from 11 October 2026. The list covers 28 of the 32 muxes of the overview and has 164 TV channels. The other 4 muxes (562, 746, 770 and 778 MHz) have not been scanned yet. Their 19 channels have a provisional name derived from the overview.
- Of the 183 channels, 115 have a logo under the name Tvheadend looks for and 68 have no logo yet.
- The repository has 115 logo files, one for each channel with a logo. Following the Tvheadend list, 25 files were renamed and 7 were copied for channels with the same logo. Two more files got a provisional name from the overview. The 94 files that no channel uses were removed.

The full channel list is in [CHANNELS.md](CHANNELS.md).

### Known issues

- `Leo TV HD.png` has the `.png` extension but is really a GIF.
- `CT _D HD - CT art HD.png` shows the ČT :D HD and ČT art HD logos one above the other in one image.

## Legal notice

The TV channel logos in this repository are not my work. Copyright and trademark rights in them belong to their respective owners, usually the operators of the channels. The logos come from publicly available sources. I renamed the files so that Tvheadend matches them to the right channels and at most adjusted them technically (proportional downscaling, file format, lossless compression). I do not alter the logos themselves.

This project is not affiliated with, endorsed by or approved by Vodafone Czech Republic a.s. (legal successor of UPC Česká republika, s.r.o.) or any TV channel. Channel names and logos are used only to identify the channels a viewer receives. The channel list is based on the Vodafone TV channel line-up as published in the [channel overview](https://static.vodafone.cz/cp/docs/tv/frekvencni_prehled_digitalni_kabelove_televize.pdf), and the file names are being aligned with it.

I do not grant, and cannot grant, any licence to the logos. This repository was made for non-commercial display of channel icons in software such as Tvheadend or Kodi. Anyone who wants to use the logos in another way, for example commercially, needs permission from their owners.

If you own the rights to a logo and object to it being published here, email <github@bloodkings.eu> or open an issue. I will remove it without discussion and without undue delay, at the latest within 30 days of receiving the request.

## Contributing

Pull requests and issues are welcome.

- The most useful contributions are the missing logos (see [CHANNELS.md](CHANNELS.md)) and the real channel names on the 562, 746, 770 and 778 MHz muxes, which have not been scanned yet. A Tvheadend on the Vodafone network shows them in Configuration → Channel / EPG → Channels, and you can send them in an issue, for example. They will be used to check the provisional names in [CHANNELS.md](CHANNELS.md).
- Derive the name of a new file from the channel name in Tvheadend using the rules above. The "User icon" column shows it directly.
- Provide the logo as a PNG with a transparent background, ideally from the channel's official material or from Wikimedia Commons with the PD-textlogo or PD-shape tag. Say in the pull request where the logo comes from.
- Do not alter logos. Do not recolour them, crop them into new shapes, combine several logos in one image or redraw them. Only technical changes are fine (proportional downscaling, file format, lossless compression).
- Do not add logos from sources that forbid or restrict redistribution (paid picon sets, "Non-free logo" files from Wikipedia, sets for personal use only).
- IPTV playlists, stream addresses and keys do not belong in this repository.

Questions and comments: <github@bloodkings.eu>
