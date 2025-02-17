---
title: Intune-adattárház API-végpontja
titleSuffix: Microsoft Intune
description: A referencia-témakör ismerteti a Microsoft Intune Data Warehouse API URL-Címének szerkezete. Szűrő példák állnak rendelkezésre.
keywords: Intune-adattárház
author: Erikre
ms.author: erikre
manager: dougeby
ms.date: 02/25/2019
ms.topic: reference
ms.service: microsoft-intune
ms.localizationpriority: medium
ms.technology: ''
ms.assetid: A7A174EC-109D-4BB8-B460-F53AA2D033E6
ms.reviewer: aanavath
ms.suite: ems
search.appverid: MET150
ms.custom: intune-classic
ms.collection: M365-identity-device-management
ms.openlocfilehash: 122a7eef56f2850fced164a37b518f650ed2ecdf
ms.sourcegitcommit: 916fed64f3d173498a2905c7ed8d2d6416e34061
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 05/23/2019
ms.locfileid: "66045860"
---
# <a name="intune-data-warehouse-api-endpoint"></a>Intune-adattárház API-végpontja

Az Intune-adattárház API-t az adott szerepköralapú hozzáférés-vezérlőkkel és Azure AD-beli hitelesítő adatokkal rendelkező fiókokkal használhatja. Ezután az OAuth 2.0 segítségével hitelesíti majd REST-ügyfelét az Azure AD szolgáltatással. Végül pedig megalkotja az adattárház-erőforrás hívására szolgáló beazonosítható URL-címet.

[!INCLUDE [reports-credential-reqs](./includes/reports-credential-reqs.md)]

## <a name="authorization"></a>Engedélyezés

Az Azure Active Directory (Azure AD) az OAuth 2.0 használatával teszi lehetővé a webalkalmazásokhoz és webes API-khez való hozzáférés engedélyezését az Azure AD-bérlőben. Jelen útmutató nyelvektől független, és azt ismerteti, hogyan küldhetők és fogadhatók HTTP-üzenetek a nyílt forráskódú könyvtárak bármelyikének használata nélkül. Az OAuth 2.0 engedélyezési kódfolyamról bővebben az OAuth 2.0 ismertetőjének [4.1 szakaszában](https://tools.ietf.org/html/rfc6749#section-4.1) olvashat.

További információt az [Hozzáférés engedélyezése webes alkalmazásokhoz az OAuth 2.0 és az Azure Active Directory használatával](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code) című témakörben talál.

## <a name="api-url-structure"></a>API URL-címének szerkezete

Az adattárház API-végpontjai az egyes készletekhez tartozó entitásokat olvassák. Az API támogatja a **GET** HTTP-parancsot, valamint a lekérdezési beállítások alkészletét.

Az Intune-hoz tartozó URL-cím a következő formátumot használja:  
`https://fef.{location}.manage.microsoft.com/ReportingService/DataWarehouseFEService/{entity-collection}?api-version={api-version}`

> [!NOTE]
> A fenti URL-címben cserélje le a `{location}`, `{entity-collection}` és `{api-version}` értékeket az alábbi táblázatban szereplő információk alapján.

Az URL-cím a következő elemeket tartalmazza:

| Elem | Példa | Leírás |
|-------------------|------------|--------------------------------------------------------------------------------------------------------------------|
| location | msua06 | Az alap URL-cím helye az Azure Portalon található adattárház API paneljén látható. |
| entitásgyűjtemény | dátumok | Az OData-entitásgyűjtemény neve. Az adatmodellben lévő gyűjteményekről és entitásokról további információt a [Adatmodell](reports-ref-data-model.md) című témakörben talál. |
| api-verzió | béta | Verzió alatt az elérni kívánt API verzióját értjük. További információt a [Verzió](reports-api-url.md#api-version-information) című témakörben talál. |
| maxhistorydays | 7 | (Nem kötelező) Az előzmények bejegyzéseinek maximális lekérési időtartama (napokban kifejezve). Ez a paraméter bármely gyűjteménnyel használható, de csak olyan gyűjteményeknél lép érvénybe, amelyek tartalmazzák a `dateKey` értéket a kulcstulajdonság részeként. További információk: [DateKey típusú tartományszűrők](reports-api-url.md#datekey-range-filters). |

## <a name="api-version-information"></a>Az API-verzióra vonatkozó információk

Most már használható az Intune Adattárház v1.0-ás verziója az  `api-version=v1.0` lekérdezésparaméter megadásával. Az Adattárház gyűjteményeinek frissítései hozzáadó jellegűek, és nem okoznak fennakadást a meglévő forgatókönyvekben.

A bétaverzió használatával kipróbálhatja az adattárház legújabb funkcióit. A bétaverzió használatához URL-címének az  `api-version=beta` lekérdezési paraméterrel kell rendelkeznie. A bétaverzió olyan funkciókat is kínál, amelyek támogatott szolgáltatásként még érhetőek el általánosan. Amikor az Intune új funkciókkal bővíti a szolgáltatást, előfordulhat, hogy a bétaverzió viselkedése és az adategyezmény megváltozik. Frissítések esetén a bétaverziót használó egyéni kódok vagy jelentéskészítő eszközök működésében hibák jelentkezhetnek.

## <a name="odata-query-options"></a>Az OData-lekérdezés beállításai

A jelenlegi verzió a következő OData-lekérdezésparamétereket támogatja: `$filter`, `$select`, `$skip,` és `$top`. A `$filter`, csak `DateKey` vagy `RowLastModifiedDateTimeUTC` lehet, hogy támogatja az oszlopok vonatkoznak, és egyéb tulajdonságok hibás kérelem lép működésbe.

## <a name="datekey-range-filters"></a>DateKey típusú tartományszűrők

A `DateKey` tartományszűrők az adatletöltés korlátozására használhatók a `dateKey` kulcstulajdonsággal rendelkező egyes gyűjtemények esetén. A `DateKey` szűrő a szolgáltatás teljesítményének optimalizálására használható az alábbi `$filter` lekérdezési paraméter megadásával:

1.  A `DateKey` önállóan a `$filter` szűrűben. Támogatja az `lt/le/eq/ge/gt` operátorokat és az `and` logikai operátort, amelyet a kezdő dátum és/vagy a záró dátum leképezéséhez lehet használni.
2.  A `maxhistorydays` egyéni lekérdezési lehetőségként van megadva.<br>

## <a name="filter-examples"></a>Példák a szűrőkre

> [!NOTE]
> A szűrő példákban feltételezzük még ma a 2019/2/21.

|                             Szűrés                             |           A teljesítmény optimalizálása           |                                          Leírás                                          |
|:--------------------------------------------------------------:|:--------------------------------------------:|:---------------------------------------------------------------------------------------------:|
|    `maxhistorydays=7`                                            |    Összes                                      |    Olyan adatokat ad vissza, amelyekben a `DateKey` értéke 20180214 és 20180221 között van.                                     |
|    `$filter=DateKey eq 20180214`                                 |    Összes                                      |    Olyan adatokat ad vissza, amelyekben a `DateKey` értéke megegyezik a 20180214 értékkel.                                                    |
|    `$filter=DateKey ge 20180214 and DateKey lt 20180221`         |    Összes                                      |    Olyan adatokat ad vissza, amelyekben a `DateKey` értéke 20180214 és 20180220 között van.                                     |
|    `maxhistorydays=7&$filter=DateKey eq 20180214`                |    Összes                                      |    Olyan adatokat ad vissza, amelyekben a `DateKey` értéke megegyezik a 20180214 értékkel. A rendszer mellőzi a `maxhistorydays` értékét.                            |
|    `$filter=RowLastModifiedDateTimeUTC ge 2018-02-21T23:18:51.3277273Z`                                |    Összes                                       |    Vissza az adatokat `RowLastModifiedDateTimeUTC` nagyobb vagy egyenlő `2018-02-21T23:18:51.3277273Z`                             |
