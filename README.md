# Translator – Windows portable

**[Stáhnout aplikaci RC2 (ZIP, 178 MiB)](https://github.com/jppicasso/translator-releases/releases/download/v1.0.0-rc.2/Translator-1.0.0-rc.2-windows-x64-portable.zip)** · [Stránka vydání a soubory ke stažení](https://github.com/jppicasso/translator-releases/releases/tag/v1.0.0-rc.2)

Rozbalte celý ZIP a spusťte `Translator.exe`. Nabídka **Code → Download ZIP** stahuje jen soubory tohoto distribučního repozitáře, ne aplikaci. Soubor `.zip.sha256` slouží jen ke kontrole stažení.

Tento veřejný repozitář obsahuje distribuční balíčky, soukromá uživatelská data se nepřibalují. Doporučujeme RC2 s doplněnými licenčními podklady; starší RC1 je zachovaná pro návrat.

Následující návod je také uvnitř ZIPu jako README.md; odkazy na soubory a složky v něm označují obsah rozbalené aplikace.

---

# Translator 1.0 RC2 – portable pro Windows

Rozbalte celý ZIP do vlastní zapisovatelné složky a spusťte **Translator.exe**. Python ani připojení k internetu nejsou pro překlad potřeba. Přenášejte celou složku včetně `_internal`, `licenses` a `sources`.

## Ovládání

- **Ctrl+Shift+H** skryje nebo zobrazí rámeček, včetně zámku. Zůstává na stejné pozici a zachová velikost, zámek i stav spuštění. Při skrytí se nové snímání pozastaví. Ovládací okno lze samostatně minimalizovat na hlavní panel.
- **Ctrl+Alt+T** zamyká a odemyká rámeček. Zamčený propouští kliknutí.
- Pravý tah za dolní pravý roh přesouvá rámeček; levý tah mění velikost.
- **▶ / Ⅱ** vlevo nahoře spouští a pozastavuje překlad; **↻** vynutí nové rozpoznání a překlad.
- **×** vpravo nahoře zavře celou aplikaci.
- **1–3**: pravý klik uloží rozměry, levý je načte bez přesunu.
- **A/M** přepíná automatické/ruční písmo; v M mění velikost kolečko nad ikonou.
- **S** zmrazí obraz a umožní zachytit překlad pomocí Win+Shift+S. Opětovné S pokračuje ve snímání.

Pokud je klávesová zkratka obsazená jiným programem, použijte odpovídající tlačítko v ovládacím okně. Angličtina → čeština je pevná dvojice. Opravování českého pravopisu lze zapnout/vypnout; nenahrazuje významovou korekturu.

RC2 používá CPU backend Ruy místo dřívějšího Intel MKL/OpenMP, aby bylo možné doložit podklady distribuce. Váhy překladového modelu jsou stejné. Na vývojovém PC trval opakovaný překlad celého zkušebního questu s kontextovým slovníčkem a pravopisem přibližně 3 s oproti 0,6 s v dosavadní BAT verzi, bez času OCR. Rychlost závisí na PC a délce textu. BAT prostředí se touto distribucí nemění.

## Vlastní slovníček

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

Při zapnuté volbě **České překlady míst a questů v závorkách** vzniká poznámka až při prvním skutečném použití názvu. Trvale se ukládá **vedle vašeho slovníčku**, např. `muj-slovnik.translations-cs.json`. Po restartu se použije stejná poznámka; Obnovit ji nepřekládá znovu.

Pokud model jen zopakuje anglický název, závorka se nepřidá. I tento prázdný výsledek se pamatuje. Automat může udělat významovou chybu. Soubor uložených poznámek můžete po ukončení aplikace upravit nebo odstranit záznam k novému překladu. Původní anglický slovníček se za běhu nepřepisuje. Při chybě uložení program upozorní u slovníčku a pokračuje v paměti.

Slovníček i automatické poznámky jsou vaše místní soubory. Pro někoho dalšího sdílejte původní distribuční ZIP, pokud k němu nechcete přidávat vlastní data. Při použití dat z cizího webu dodržte jeho licenci a atribuci.

## Nastavení a přenos

Nastavení a logy aplikace jsou v `data/` vedle EXE. Při aktualizaci tuto složku zachovejte. Vývojová verze spouštěná přes BAT používá `%LOCALAPPDATA%\Translator\settings.json`; při zavřené aplikaci lze tento soubor zkopírovat do `data/settings.json` portable verze. Slovníček zůstává na zapamatované cestě. Při přesunu na jiný PC jej případně připojte znovu.

## Licence a zdroje

Před používáním a dalším šířením se seznamte s `DISTRIBUTION-TERMS.txt`, `THIRD_PARTY.md` a složkou `licenses/`. Vlastní kód Translatoru je pod MIT; přibalené součásti mají vlastní licence. Použitím a dalším šířením přijímáte příslušné podmínky včetně podmínek pro Microsoft Distributable Code, omezených výhradně na tento kód.

Odpovídající zdroje **GEOS** a **Spylls** jsou ve `sources/`; zdrojová česká pravopisná data `.aff` a `.dic` s licencí jsou v `_internal/resources/spelling/cs/`. Podrobnosti k úpravě a nahrazení knihovny GEOS obsahuje `sources/README.md`. Tyto komponenty zůstávají oddělené a mohou být používány a upravovány podle svých licencí.

Balíček je pro Windows 10 2004+ / Windows 11 x64. Není digitálně podepsaný. Konkrétní chyba po spuštění bude zaznamenaná v `data/translator.log` nebo `data/error.log`.
