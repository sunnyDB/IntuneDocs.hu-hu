---
title: Alkalmazáskonfigurációs szabályzatok hozzáadása felügyelt Android-eszközökhöz
titleSuffix: Microsoft Intune
description: A Microsoft Intune alkalmazáskonfigurációs szabályzataival beállításokat adhat meg a felhasználók által futtatott, androidos munkahelyi profilt használó alkalmazásokhoz.
keywords: ''
author: Erikre
ms.author: erikre
manager: dougeby
ms.date: 02/21/2019
ms.topic: conceptual
ms.prod: ''
ms.service: microsoft-intune
ms.localizationpriority: high
ms.technology: ''
ms.assetid: d0b6f3fe-2bd4-4518-a6fe-b9fd115ed5e0
ms.reviewer: chrisbal
ms.suite: ems
search.appverid: MET150
ms.custom: intune-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: dccbfe597fa4bd461bb71cb86d38ffdfd52d719a
ms.sourcegitcommit: 1cae690ca2ac6cc97bbcdf656f54b31878297ae8
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59896423"
---
# <a name="add-app-configuration-policies-for-managed-android-devices"></a>Alkalmazáskonfigurációs szabályzatok hozzáadása felügyelt Android-eszközökhöz

[!INCLUDE [azure_portal](./includes/azure_portal.md)]

Alkalmazáskonfigurációs szabályzatok használata a Microsoft Intune-ban az androidos munkahelyi profilt használó alkalmazások beállításainak megadásához. Az alkalmazásfejlesztőnek közzé kell tennie az Androidos kezelt alkalmazás konfigurációs beállításait az alkalmazás konfigurációs beállításainak megadásához. Rendelje hozzá az alkalmazáskonfigurációs szabályzatot ahhoz a felhasználói csoporthoz, amelyre alkalmazni szeretné a beállításokat.  A szabályzatbeállítások akkor használatosak, amikor egy alkalmazás keresi azokat (általában az első futtatáskor).

> [!Note]  
> Az alkalmazáskonfigurációt nem minden alkalmazás támogatja. Érdeklődjön az alkalmazás fejlesztőjénél, hogy az alkalmazás az alkalmazáskonfigurációs szabályzatok támogatásához készült-e.

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Válassza a **Minden szolgáltatás** > **Intune** lehetőséget. Az Intune a **Figyelés + felügyelet** szakaszban található.
3. Válassza az **Ügyfélalkalmazások** tevékenységprofilt.
4. Válassza az **Alkalmazáskonfigurációs szabályzatok** lehetőséget a **Felügyelet** csoportban, majd a **Hozzáadás** lehetőséget.
5. Adja meg a következő adatokat:
    - **Név** – Az Azure Portalon megjelenítendő profilnév.
    - **Leírás** – Az Azure Portalon megjelenítendő profilleírás.
    - **Eszközregisztráció típusa** – Válassza a **Felügyelt eszközök** lehetőséget.
6. A **Platform** beállításban válassza az **Android** lehetőséget.
7. Válassza a **Társított alkalmazás** lehetőséget azon alkalmazás kiválasztásához, amelyhez a konfigurációs szabályzatot definiálni szeretné. Válassza ki a listából azokat az androidos munkahelyi profilt használó alkalmazásokat, amelyeket jóváhagyott és szinkronizált az Intune-nal.
8. Válassza az **Engedélyek** lehetőséget. A konfigurációkat a következőkkel adhatja meg:
    - [A konfigurációtervező](#use-the-configuration-designer)
    - [A JSON-szerkesztő](#enter-the-json-editor)
9. Válassza az **OK**, majd a **Hozzáadás** gombot.

## <a name="use-the-configuration-designer"></a>A konfigurációtervező használata

Használhatja a configuration designer Android-alkalmazások esetén, ha az alkalmazás támogatja a konfigurációs beállításokat úgy lett kialakítva. A konfiguráció az Intune-ban regisztrált eszközökre fog vonatkozni. A tervezővel konkrét konfigurációs értékeket adhat meg az alkalmazás által közzétett beállításokhoz.

Az alkalmazáshoz megadni kívánt konfigurációs beállítások kiválasztásához válassza a **Hozzáadás** lehetőséget.  
A konfiguráció minden kulcsához és értékéhez állítsa be az alábbiakat:

  - **Érték típusa**  
    A konfigurációs érték adattípusa. Sztring értéktípusok esetében igény szerint megadhat egy változót vagy tanúsítványprofilt értéktípusnak.
  - **Konfigurációs érték**  
    A konfiguráció értéke. Ha értéktípusnak egy változót vagy tanúsítványt ad meg, a konfigurációs érték legördülő menüjében változók és tanúsítványprofilok közül választhat.  Ha egy tanúsítványt választ, az eszközre alkalmazott tanúsítványalias a futásidő során lesz feltöltve.
    
### <a name="supported-variables-for-configuration-values"></a>A konfigurációs értékek támogatott változói

Ha változót szeretne megadni értéktípusnak, az alábbi lehetőségek közül választhat:

| Beállítás | Példa |
|----|----|
| Mail | john@contoso.com |
| Egyszerű felhasználónév | john@contoso.com |
| Részleges egyszerű felhasználónév | János |
| Domain | contoso.com |
| Felhasználónév | John DoE-val |
| Fiókazonosító | fc0dc142-71d8-4b12-bbea-bae2a8514c81 |
| Felhasználói azonosító | 3ec2c00f-b125-4519-acf0-302ac3761822 |
| Eszköz azonosítója | b9841cd9-9843-405f-be28-b2265c59ef97 |

### <a name="allow-only-configured-organization-accounts-in-multi-identity-apps"></a>Csak a konfigurált szervezeti fiókok engedélyezése a többidentitásos alkalmazásokban 

Android-eszközök esetén használja az alábbi kulcs/érték párokat:

| **Kulcs** | com.microsoft.intune.mam.AllowedAccountUPNs |
|--------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Értékek** | <ul><li>Egy vagy több <code>;</code> karakterrel elválasztott egyszerű felhasználónév (UPN).</li><li>Csak az ezzel a kulccsal meghatározott, felügyelt felhasználói fiók(ok) engedélyezett(ek).</li><li> Az Intune-ban regisztrált eszközökhöz a <code>{{userprincipalname}}</code> token használható a regisztrált felhasználói fiók helyettesítésére.</li></ul> |

   > [!NOTE]
   > Ha csak a konfigurált, többszörös identitással rendelkező szervezeti fiókokat engedélyezi, az Outlook for Android 2.2.222 vagy újabb verzióját kell használnia.<p></p>
   > A Microsoft Intune rendszergazdájaként szabályozhatja, hogy melyik felhasználói fiókok legyenek hozzáadva a Microsoft Office-alkalmazásokhoz a felügyelt eszközökön. A hozzáférést korlátozhatja csak a szervezeti felhasználói fiókokra, és blokkolhatja a személyes fiókok használatát a regisztrált eszközökön. A támogató alkalmazások feldolgozzák az alkalmazáskonfigurációt, majd eltávolítják és letiltják a nem jóváhagyott fiókokat.<p></p>
   > A Microsoft Wordhöz, a Microsoft Excelhez és a Microsoft PowerPointhoz az alkalmazás 16.0.9327.1000-es vagy újabb verzióját kell használnia. 

## <a name="enter-the-json-editor"></a>A JSON-szerkesztő megnyitása

Az alkalmazások bizonyos konfigurációs beállításai (például a kötegtípussal rendelkezőkéi) a configuration designerrel nem állíthatók be. Ezekhez az értékekhez a JSON-szerkesztőt kell használnia. A beállítások megadása ezen alkalmazások számára az alkalmazás telepítésekor automatikusan történik.

1. A **Konfigurációs beállítások formátuma** beállításban válassza a **Belépés a JSON-szerkesztőbe** lehetőséget.
2. A szerkesztőben definiálhatja konfigurációs beállítások JSON-értékeit. Választhatja a **JSON-sablon letöltése** lehetőséget a mintafájl letöltéséhez, amelyet ezután konfigurálhat.
3. Válassza az **OK**, majd a **Hozzáadás** gombot.

Ekkor létrejön a szabályzat, és megjelenik a szabályzatok listáját tartalmazó panelen.

Amikor valamelyik eszközön sor kerül az alkalmazáskonfigurációs szabályzathoz rendelt alkalmazás futtatására, akkor a rendszer a szabályzatban szereplő beállításoknak megfelelően fogja futtatni azt.

## <a name="preconfigure-the-permissions-grant-state-for-apps"></a>Az engedélyek engedélyezési állapotának előzetes konfigurálása az alkalmazásokhoz

Az alkalmazások az Android-eszköz funkcióinak eléréséhez szükséges engedélyét is megadhatja előre. Alapértelmezés szerint az eszközengedélyeket (például a tartózkodási hely adataihoz vagy az eszköz kamerájához való hozzáférést) megkövetelő Android-alkalmazások felszólítják a felhasználókat az engedélyek elfogadására vagy elutasítására. Ha például egy alkalmazás az eszköz mikrofonját használja, a rendszer felszólítja a felhasználót, hogy adjon engedélyt az alkalmazásnak a mikrofon használatára.

1. Jelentkezzen be az [Azure Portalra](https://portal.azure.com).
2. Válassza a **Minden szolgáltatás** > **Intune** lehetőséget. Az Intune a **Figyelés + felügyelet** szakaszban található.
3. Válassza az **Ügyfélalkalmazások** lehetőséget.
3. A **Felügyelet** csoportban válassza az **Alkalmazáskonfigurációs szabályzatok** lehetőséget, majd a **Hozzáadás** lehetőséget.
4. Adja meg a következő adatokat:
    - **Név**. Az Azure Portalon megjelenő profilnév.
    - **Leírás**. Az Azure Portalon megjelenő profilleírás.
    - **Eszközregisztráció típusa**. Válassza a **Felügyelt eszközök** lehetőséget.
    - **Platform**. Válassza az **Android** lehetőséget.
5. Válassza a **Társított alkalmazás** lehetőséget azon alkalmazás kiválasztásához, amelyhez a konfigurációs szabályzatot definiálni szeretné. Válassza ki a listából azokat az androidos munkahelyi profilt használó alkalmazásokat, amelyeket jóváhagyott és szinkronizált az Intune-nal.
6. Válassza az **Engedélyek** majd **Hozzáadás** lehetőséget.
7. Válasszon a rendelkezésre álló alkalmazásengedélyek listájából, és kattintson az **OK** gombra.
8. Válasszon beállítást az ehhez a szabályzathoz megadandó egyes engedélyekhez:
    - **Rákérdezés**. A felhasználó elfogadásra vagy elutasításra való felszólítása.
    - **Automatikus engedélyezés**. Automatikus jóváhagyás a felhasználó értesítése nélkül.
    - **Automatikus elutasítás**. Automatikus elutasítás a felhasználó értesítése nélkül.
9. Az alkalmazáskonfigurációs szabályzat hozzárendeléséhez jelölje ki a szabályzatot, és válassza a **Hozzárendelés**, majd a **Csoportok kiválasztása** lehetőséget.
10. Jelölje ki a hozzárendelendő felhasználói csoportokat, majd válassza a **Kijelölés** lehetőséget.
11. A szabályzat hozzárendeléséhez kattintson a **Mentés** gombra.

## <a name="next-steps"></a>További lépések

Ezt követően gondoskodhat az alkalmazás valamely csoporthoz történő [hozzárendeléséről](apps-deploy.md) és [figyeléséről](apps-monitor.md).

