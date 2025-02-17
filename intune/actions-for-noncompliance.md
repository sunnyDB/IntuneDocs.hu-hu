---
title: Meg nem felelés esetén használható üzenetek és műveletek az Azure-beli Microsoft Intune-ban | Microsoft Docs
description: Értesítési e-mailt hozhat létre, és elküldheti azt a nem megfelelő eszközökre. Miután az eszköz nem megfelelőként lett megjelölve, hozzáadhat olyan műveleteket, mint a türelmi időszak kijelölése a megfelelőség teljesítéséig, vagy egy ütemterv, amely az eszköz megfelelővé válásáig letiltja a hozzáférést. Mindezt megteheti az Azure-beli Microsoft Intune használatával.
keywords: ''
author: MandiOhlinger
ms.author: mandia
manager: dougeby
ms.date: 05/01/2019
ms.topic: conceptual
ms.service: microsoft-intune
ms.localizationpriority: high
ms.technology: ''
ms.suite: ems
search.appverid: MET150
ms.custom: intune-azure
ms.collection: M365-identity-device-management
ms.openlocfilehash: bf808a9a7f5a801997f37bd2ecf4c13e3823c332
ms.sourcegitcommit: 4b83697de8add3b90675c576202ef2ecb49d80b2
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 06/13/2019
ms.locfileid: "67044800"
---
# <a name="automate-email-and-add-actions-for-noncompliant-devices-in-intune"></a>Automatizált e-mailek és műveletek nem megfelelő eszközök hozzáadása az Intune-ban

A megfelelőségi szabályzatok és a szabályok nem megfelelő eszközök esetében is hozzáadhat **meg nem felelési műveletek**. Ez a szolgáltatás olyan műveletsorozatot, műveletek, például az e-mailben a végfelhasználó számára, és további konfigurálja.

## <a name="overview"></a>Áttekintés

Alapértelmezés szerint az Intune a nem megfelelő eszköz észlelése után azonnal nem megfelelőként jelöli meg azt. Az Azure Active Directory (AD) [feltételes hozzáférési](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) majd letiltja az eszközt. Ha egy eszköz nem megfelelő, **nem felelés esetén végrehajtandó műveletet** is dönthet arról, hogy mi a teendő rugalmas alkalmazáskezelést nyújt. Például nem kell azonnal letiltani az eszközt, hanem türelmi időszakot is meghatározhat az eszköz megfelelőségének visszaállításáig.

Többféle művelet használható:

- **E-mail küldése a végfelhasználónak**: Testre szabhatja az e-mail-értesítést, mielőtt elküldené a végfelhasználónak. Testre szabhatja az e-mail címzettjeit és tárgyát, az üzenet szövegét, a céges emblémát és a kapcsolattartási adatokat.

    Az Intune a nem megfelelő eszköz adatait is szerepelteti az értesítésben.

- **A nem megfelelő eszköz távoli zárolása**: A szabályzatoknak nem megfelelő eszközök esetén a távoli zárolás adhat ki. A felhasználótól az eszköz PIN-kódot vagy jelszót fog kérni az eszköz feloldásához. További információ a [Távoli zárolás](device-remote-lock.md) funkcióról. 

- **Eszköz megjelölése nem megfelelőként**: Ütemezés létrehozása (a napok száma) után az eszköz akkor nem megfelelőként megjelölve. A műveletet konfigurálhatja azonnali kezdettel, de meghatározhat egy türelmi időszakot is a megfelelőséghez.

Ez a cikk a következőkhöz nyújt útmutatást:

- Üzenetalapú értesítés sablonjának létrehozása
- Művelet létrehozása nem megfelelőségi esetekhez, például e-mail küldése vagy eszköz távoli zárolása


## <a name="before-you-begin"></a>Előkészületek

- A meg nem felelés esetén végrehajtandó műveletek beállításához legalább egy eszközmegfelelőségi szabályzatra van szükség. Eszközmegfelelőségi szabályzat létrehozásához tekintse meg a következő platformokat:

  - [Android](compliance-policy-create-android.md)
  - [Androidos munkahelyi profilok](compliance-policy-create-android-for-work.md)
  - [iOS](compliance-policy-create-ios.md)
  - [macOS](compliance-policy-create-mac-os.md)
  - [Windows](compliance-policy-create-windows.md)

- Az eszközmegfelelőségi szabályzatok használata az eszközök céges erőforrásokhoz, amikor az Azure AD feltételes hozzáférési be kell állítania. Lásd: [feltételes hozzáférés az Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal) vagy [az Intune-nal feltételes hozzáférés használatának szokásos módjai](conditional-access-intune-common-ways-use.md) útmutatást.

## <a name="create-a-notification-message-template"></a>Értesítési üzenetsablon létrehozása

Ha e-mailt szeretne küldeni a felhasználóknak, hozzon létre egy értesítési üzenetsablont. Ha egy eszköz nem megfelelő, a sablonban megadott adatok a felhasználóknak küldött e-mail tetején jelennek meg.

1. Jelentkezzen be a [Intune](https://go.microsoft.com/fwlink/?linkid=2090973).
2. Válassza az **Eszközmegfelelőség** > **Értesítések** lehetőséget.
3. Válassza az **Értesítés létrehozása** lehetőséget. Adja meg a következő információkat:

   - **Name (Név)**
   - **Tárgy**
   - **Üzenet**
   - **E-mail fejléce – a cég emblémájának megjelenítése**
   - **E-mail lábléce – a cég emblémájának megjelenítése**
   - **E-mail lábléce – a kapcsolatfelvételi adatok megjelenítése**

   ![Megfelelőségről szóló értesítési üzenetminta az Intune-ban](./media/actionsfornoncompliance-1.PNG)

4. Ha megadta a szükséges információkat, válassza a **Létrehozás** elemet. Az értesítési üzenet sablonja mostantól használható. Az emblémát a céges portál védjegyezése részeként feltöltött használható e-mail-sablonokat. A Céges portál arculatának kialakításáról a [Vállalati identitási arculat testreszabása](company-portal-app.md#company-identity-branding-customization) című cikk nyújt bővebb tájékoztatást.

> [!NOTE]
> Emellett módosíthatja vagy egy meglévő, korábban létrehozott értesítési sablon frissítését.

## <a name="add-actions-for-noncompliance"></a>Meg nem felelés esetén végrehajtandó műveletek hozzáadása

Eszközmegfelelőségi szabályzat létrehozásakor az Intune automatikusan létrehoz egy meg nem felelés esetén végrehajtandó műveletet. Ha egy eszköz nem teljesíti a megfelelőségi szabályzatot, ezzel a művelettel az eszköz nem megfelelőként jelöli meg. Testre szabhatja, hogy az eszköz meddig maradjon nem megfelelőként megjelölve. Ezt a műveletet nem lehet eltávolítani.

További műveletet akkor vehet fel, ha megfelelőségi szabályzatot hoz létre, vagy frissít egy meglévő szabályzatot. 

1. Jelentkezzen be a [Intune](https://go.microsoft.com/fwlink/?linkid=2090973) válassza **eszközmegfelelőség**.
2. Kattintson a **Szabályzatok** elemre, válassza ki az egyik szabályzatot, majd kattintson a **Tulajdonságok** elemre. 

    Még nincs szabályzata? Létrehozhat egy új szabályzatot [Android](compliance-policy-create-android.md), [iOS](compliance-policy-create-ios.md), [Windows](compliance-policy-create-windows.md), vagy más platformokon.
  
    > [!NOTE]
    > A JAMF-eszközök és az eszközcsoportok segítségével megcélzott eszközök jelenleg nem képesek megfelelőségi műveleteket fogadni.

3. Válassza a **Meg nem felelés esetén végrehajtandó műveletek** > **Hozzáadás** lehetőséget.
4. Válassza ki a **műveletet**: 

    - **E-mail küldése a végfelhasználóknak**: Amikor az eszköz nem megfelelő, válassza ki a felhasználó e-mail-címre. Ezenkívül: 
    
         - Válassza ki a korábban létrehozott **üzenetsablont**
         - Csoportok kiválasztásával adjon meg esetleges **további címzetteket**
    
    - **A nem megfelelő eszköz távoli zárolása**: Ha az eszköz nem megfelelő, akkor zárolja az eszközt. Ez a művelet kényszeríti a felhasználónak meg kell adnia a PIN-kódot vagy jelszót az eszköz zárolásának feloldásához. 
    
5. Konfigurálja a **ütemezés**: Adja meg a nap (0 és 365 között), a felhasználók eszközein a műveletet nem megfelelő állapot kezdete után. A türelmi időszak után kényszerítheti a feltételes hozzáférési szabályzat. Ha megad **0** (nulla) számot nap, akkor a feltételes hozzáférés érvénybe **azonnal**. Például azonnal letilthatja a vállalati erőforrásokhoz való hozzáférést egy eszköz meg nem felelése esetén.

6. Ha elkészült, kattintson a **Hozzáadás** > **OK** elemre a módosítások mentéséhez.

## <a name="next-steps"></a>További lépések

[A szabályzatok figyelése a](compliance-policy-monitor.md).
