# Translator – Windows portable

**[Stáhnout aplikaci RC3 (ZIP, 178 MiB)](https://github.com/jppicasso/translator-releases/releases/download/v1.0.0-rc.3/Translator-1.0.0-rc.3-windows-x64-portable.zip)** · [Stránka vydání a soubory ke stažení](https://github.com/jppicasso/translator-releases/releases/tag/v1.0.0-rc.3)

Rozbalte celý ZIP a spusťte `Translator.exe`. Nabídka **Code → Download ZIP** stahuje jen soubory tohoto distribučního repozitáře, ne aplikaci. Soubor `.zip.sha256` slouží jen ke kontrole stažení.

RC3 opravuje ztrácení rámečku mimo monitor a překrývání přeložených bloků. Přidává pět pojmenovaných předvoleb s volitelnou polohou a Ctrl+kolečko pro velikost písma. Původní RC1 a RC2 zůstávají zachované pro návrat. Soukromá uživatelská data se nepřibalují.

Následující návod je i uvnitř ZIPu jako README.md; odkazy na soubory a složky označují obsah rozbalené aplikace.

---

# Translator 1.0 RC3 – portable pro Windows

Rozbalte celý ZIP do vlastní zapisovatelné složky a spusťte **Translator.exe**. Python ani připojení k internetu nejsou pro překlad potřeba. Přenášejte celou složku včetně `_internal`, `licenses` a `sources`.

## Ovládání

- **Vrátit rámeček na obrazovku** v ovládacím okně obnoví ztracený rámeček na monitor tohoto okna a odemkne ho. Nastavení a předvolby zůstanou zachované.
- Při načtení předvolby a při startu se celý rámeček včetně spodního ovládání vejde do pracovní plochy jednoho monitoru. Pokud by přesahoval okraj, posune se jen o nutnou vzdálenost; příliš velké rozměry se omezí pro daný monitor. Uložená předvolba se tím nepřepisuje. Pokud se rámeček vejde, jeho poloha se nemění.

- **Ctrl+Shift+H** skryje nebo zobrazí rámeček, včetně zámku. Zůstává na stejné pozici a zachová velikost, zámek i stav spuštění. Při skrytí se nové snímání pozastaví. Ovládací okno lze samostatně minimalizovat na hlavní panel.
- **Ctrl+Alt+T** zamyká a odemyká rámeček. Zamčený propouští kliknutí.
- Pravý tah za dolní pravý roh přesouvá rámeček; levý tah mění velikost.
- **▶ / Ⅱ** vlevo nahoře spouští a pozastavuje překlad; **↻** vynutí nové rozpoznání a překlad.
- **×** vpravo nahoře zavře celou aplikaci.
- **1–5**: pravý klik uloží rámeček, levý načte předvolbu. Volba **Ukládat do předvoleb i polohu rámečku** je výchozí zapnutá: ukládá a načítá rozměry i polohu. Po vypnutí se používá jen velikost a rámeček zůstane na aktuálním místě, pokud se vejde na monitor. Staré předvolby bez polohy ji získají až po novém uložení pravým kliknutím.
- **Názvy předvoleb** upravíte v pěti polích v ovládacím okně, např. NPC, Chat, Log, Titulky, Quest. Potvrďte Enterem nebo přejděte do jiného pole. Název (nejvýše 32 znaků) se objeví při najetí myší na číslo předvolby a pamatuje se po restartu. U úzkého rámečku se ovládání rozdělí do dvou řad.
- **A/M** přepíná automatické/ruční písmo; v M mění velikost kolečko nad ikonou nebo **Ctrl+kolečko nad překladem**. Běžné kolečko nad textem velikost nemění. Pokud je zvolená velikost pro blok příliš velká, text se stále zmenší, aby se vešel a nepřepsal okolní bloky.
- **S** zmrazí obraz a umožní zachytit překlad pomocí Win+Shift+S. Opětovné S pokračuje ve snímání.

Pokud je klávesová zkratka obsazená jiným programem, použijte odpovídající tlačítko v ovládacím okně. Angličtina → čeština je pevná dvojice. Opravování českého pravopisu lze zapnout/vypnout; nenahrazuje významovou korekturu.

Od RC2 používá portable verze CPU backend Ruy místo dřívějšího Intel MKL/OpenMP, aby bylo možné doložit podklady distribuce. RC3 ponechává stejné jádro i váhy překladového modelu. Na vývojovém PC trval opakovaný překlad celého zkušebního questu s kontextovým slovníkem a pravopisem přibližně 3 s oproti 0,6 s v dosavadní BAT verzi, bez času OCR. Rychlost závisí na PC a délce textu. BAT prostředí se touto distribucí nemění.

## Vlastní slovník

Veřejný balíček neobsahuje soukromá data konkrétní hry. Můžete připojit vlastní **JSON**, případně **TXT/TSV**, v kódování **UTF-8**. V ovládacím okně klikněte na **Připojit…** a soubor vyberte. Program si cestu zapamatuje. **Odpojit** zapomene cestu; váš soubor nesmaže.

Začněte úpravou přiloženého `examples/glossary.json`. Příklad JSON:

```json
{
  "format": 1,
  "entries": [
    {"source": "Captain Rowan", "kind": "name"},
    {"source": "Crystal Valley", "kind": "place"},
    {"source": "The Lost Map", "kind": "quest"}
  ]
}
```

- `source`: původní anglický název, který se zachová v překladu.
- `kind: "name"`: jméno postavy nebo jiný chráněný výraz, bez automatické závorky. Tento typ se použije i při vynechání `kind`.
- `kind: "place"`: místo; při zapnuté volbě českých překladů v závorkách může vzniknout `Crystal Valley (český překlad)`.
- `kind: "quest"`: název questu či úkolu, stejný způsob české poznámky. Nejdelší celý název má přednost před kratším názvem uvnitř.
- Volitelné `note`: vlastní český výklad, např. `{"source":"Crystal Valley","kind":"place","note":"Křišťálové údolí"}`. Má přednost před automatem.
- Volitelné `require_capital: true` vedle `format` požaduje u názvů začínajících velkým písmenem velké počáteční písmeno i v rozpoznaném textu. Jinak se velikost písmen při hledání nerozlišuje.

JSON nepovoluje komentáře ani čárku za posledním záznamem. Maximální velikost souboru je 20 MB; název může mít 200 znaků, vlastní poznámka 300 znaků. Po úpravě soubor znovu připojte nebo aplikaci restartujte.

Jednoduchý TXT obsahuje jeden chráněný název na řádek. Volitelný druhý sloupec je česká poznámka, třetí sloupec typ. Sloupce oddělujte skutečným **tabulátorem**, nikoli mezerami. Pro automatickou poznámku a typ `place` ponechte druhý sloupec prázdný, tedy napište dva tabulátory za názvem. Řádky začínající `#` jsou komentáře. Pro začátek je obvykle přehlednější JSON.

## Jednou přeložené české poznámky

Při zapnuté volbě **České překlady míst a questů v závorkách** vzniká poznámka až při prvním skutečném použití názvu. Trvale se ukládá **vedle vašeho slovníku**, např. `muj-slovnik.translations-cs.json`. Po restartu se použije stejná poznámka; Obnovit ji nepřekládá znovu.

Pokud model jen zopakuje anglický název, závorka se nepřidá. I tento prázdný výsledek se pamatuje. Automat může udělat významovou chybu. Soubor uložených poznámek můžete po ukončení aplikace upravit nebo odstranit záznam k novému překladu. Původní anglický slovník se za běhu nepřepisuje. Při chybě uložení program upozorní u slovníku a pokračuje v paměti.

Slovník i automatické poznámky jsou vaše místní soubory. Pro někoho dalšího sdílejte původní distribuční ZIP, pokud k němu nechcete přidávat vlastní data. Při použití dat z cizího webu dodržte jeho licenci a atribuci.

## Nastavení a přenos

Při aktualizaci rozbalte novou verzi do samostatné složky a při zavřených aplikacích do ní zkopírujte `data/` ze staré portable verze. Původní složku si ponechte pro návrat. Dosavadní tři předvolby se zachovají, čtvrtá a pátá jsou prázdné. Pokud stará předvolba obsahovala jen velikost, poloha se doplní až jejím novým uložením pravým kliknutím. Vlastní názvy předvoleb i volba ukládání polohy se pamatují.

Nastavení a logy aplikace jsou v `data/` vedle EXE. Při aktualizaci tuto složku zachovejte. Vývojová verze spouštěná přes BAT používá `%LOCALAPPDATA%\Translator\settings.json`; při zavřené aplikaci lze tento soubor zkopírovat do `data/settings.json` portable verze. Slovník zůstává na zapamatované cestě. Při přesunu na jiný PC jej případně připojte znovu.

## Licence a zdroje

Před používáním a dalším šířením se seznamte s `DISTRIBUTION-TERMS.txt`, `THIRD_PARTY.md` a složkou `licenses/`. Vlastní kód Translatoru je pod MIT; přibalené součásti mají vlastní licence. Použitím a dalším šířením přijímáte příslušné podmínky včetně podmínek pro Microsoft Distributable Code, omezených výhradně na tento kód.

Odpovídající zdroje **GEOS** a **Spylls** jsou ve `sources/`; zdrojová česká pravopisná data `.aff` a `.dic` s licencí jsou v `_internal/resources/spelling/cs/`. Podrobnosti k úpravě a nahrazení knihovny GEOS obsahuje `sources/README.md`. Tyto komponenty zůstávají oddělené a mohou být používány a upravovány podle svých licencí.

Balíček je pro Windows 10 2004+ / Windows 11 x64. Není digitálně podepsaný. Konkrétní chyba po spuštění bude zaznamenaná v `data/translator.log` nebo `data/error.log`.
