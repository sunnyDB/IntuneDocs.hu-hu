---
title: Vállalati azonosítók hozzáadása az Intune-hoz
titleSuffix: ''
description: Ismerje meg, hogyan adhat hozzá vállalati azonosítókat (regisztrációs módszer, IMEI és sorozatszámok) a Microsoft Intune-bA.
keywords: ''
author: ErikjeMS
ms.author: erikje
manager: dougeby
ms.date: 02/22/2018
ms.topic: conceptual
ms.prod: ''
ms.service: microsoft-intune
ms.localizationpriority: high
ms.technology: ''
ms.assetid: 566ed16d-8030-42ee-bac9-5f8252a83012
ms.reviewer: dagerrit
ms.suite: ems
search.appverid: MET150
ms.custom: seodec18
ms.collection: M365-identity-device-management
ms.openlocfilehash: 28ad1e492c4bdd7c87371611530cd3f8e2abc2e1
ms.sourcegitcommit: 1cae690ca2ac6cc97bbcdf656f54b31878297ae8
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59900961"
---
# <a name="identify-devices-as-corporate-owned"></a>Eszközök azonosítása vállalati tulajdonúként

[!INCLUDE [azure_portal](./includes/azure_portal.md)]

Intune-rendszergazdaként vállalati tulajdonúként azonosíthat eszközöket, így finomíthatja a felügyeletet és az azonosítást. Az Intune további felügyeleti feladatokat végezhet, valamint további adatokat gyűjthet, például a vállalati tulajdonú eszközök teljes telefonszámát és az eszközökön található alkalmazások leltárát. Eszközkorlátozásokat is beállíthat a nem vállalati tulajdonú eszközök regisztrációjának megakadályozásához.

A regisztrálás idején az Intune automatikusan vállalati tulajdonú státuszt rendel az eszközhöz, ha azt:

- [i](device-enrollment-manager-enroll.md) fiókkal regisztrálták (bármely platformon).
- Az Apple [Készülékregisztrációs programban](device-enrollment-program-enroll-ios.md), az [Apple School Managerrel](apple-school-manager-set-up-ios.md) vagy az [Apple Configuratorral](apple-configurator-enroll-ios.md) regisztrálták (csak iOS esetén).
- [Regisztráció előtt az eszközt céges eszközként azonosították](#identify-corporate-owned-devices-with-imei-or-serial-number) nemzetközi mobilkészülék-azonosító (IMEI-) számmal (minden IMEI-számmal rendelkező platform esetében) vagy sorozatszámmal (csak iOS és Android esetében)
- Csatlakozva az Azure Active Directoryhoz Windows 10 Enterprise-eszközként
- Vállalatiként van beállítva az [eszköz tulajdonságaiban](#change-device-ownership)

A regisztrálás után [a tulajdonosi beállítás módosítható](#change-device-ownership), és beállítható akár **Személyes**, akár **Vállalati** tulajdonra.

## <a name="identify-corporate-owned-devices-with-imei-or-serial-number"></a>Céges eszközök azonosítása IMEI- vagy sorozatszám alapján

Intune rendszergazdaként létrehozhat és importálhat egy vesszővel tagolt (.csv) fájlt 14 számjegy IMEI számokat vagy sorozatszámokat felsoroló. Az Intune ezeket az azonosítókat használva határozza meg a regisztráció során, hogy egy adott eszköz céges tulajdonú-e. IMEI-számot minden támogatott platformhoz megadhat. Sorozatszámot csak iOS, macOS és Android rendszerű eszközhöz adhat meg. Adminisztrációs célból minden IMEI-azonosítóhoz vagy sorozatszámhoz megadhat további adatokat is.

<!-- When you upload serial numbers for corporate-owned iOS devices, they must be paired with a corporate enrollment profile. Devices must then be enrolled using either Apple’s device enrollment program (DEP) or Apple Configurator to have them appear as corporate-owned. -->

[Ismerje meg, hogyan találhatja meg egy Apple-eszköz sorozatszámát](https://support.apple.com/HT204308).<br>
[Ismerje meg, hogyan találhatja meg a saját Apple-eszköz sorozatszámát](https://support.google.com/store/answer/3333000).

## <a name="add-corporate-identifiers-by-using-a-csv-file"></a>Vállalati azonosítók felvétele .csv-fájlból
Hozzon létre egy kétoszlopos, fejléc nélküli listát .csv formátumban. Adja hozzá a 14 számjegy IMEI és sorozatszámok a bal oldali oszlopban, és a részletek a jobb oldali oszlopban. Egy .csv-fájlból csak egyféle azonosítót (IMEI-azonosítót vagy sorozatszámot) importálhat. A részletes adatok maximális terjedelme 128 karakter, csak adminisztratív célt szolgálnak, és nem jelennek meg az eszközön. A .csv-fájlok jelenleg legfeljebb 5 000 sorosak lehetnek.

**Sorozatszámokat tartalmazó .csv-fájl feltöltése** – Hozzon létre egy vesszővel elválasztott, kétoszlopos, fejléc nélküli értéklistát (.csv), amelyben maximum 5,000 eszköz szerepel. A csv-fájl mérete nem haladja meg az 5 MB-ot.

|||
|-|-|
|&lt;1. azonosító&gt;|&lt;1. eszköz részletei&gt;|
|&lt;2. azonosító&gt;|&lt;2. eszköz részletei&gt;|

Ez a .csv- fájl egy szövegszerkesztőben megtekintve így jelenik meg:

```
01234567890123,device details
02234567890123,device details
```

> [!IMPORTANT]
> Egyes Android és IOS rendszerű eszközök több IMEI-számmal rendelkeznek. Az Intune regisztrált eszközönként csak egy IMEI-számot olvas be. Ha olyan IMEI-számot importál, de nem az Intune által leltározott IMEI, az eszköz akkor minősül helyett a vállalat által birtokolt eszközt. Ha egy eszközhöz több IMEI-számot importál, a leltárban nem szereplő számok **Ismeretlen** regisztrációs állapottal jelennek meg.<br>
>Azt is vegye figyelembe: Sorozatszámokat tartalmazó iOS-eszközök azonosítása ajánlott formájában.
>Androidos sorozatszámok egyediek vagy elérhetőek lesznek a nem garantált. Egyeztessen az eszköz szállítójával arról, hogy a sorozatszám tekinthető-e megbízható eszközazonosítónak.
>Elképzelhető, hogy az a sorozatszám, amelyet az eszköz az Intune felé jelez, nem egyezik meg az eszköz Android beállítások/Névjegy (Android Settings/About) menüjében megjelenő azonosítóval. Ellenőrizze, hogy az eszköz gyártója milyen típusú sorozatszámot tüntet fel itt.
>Ha a feltöltendő sorozatszámokból álló fájl pontokat (.) tartalmaz, a feltöltés sikertelen lesz. A pontokat tartalmazó sorozatszámok nem támogatottak.

### <a name="upload-a-csv-list-of-corporate-identifiers"></a>Céges azonosítók .csv-listájának feltöltése

1. Az [Azure Portalbeli Intune-ban](https://portal.azure.com) válassza az **Eszközök regisztrálása** > **Céges készülékazonosítók** > **Hozzáadás** > **CSV-fájl feltöltése** lehetőséget.

   ![A céges készülékazonosítók munkaterülete a Hozzáadás gomb kiemelésével](./media/add-corp-id.png)

2. Az a **azonosítók hozzáadása** panelen adja meg az azonosító típusát: **IMEI** vagy **soros**.

3. Kattintson a mappa ikonra, és adja meg az importálni kívánt lista elérési útját. Keresse meg a .csv-fájlt, és kattintson a **Hozzáadás** elemre. 

4. Ha a .csv-fájl az Intune-ban már meglévő céges azonosítókat tartalmaz, de más részletekkel, megjelenik az **Ismétlődő azonosítók áttekintése** előugró üzenet. Válassza ki azokat az azonosítókat, amelyeket szeretne felülírni az Intune-ban, majd válassza az **OK** gombot az azonosítók hozzáadásához. Az egyes azonosítóknál csak az első ismétlődés lesz összehasonlítva.

## <a name="manually-enter-corporate-identifiers"></a>A céges azonosítók manuális megadása

1. Az [Azure Portalbeli Intune-ban](https://portal.azure.com) válassza az **Eszközök regisztrálása** > **Céges készülékazonosítók** > **Hozzáadás** > **Beírás manuálisan** lehetőséget.

2. Az a **azonosítók hozzáadása** panelen adja meg az azonosító típusát: **IMEI** vagy **soros**.

3. Adja meg minden egyes azonosítóhoz, amelyet fel szeretne venni az **Azonosító** és a **Részletek** adatait. Az azonosítók megadásának befejezése után válassza a **Hozzáadás** gombot.

5. Ha az Intune-ban már meglévő céges azonosítókat adott meg, de más részletekkel, megjelenik az **Ismétlődő azonosítók áttekintése** előugró üzenet. Válassza ki azokat az azonosítókat, amelyeket szeretne felülírni az Intune-ban, majd válassza az **OK** gombot az azonosítók hozzáadásához. Az egyes azonosítóknál csak az első ismétlődés lesz összehasonlítva.

A **Frissítés** elemre kattintva megtekintheti az új eszközazonosítókat.

Az importált eszközöket nem mindig regisztrálja a rendszer. Emiatt az eszközök **Regisztrált** vagy **Nincs kapcsolat** állapotúak lehetnek. Utóbbi állapot azt jelenti, hogy az eszköz még **nem lépett kapcsolatba** az Intune szolgáltatással.

## <a name="delete-corporate-identifiers"></a>Céges azonosítók törlése

1. Az [Azure Portalbeli Intune-ban](https://portal.azure.com) válassza az **Eszközök regisztrálása** > **Céges készülékazonosítók** lehetőséget.
2. Válassza ki a törölni kívánt eszközazonosítókat, majd kattintson a **Törlés** gombra.
3. Hagyja jóvá a törlést.

Ha törli egy regisztrált eszköz céges azonosítóját, azzal nem változtatja meg az eszköz tulajdonosát. Az eszköz tulajdonosának módosításához nyissa meg az **Eszközök** panelt, válassza ki az eszközt, majd válassza a **Tulajdonságok** lehetőséget, és módosítsa az **Eszköz tulajdonosa** értékét.

## <a name="imei-specifications"></a>IMEI-azonosítók specifikációi
Az IMEI-azonosítók részletes specifikációját a [3GGPP TS 23.003](https://portal.3gpp.org/desktopmodules/Specifications/SpecificationDetails.aspx?specificationId=729) lapon találja.

## <a name="change-device-ownership"></a>Eszköz tulajdonosának módosítása

Az Intune-ban az eszközök tulajdonságai között mindegyik eszközbejegyzésnél szerepel a **Tulajdonos**. Rendszergazdaként megadhatja, hogy az adott eszköz **Személyes** vagy **Céges**.

**Eszköz tulajdonosának módosítása:**
1. Az [Azure Portalbeli Intune-ban](https://portal.azure.com) az **Eszközök** területen válassza ki az adott eszközt.
2. Válassza a **Tulajdonságok** lehetőséget.
3. Adja meg az **Eszköz tulajdonosa** beállításnál, hogy az eszköz **Személyes** vagy **Céges**.

   ![Az eszköz tulajdonságai képernyő az Eszközkategória és az Eszköz tulajdonosa beállításokkal](./media/device-properties.png)
